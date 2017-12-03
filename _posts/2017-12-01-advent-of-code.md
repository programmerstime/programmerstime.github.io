---
layout: page

title: "Advent of Code"
#subheadline: "subheadline"

teaser: "Chciałbym wszystkich niniejszym zachęcić do udziału w konkursie programistycznym 
         <a href='http://adventofcode.com/'>Advent of Code</a>.
         Natrafiłem na niego przypadkiem, ale już wcześniej zdarzyło mi się o nim słyszeć.
         <br><br>
         W skrócie mamy 25 problemów do rozwiązania, jeden na każdy dzień adwentu.
         Wspaniała forma na wyczekiwanie i przygotowanie do świąt ;).
         <br><br>
         W Advent of Code nie musimy wysyłać kodu źródłowego. Wystarczy otrzymanie
          wyniku obliczonego dla podanych danych wejściowych.
         <br><br>
         Postanowiłem spróbować swoich sił. Takie wydarzenia, to na przykład
         wspaniała forma poznawania nowych języków programowania. Ja na pierwsze zadania 
         wybrałem Groovy. Język łatwy do przyswojenia dla zaznajomionych z Javą. 
         Jednocześnie dużo lżejszy i bardziej ekspresyjny."

tags:
  - programowanie

category: dev

image:
  thumb: "advent-code-tree.jpg"
  title: "advent-code-tree.jpg"

comments: true

---

## Projekt

Zacząłem od inicjalizacji prostego projektu przy użyciu gradle.
Sam gradle nie jest konieczny, można by projekt stworzyć używając dowolnego innego sposobu.

{% include alert text='
Dla niewtajemniczonych gradle to system budowania i zarządzania zależnościami w projekcie,
podobnie jak maven. W tym wpisie nie będziemy sie nim zbytnio zajmować.
' %}

Do stworzenia projektu wystarczy polecenie:

{% include alert terminal='
gradle init --type groovy-library' %}


Dostajemy po nim następującą strukturę:

<pre>
.
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   └── groovy
    └── test
        └── groovy
</pre>

Mamy zatem najprostszy jak to tylko możliwe project w Groovym. 
Jak nietrudno się domyślić, implementację zadań umieścimy w *src/main*,
umieszczając testy w odpowiadających pakietach w *src/test*.


## Groovy

Wiedzałem, że nie będę nigdzie wysyłał mojego kodu. Wystarczy zatem żeby zrobił on 
to co do niego należy -- przeprowadził kalkulacje zgodnie z podanym algorytmem.

W takiej sytuacji sam kod nie musiał być idealny, szybki, ponadprzeciętnie czytelny 
czy też łatwy w utrzymaniu.

Najbardziej zależało mi na łatwości i zwinności w implementacji rozwiązania.
Na tym żebym się mogł skupić na algorytmie bez zbytecznego boilerplejtu.   

Niezłym wyborem wydał się właśnie Groovy. Język funkcyjny z dynamicznym typowaniem,
znakomicie wspierający pracę z kolekcjami.  Pozwalający na pisanie krótkich i eleganckich
rozwiazań.
 
Rozważałbym także pythona do tego typu zadań. Java wydaje się tutaj zbyt toporna.

## Zadanie 1: Inversed Captcha

Całość opakowana jest w zabawną fabułę. Otóż w przedświąteczną noc okazuje się,
że Świety Mikołaj nie jest stanie wydrukować jego "Naughty or Nice list".

Na drodze do tego stoi 50 bugów, które trapi jego drukarkę. Naszym zadaniem będzie
ich eliminacja.

Pierwszym wyzywaniem jest przejście captchy. Składa sie ona z listy cyfr.
Poszukiwanym rozwiązaniem jest suma wszystkich elementów, które są powtórzone
na następnej pozycji. Listę należy traktować jako cykliczną.

Przykładowo 1122 daje wynik 3 (1+2), a 91212129 oblicza sie do 9.

### Testy:

Najpierw napisałem testy dla brzegowych przypadków oraz podanych, potem implementację -- była dość prosta.
Następnie dodałem test z danymi przykładowymi, by upewnić się, że mój program działa prawidłowo.
W końcu napisałem test case dający ostateczny wynik.

Tak wyglądały testy:

{% highlight groovy %}
def shouldReturn0ForEmptyInput() {
   expect:
   InversedCaptcha.calculateSum("") == 0;
}

def shouldWorkForSingleDigitInput() {
   expect:
   InversedCaptcha.calculateSum("1") == 1;
}

def shouldWorkForTwoSameDigitsInput() {
   expect:
   InversedCaptcha.calculateSum("11") == 2;
}

def shouldWorkForTwoDifferentDigitsInput() {
   expect:
   InversedCaptcha.calculateSum("12") == 0;
}

@Unroll
def shouldWorkForGivenExamples() {
   expect:
   InversedCaptcha.calculateSum(input) == expectedResult;

   where:
   input      | expectedResult
   "1122"     | 3
   "1111"     | 4
   "1234"     | 0
   "91212129" | 9
}
{% endhighlight %}


Co powiecie na to jak znakomicie są czytelne? Jest tak za sprawą frameworku Spock,
który mam przyjemność wykorzystywać także w projektach Javowych.

Po testach zrobiłem mala refaktoryzajcę kodu.
 

### Rozwiazanie:

{% highlight groovy %}
static int calculateSum(String input) {
    input.toList()
         .withIndex()
         .collect { x, i -> x == input[(i + 1) % input.size()] ? x as Integer : 0 }
         .sum(0)
}
{% endhighlight %}

Implementacja to prosta transformacja listy oraz jej zsumowanie.


## Zadanie 2: Corruption Checksum

Kolejne nietrudne zadanie. Dostajemy arkusz. Dla każdego wiersza musimy obliczyć
różnicę maksymalnego i minimalnego elementu. Różnice te po zsumowaniu dadzą nam ostateczny wynik.

Testy:

{% highlight groovy %}
def exampleTestCase() {
    expect:
    CorruptionChecksum.calculate(input) == result

    where:
    input          | result
    [[5, 1, 9, 5],
    [7, 5, 3],
    [2, 4, 6, 8]] | 18
}
{% endhighlight %}

i rozwiązanie:

{% highlight groovy %}
static calculate(List<List<Integer>> input) {
    input.collect { it.max() - it.min() }.sum()
}
{% endhighlight %}


To sama przyjemność pisać tak zwięzły i czytelny kod.
Zobaczymy czy uda się zachować ten standard przy bardziej skomplikowanych zadaniach.

Zapraszam do śledzenia kolejnych wpisów. Moje rozwiązania można zobaczyć na 
[githubie](https://github.com/paliwodar/advent-of-code-2017).


