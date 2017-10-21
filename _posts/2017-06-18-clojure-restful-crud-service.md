---
layout: page

title: "Clojure RESTful CRUD service"
subheadline: "Microservices #2"

teaser: "Rozwinięcie artykułu <a href='/twoj-pierwszy-rest-serwis-w-clojure'>o stawianiu webserwisu opartego na Compojure</a>.

         Pokazałem wtedy jak stworzyć od podstaw projekt, który z grubsza jedynie wita się ze światem.
            <br><br>
         Przetransformujemy go teraz w minimalną działajacą aplikację. Będzie
         to fragment systemu obsługującego księgarnię. Celem jest napisanie
         REST-owego interfesju do zarządzania bazą książek."

category: dev

tags:
  - clojure
  - compojure
  - rest

image:
  thumb: "photo-rest2.jpg"

header:
  image_fullwidth: "photo-rest2.jpg"

comments: true

---


Pozwoli on pobranie zbioru książek, a także na jego modyfikację --
dodanie nowej, usunięcie i modyfikację istniejących.
Nic skomplikowanego, ale od czegoś trzeba przecież zacząć.

Każda książka będzie miała autora, tytuł i rok wydania.
Na razie tyle nam wystarczy.

Przykładowy JSON:

{% highlight json %}
{
      "id" : "99a54b35-53b6-4d73-a640-607f42f14fb0",
      "author" : "Andrzej Sapkowski",
      "title" : "Last Wish",
      "year" : "1993"
}
{% endhighlight %}



## REST?

O pełną definicję się niestety nie pokuszę. Nie wiem czy w ogóle takowa istnieje.

Powiem jednak, że jest to implementacja jednego ze stylów architektury integracyjnej -
RPC, czyli zdalnego wywołania procedury[^rpc]. Oparta na protokole HTTP.

Mówimy z angielskiego, że serwis jest RESTful. No i może on być RESTful w różnym stopniu.
Możemy np. wystawić tylko jeden *endpoint* i obsługiwać zapytania inaczej w zależności
od przesłanej treści.

Można wyodrębnić oddzielne zasoby, posiadające odzielne URL-e, a także obsługiwać
poprawnie *HTTP verbs* jak GET, POST, PUT, etc w celu wykorzystania cache'owania.

Więcej o tym możemy przeczytać w [artykule Martina Fowlera](https://martinfowler.com/articles/richardsonMaturityModel.html)
(XML) albo w [tym artykule na DZone](https://dzone.com/articles/richardson-maturity-model-and-pizzas) (JSON).


## Handler

Przypomnijmy, że nasz handler wygląda do tej pory tak:


{% highlight clojure %}
(defroutes app-routes
           (GET "/greeting" [name] (str "Hello, " (or name "World") "!"))
           (route/not-found "Not Found"))

(def app
  (handler/api app-routes))
{% endhighlight %}

Pierwsza rzecz jaką zrobimy jest dodanie obsługi jsona. Mam tutaj na myśli transformację
z tekstu np. *"{ \"author\":\"Hickey\" }"* na odpowiadajacą mu strukturę Clojure,
 np. mapę *{ :author "Hickey" }*. I na odwrót dla _response_.

Istnieje wiele bibliotek, które mogą to dla nas załatwić jak np. chessire, data.json.
W naszym przypadku użyjemy tego, co oferuje nam [Ring](https://github.com/ring-clojure/ring),
na którym oparty jest Compojure, czyli funkcji:

`(wrap-json-response), (wrap-json-body)`

Zmienimy także `handler/site` and `handler/api`, który zgodnie z dokumentacją
zdaje się lepiej odpowiadać naszym potrzebom: *"Create a handler suitable for a web API(...)"*.

Funkcja _app_ wyglada teraz tak:


{% highlight clojure %}
 (def app
   (-> (handler/api app-routes)
       (wrap-json-body)
       (wrap-json-response)))
{% endhighlight %}

Poslużyliśmy się tutaj wygodnym niekiedy makrem `->`, które pozwala na bardziej czytelne
wyrażenie złożenia funkcji. Kod zwraca `(wrap-json-response (wrap-json-body (handler/api app-routes)))`.

W tym przypadku najpierw tworzymy *handlera* naszych zadań http aplikując funkcję
handler/api do instancji definicji routingu. Nastepnie ,,wzbogacamy'' go
o obsługę jsona.

Dla większego zrozumienia warto porównać implementację `wrap-json-body` z `wrap-json-response`.


## Routes

Definicja routingu tworzy szkielet naszej aplikacji. Tutaj określamy
jakie mamy mieć *endpointy* i jakie funkcje je obsługują.


GET /books
: Zwraca zbiór wszystkich książek. W Compojure wyrażony przez `(GET "/books" [] (read-books)`, gdzie
_read-books_ odpowiada za operację czytania. Zobaczymy jej implementację niebawem.

GET /books/[id]
: Zwraca książkę o podanym id. Definicja tej ruty to `(GET "/books/:id" [id] (read-book-by-id id))`

POST /books
: Dodanie nowej książki -- `(POST "/books" {body :body} (insert-new-book body))`


Możemy zauważyć, że wszystkie te operacje odnoszą się do tego samego zasobu _/books_.
Compojure pozwoli nam zgrupować je używając makra `context`.


Trzeba podkreślić ponadto, że starałem się oddzielić logikę operacji od definicji routingu,
przy użyciu pomocniczych funkcji. Jako zaawansowany początkujący w Clojure,
nie znam wszystkich konwencji, ale podowiadał mi to zdrowy rozsądek.

Podsumujmy zatem czego udało nam się dokonać do tej pory.
Poniżej kod routingu wraz z dodatkowymi operacjami usuwania i modyfikacji.

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



## ,,Logika biznesowa''

Każda aplikacja musi posiadać _warstwę_ logiki biznesowej[^logika].
Nasza nie będzie gorsza. Ta logika, to implementacja funkcji takich jak `read-books`.

W naszym przypadku operacje te będą głównie bazować na odwołaniach do bazy danych.
Być może z dodakiem pewnych dodatkowych detali.

No i właśnie ta pierwsza funkcja, podobnie jak ta odpowiadajaca za usuwanie książek
będą mieć trywialną implementację:

{% highlight clojure %}
(defn read-books []
  (db/read-books))

(defn delete-book [book-id]
  (db/delete-book book-id))
{% endhighlight %}

Zwyczajnie odwołują się one do funkcji bazodanowych. Trochę ciekawiej to wygląda
w przypadku dostępu do konkretnego zasobu książkowego:

{% highlight clojure %}
(defn read-book-by-id [id]
  (let [results (db/read-book id)]
    (if (empty? results)
      {:status 404}
      (ring.util.response/response (first results)))))
{% endhighlight %}

Funkcja `db/read-book` jedynie wykonuje *select* zwracając listę wierszy z książkami.
A zatem pozostaje nam wyłuskac wynikowa książkę oraz obsłużyć przypadek braku wyników.
Zwracamy wtedy naturalnie kod 404, czyli *not found*.


## Baza danych

Książki musimy gdzieś trzymać. Potrzebujemy zatem bazy danych.
Na potrzeby naszego przykładu będzie to H2, istniejąca tylko w pamięci - tzw. *in-memory database*.
Będziemy mogli w dowolnym momencie to zmienić, nie modyfikując kodu naszej aplikacji.

Dla lepszego uporządkowania cały kod związany z bazą znajdzie się w oddzielnym namespace -- `bookstore-rest.db`.

#### Połączenie

Po pierwsze zdefiniujmy parametry połączenia do naszej bazy.
Mogą one wygladać np. tak:

{% highlight clojure %}
(def db-spec {:classname "org.h2.Driver"
              :subprotocol "h2"
              :subname "mem:bookstore;DB_CLOSE_DELAY=-1"})
{% endhighlight %}

Widzimy powyżej mapę z nastepującymi parametrami: klasę sterownika połączenia do bazy, protokół oraz dodatkowe parametry bazy H2.

{% include alert text='
<h6>Wskazówka</h6>
<smaller>Dla uproszczenia przykładu z bazą danych będziemy sie łaczyć na nowo przy każdym zapytaniu.
Prawidłową techniką byłoby wykorzytywanie puli istniejących połączeń. Rozwinięcie tematu niebawem.
Na teraz mogę wspomnieć, że mamy do dyspozycji np. ComboPooledDataSource z biblioteki C3P0 (java).
Stąd też "DB_CLOSE_DELAY=-1" &ndash; chcemy żeby baza dalej była dostępna, mimo braku aktywnych z nią połączeń.</smaller>'
%}


Możemy więc już łączyć się z bazą. No ale najpierw musimy ją odpowiednio zainicjalizować.


#### Tworzenie tabeli

Ring pozwala na zdefiniowanie funkcji wywoływanej przy starcie serwera. To idealne miejsce na umieszczenie
funkcji tworzącej tabele w bazie, która w końcu będzie żyła tak długo jak i serwer.

{% highlight clojure %}
(defn init []
  (jdbc/db-do-commands db-spec
                       (jdbc/create-table-ddl :books
                                              [[:id "varchar(256)" "primary key"]
                                               [:author "varchar(32)"]
                                               [:title "varchar(64)"]
                                               [:year "integer"]])))
{% endhighlight %}

Funkcja ta jest podpięta do ringa z poziomu project.clj. Alternatywnie możemy dodać
w razie potrzeby funkcję sprzątająca, wywoływaną podczas zatrzymania serwera.


#### Operacje bazodanowe

Stworzyliśmy już szkielet routingu. Teraz pokażę implementacje funkcji odpowiadających za poszczególne operacje.
Nie będzie tutaj zbyt dużo magii. Ot, zwykłe wywołania zapytań SQL za pośrednictwem pakietu `clojure.java.jdbc`:

{% highlight clojure %}
(defn read-books []
  (jdbc/query db-spec ["SELECT * FROM books"]))

(defn insert-book [body]
  (jdbc/insert! db-spec :books body))

(defn read-book [id]
  (jdbc/query db-spec ["SELECT * FROM books where id = ?", id]))

(defn delete-book [id]
  (jdbc/execute! db-spec ["DELETE FROM books where id = ?", id]))

(defn update-book [id body]
  (jdbc/update! db-spec :books body ["id  = ?" id]))
{% endhighlight %}



#### Na początek wystarczy, ale...


O tym jak dodać obsługę *poolingu* połączeń przeczytamy więcej na
[clojure-doc.org](http://clojure-doc.org/articles/ecosystem/java_jdbc/connection_pooling.html).

Jeżeli wiążemy jakieś większe nadzieje z projektem, to powinniśmy
pomyśleć o obsłudze migracji. Być może przy użyciu jednej z bibliotek wymienionych
na dole [tej samej strony](http://clojure-doc.org/articles/ecosystem/java_jdbc/home.html#where-to-go-beyond-java-jdbc).


## Możliwe wariacje API

Możemy się spotkać z nieznacznymi różnicami czy też usprawnieniami w realizacji tego typu serwisów. Takimi jak:

* brak *respose body* dla operacji POST, zamiast tego *Location header* wskazujący skąd pobrać nowo utworzony zasób
* PUT - obsługa statusów 404, czy też 409
* DELETE -- odpowiedź 404 w przypadku braku obiektu
* można mieć dylemat czy id powinno być częścią jsona i zwracane w GET czy może nie, szczególnie dziwnie może
  może wyglądać PUT z różnymi id w URL i body
* wykorzystanie ETAG do weryfikacji najświeższej wersji zasobu


## Przetestujmy!

Sprawdźmy zatem jak działa nasz serwis w akcji. Użyjemy do tego zwyczajnie programu curl.

{% include alert terminal="
$ curl localhost:3000/books \\
[] \\
\\
$ curl -X POST localhost:3000/books -d '{\"author\":\"Andrzej Sapkowski\",\"title\":\"Last Wish\", \"year\": 1993}' -H \"Content-Type: application/json\" \\
{\"id\":\"af73fc98-1e12-4285-871a-25a201cf1bbc\",\"author\":\"Andrzej Sapkowski\",\"title\":\"Last Wish\",\"year\":1993} \\
\\
$ curl localhost:3000/books \\
[{\"id\":\"af73fc98-1e12-4285-871a-25a201cf1bbc\",\"author\":\"Andrzej Sapkowski\",\"title\":\"Last Wish\",\"year\":1993}] \\
\\
$ curl -X DELETE http://localhost:3000/books/af73fc98-1e12-4285-871a-25a201cf1bb \\
$ curl localhost:3000/books \\
[]" %}


{% comment %}
curl -X POST localhost:3000/books -d '{"author":"john","title":"title2"}' -H "Content-Type: application/json"

dziala tylko z json application type


curl

curl -X POST localhost:3000/books -d '{"author":"john","title":"title21"}' -H "Content-Type: application/json"
curl -X DELETE http://localhost:3000/books/b27632f2-bcaf-49c7-8ea9-4ca3810b5f6d
curl localhost:3000/books
curl -X PUT localhost:3000/books/0814da21-93ce-4592-94bb-bf9849a4771c -H "Content-Type: application/json" -d '{"author":"john","title":"title_UPDATED!"}'

{% endcomment %}


## Zwiastun

Z powyższego opisu wynikać może, że pominęliśmy do tej pory bardzo ważny aspekt: testowanie.

Powiemy sobie o nich niejako post factum, w <a href='/clojure-webservice-testowanie'>jednym z kolejnych wpisów</a>.

Oczywiście nie znaczy to, że czekamy z testowaniem, aż skończymy pisać cały kod aplikacji.
Testy -- w mojej opinii -- powinny się przynajmniej implementacją zazębiać,
tak aby mieć pewność z kod działa jak należy z jak najmniejszym opóźnieniem.

Zwykle nie jest dla mnie aż tak istotne czy najpierw piszemy testy (TDD) czy też potwierdzamy
poprawne działanie już napisanego kodu. Coraz częściej -- świadom wad i zalet -- skłaniam się ku temu drugiemu.
Ale to temat na zupełnie inną okazję.

Póki co, całość kodu dostępna na [GitHubie](https://github.com/paliwodar/clojure-bookstore).



[^rpc]: Wiem, zwykle RPC kojarzy się z czymś innym, ale mam tutaj na myśli znaczenie ogólne.
[^logika]: Przynajmniej było tak kiedyś. Teraz częściej mówi się o warstwie domenowej.