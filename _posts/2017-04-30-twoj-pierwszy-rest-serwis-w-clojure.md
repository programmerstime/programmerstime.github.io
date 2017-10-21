---
layout: page

title: "Piszemy nasz pierwszy REST-owy webserwis w Clojure"
subheadline: "Microservices #1"

teaser: "W artykule tym spotkamy się po raz pierwszy z Clojure. Fantastycznym
         nowoczesnym dialektem Lispa, działającym na JVM.
         Mógłbym o nim mówić w samych superlatywach.
        <br><br>
         Na to jednak będziecie musieli trochę poczekać. Z pewnością nastąpi to w jednym z najbliższych postów.
         Teraz bowiem będzie krótko, zwięźle, do rzeczy i technicznie.
        <br><br>
         Napiszemy najprostszy możliwy REST-owy serwis. Cóż więc innego mielibyśmy napisać,
         jeżeli nie sztandarowe <i>hello world</i>?"

category: dev

tags:
  - clojure
  - compojure
  - rest

image:
  thumb: "photo-clojure.jpg"

header:
  image_fullwidth: "photo-clojure.jpg"

comments: true

---

## Clojure

Dla ułatwienia zrozumienia kodu, który pojawi się niebawem, zapoznajmy się ze składnią Clojure.
 A konkretnie z faktem, iż język ten opiera się na tzw. notacji polskiej, czyli prefiksowej.

 Funkcje i operatory zawsze pojawiają się przed argumentami. Ponadto ujęte są w nawiasy,
 w następujący sposób:

 <pre>
 (funkcja argument1 argument2)
 (println "Hello World!")
 (+ 1 2)
 (== (+ 1 2) 3)</pre>

## Cel

Stworzymy bardzo prosty webserwis, który bedzie posiadał jeden endpoint: */greeting*.

Będzie on pozdrawiał świat używając plain teksu. Będzie także reagował na parametr *name*.
I tak zachowanie opisać możemy następująco:

{% include alert terminal="
curl localhost:3000/greeting \\
Hello, World! \\
\\
curl localhost:3000/greeting?name=Jack \\
Hello, Jack!"
%}


## Tworzymy projekt

Użyjemy leiningen -- systemu budowania projektu i menadżera zależności dla Clojure.
To taki w przybliżeniu odpowiednik mavena czy gradle.

Pozwoli on nam szybko i bezboleśnie stworzyć projekt wykorzystujący Compojure,
czyli coś co wpada do kategorii *routing library* i
pozwala na przyporzadkowanie URL-om odpowiadających funkcji Clojure.

{% include alert terminal="
lein new compojure clojure-hello"
%}

Po uruchomieniu powyższego powinniśmy dostac świeżo utworzony projekt, który niebawem zmodyfikujemy.

Poniższa komenda pozwoli nam na uruchomienie tegoż serwisu.
Możemy takim go zostawić, będzie się on samoczynnie uaktualniał.

{% include alert terminal="
lein ring server"
%}


## Leiningen

Polecam w tym momencie przejście szybkiego tutoriala. Oczywiście najpierw instalacja.
A potem `lein help tutorial`. Przyda się jako, że jest to bardzo podstawowe narzedzie dla Clojure.

Pozwolę sobie tutaj wymienić kilka informacji. Niektóre z nich pojawiają się w samouczku.

Szukanie zależności
: Informacje na temat np. wersji znajdziemy na  *clojars.org*. Możemy
też wyszykiwać przy pomocy Leiningen np. `lein search clj-http`

{% comment %}
lein search clojure
    $ lein search description:crawl
    $ lein search group:clojurewerkz
    $ lein search \"Riak client\"
    {% endcomment %}

Checkout dependencies
: Mechanizm pozwalający na sprawny devlopment więcej niż jednego projektu jednocześnie,
nie musimy wtedy restartować REPL-a[^repl]

Przydatne komendy
: `lein repl` -- uruchomienie pętli REPL \\
`lein run` -- wywołanie kodu funkcji -main, jeżeli jest ona zdefiniowana \\
`lein test` -- uruchomienie testów


Znajdziemy tam także informacje, jak obchodzić się z projektem
w zależnosci czy ma on byc dostarczony końcowemu użytkownikowi,
udostepniony jako biblioteka, czy też uruchomiony na serwerze.

W zależności od zastosowania możemy chcieć utworzyć uberjar,
dodać bibliotekę to clojars lub zbudować projekt
bazując na wybranych szablonach projektów webowych,
jak compojure w naszym przypadku.

## Do kodu!

Nasz świeżo utworzony projekt powinien posiadać następującą strukturę:

<pre>
.
├── README.md
├── project.clj
├── resources
│   └── public
├── src
│   └── compo_standard
│       └── handler.clj
└── test
    └── compo_standard
        └── handler_test.clj
</pre>

Najważniejsze pliki to project.clj i handler.clj.

Ten pierwszy to odpowiednik build.gradle czy też mavenowego pom.xml.
Określa on zależności naszego projektu i definiuje *handlera requestów*.
Początkowo ma on taką treść:

{% highlight clojure %}
(defproject compo-standard "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :min-lein-version "2.0.0"
  :dependencies [[org.clojure/clojure "1.8.0"]
                 [compojure "1.5.1"]
                 [ring/ring-defaults "0.2.1"]]
  :plugins [[lein-ring "0.9.7"]]
  :ring {:handler compo-standard.handler/app}
  :profiles
  {:dev {:dependencies [[javax.servlet/servlet-api "2.5"]
                        [ring/ring-mock "0.3.0"]]}})
{% endhighlight %}

Na nasze potrzeby wygląda nieźle. Gorliwi mogą dodać opis i link do ich strony.
Następnie zajrzymy do pliku handler.clj. Tak wygląda jego główna funkcja:

{% highlight clojure %}
(defroutes app-routes
  (GET "/" [] "Hello World")
  (route/not-found "Not Found"))
{% endhighlight %}

Zmodyfikujemy ją zatem tak żeby odpowiadała naszym potrzebom:

{% highlight clojure %}
(defroutes app-routes
           (GET "/greeting" [name] (str "Hello, " (or name "World") "!"))
           (route/not-found "Not Found"))
{% endhighlight %}


{% comment %}
---

zmieniono takze handler/site and handler/api
"Create a handler suitable for a web API. This adds the following
  middleware to your routes"
----

{% endcomment %}

I już, wszystko powinno śmigać jak należy. W szczegóły zagłębimy się być może przy następnej okazji.
A oto pełny kod handlera:

{% highlight clojure %}
(ns clojure-hello.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [compojure.handler :as handler]))

(defroutes app-routes
           (GET "/greeting" [name] (str "Hello, " (or name "World") "!"))
           (route/not-found "Not Found"))

(def app
  (handler/api app-routes))
{% endhighlight %}

## Zwiastun

W <a href='/clojure-restful-crud-service'>następnym odcinku </a>spróbujemy czegoś bardziej skomplikowanego.
Jak np. bazy danych i obchodzenia się z JSON-em. *Stay tuned!*


[^repl]: Skrót od read-eval-print loop