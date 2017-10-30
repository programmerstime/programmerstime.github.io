---
layout: page

title: "Mikroserwis w Clojure &ndash; testy jednostkowe"
subheadline: "Microservices #3"

teaser: "Kolejny raz o naszym serwisie w Clojure z wykorzystaniem Compojure.
        Ostatnio zobaczyliśmy <a href='/twoj-pierwszy-rest-serwis-w-clojure'>jak stworzyć projekt
        pozwalający na zarządzanie zasobami książkowymi</a>.
            <br><br>
         Pokażę teraz w jaki sposób możemy wzbogacić go o niezbędne testy jednostkowe.
         W praktyce pisane równocześnie z implementacją. Zasługujące jednak na
         samodzielny wpis."

category: dev

tags:
  - clojure
  - dev
  - testowanie

image:
  thumb: "photo-clojure-testing_thumb.jpg"
  title: "photo-clojure-testing.jpg"

comments: true

#sidebar: right
---

## Przypomnienie

Podsumujmy, że nasz serwis obsługuje następujące endpointy:

<pre>
GET /books
GET /books/[id]
POST /books
PUT /books/[id]
DELETE /books/[id]
DELETE /books/[id]
</pre>


Definicje powyższych operacji zawarte są niemalże w zupełności w jednej funcji -- handler (url do githuba)
i polegają w dużej mierze na delegacji zapytania do bazy danych.
Wzbogaconego miejscami o obsługę przypadków brzegowych (jak np. brak elementu w bazie)
oraz nadawanie id naszym zasobom.


{% highlight clojure %}
(defroutes app-routes
           (context "/books" []
             (GET "/" [] (read-books))
             (POST "/" {body :body} (insert-new-book body))
             (context "/:id" [id]
               (GET "/" [] (read-book-by-id id))
               (PUT "/" {body :body} (update-existing-book id body))
               (DELETE "/" [] (delete-book id))))
           (route/not-found "Not Found"))
{% endhighlight %}


Rozsądnym wydaje się tutaj testowanie działania kodu z poziomu tegoż właśnie handlera.
Będziemy chcieli jednak uniknąć odwołania do prawdziwej bazy danych.

## Jak mockować zależności w Clojure?

Bibliotek do mockowania w Clojure znajdziemy całkiem sporo. Niektóre zdają się
być napisane ze znacznym rozmachem (jak np. midje), inne są tylko nakładkami na
standardowe mechanizmy jakie oferuje sam język.

Mowa tutaj o funkcji *with-redefs*, która pozwala zmienić w locie definicję danej funkcji.
W naszym przypadku to właśnie jej użyjemy.

Podejściem naszym będzie, by zdefiniować w klasie testowej, kolekcję *books-data*,
która będzie odpowiednio modyfikowana podczas wywołania fukcji bazodanowych.

Zgodnie z powyższym kod testów może mieć następującą strukturę:


{% highlight clojure %}
(deftest test-app
  (let [books-data (ref ())]
    (with-redefs
      [db/read-books (fn [] @books-data)
       db/read-book (fn [id] (filter #(= (% "id") id) @books-data))
       db/insert-book (fn [book] (dosync (alter books-data conj book)))
       ...]
      (testing "Pierwszy test" ... )
      (testing "Drugi test" ... )
      ...)))

{% endhighlight %}


Składnia jest następująca: po słowie kluczowym *with-redefs* podajemy listę funkcji wraz z ich nowym znaczeniem.
Następne argumenty to kod, który będzie objęty nowymi definicjami miast starych.

W tym przypadku *db/read-books* zwyczajnie zwróci zawartość zmiennej *books-data*,
podczas gdy *db/read-book* zwróci tylko te elementy na liście *books-data*, które posiadają podane id.
No i oczywiście *insert* podmienia *books-data* z listą o jeden element dłuższą.

Jako że modyfikuje referencję, to musi być wywołana w transakcji. Clojure posiada
elementy znakomicie ułatwiające programowanie wielowątkowe, takie jak np.
[*Software Transactional Memory*](https://clojure.org/reference/refs)

Podsumowując, zamiast odwoływać się do bazy danych, operacje delegujemy do lokalnej listy książek.


## Jak pisać testy?

Przy obecnym setupie, pisząc testy, mamy do wyboru różne podejścia.
Osobiście **jestem zwolennikiem krótkich, niezależnych i wyspecjalizowanych testów**.
Pewnie brakuje jeszcze kilku epitetów do uczynienia tej listy wyczerpującą.

Powinno być bezsprzecznie wiadomo jaką operację pokrywa dany test,
jakie są warunki wstępne i czego oczekujemy po tej operacji wykonaniu.

##### Testy jako dokumentacja

Pamiętać trzeba, że **testy to najlepsza z możliwych dokumentacji naszego systemu**.
Dlatego też powinny być najbardziej przejrzyste jak to tylko możliwe.

Przykład: litania operacji z powplatanymi asercjami nie będzie dobrą praktyką.

##### Niezależne

Co jeżeli testy korzytają ze wspólnego zasobu? Otóż nie powinno mieć to miejsca
w testach jednostkowych. Alternatywnie, w najgorszym wypadku, powinniśmy zadbać o to, żeby jego stan był resetowany.

I także w naszym przypadku - nie powinniśmy współdzielić naszego mocka do bazy
danych pomiędzy testami. Każdy powinien posiadać oddzielną instancję.

##### Z precyzyjnymi komunikatami

I wreszcie -- warto przyjrzeć się jakie komunikaty generują nasze testy w przypadku
niepowodzenia. Informacja zwrotna powinna być możliwie najbardziej precyzyjna.

## Nasz scenariusz testowy

To powiedziawszy podzielę się następującym scenariuszem testowym.
Jest on napisany po części wbrew powyższym wytycznym. Składa się bowiem
z kolejnych, zależnych od siebie przypadków testowych.

Gdyby to miał być kod produkcyjny, to na pewno chciałbym go rozbić na niezależne części.
Możecie spróbować tego dokonać w ramach ćwiczeń.


{% highlight clojure %}
(testing "nonexistent route results with 404"
  (let [response (app (mock/request :get "/"))]
    (is (= (:status response) 404))))

(testing "bookstore should be initially empty"
  (let [response (app (mock/request :get "/books"))]
    (is (= (:status response) 200))
    (is (= (:body response) "[]"))))

(testing "a book insertion is successful"
  (let [insertion-response (-> (mock/request :post "/books")
                               (mock/body "{\"author\":\"Andrzej Sapkowski\", \"title\":\"Last Wish\"}")
                               (mock/content-type "application/json")
                               app)]
    (is (= (:status insertion-response) 200))
    (is (= "Andrzej Sapkowski" ((parse-string (:body insertion-response)) "author")))))

(testing "there should be exactly one book in the bookstore after insertion"
  (let [read-response (app (mock/request :get "/books"))
        books (parse-string (:body read-response))
        book (first books)]
    (is (= 1 (count books)))
    (is (= (book "author") "Andrzej Sapkowski"))
    (is (= (book "title") "Last Wish"))
    (is (some? (book "id")))))
{% endhighlight %}


Jak widzimy, wywołania testowanych operacji oraz pomocnicze zmienne znajdują się na liście funkcji `let`.
Asercje na temat znajdziemy w bloku poniżej.

Tradycyjnie, całość kodu dostępna na [GitHubie](https://github.com/paliwodar/clojure-bookstore).

