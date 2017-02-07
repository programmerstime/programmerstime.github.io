---
layout: page

title: "Wprowadzenie do sieci neuronowych, cz. 1"
#subheadline: ""

teaser: "Jeden z nielicznych (jeszcze) postów napisanych przeze mnie oryginalnie po angielsku.
         IMO wartościowy i godny zamieszczenia także na niniejszym blogu. Wersja ojczysta.<br><br>
         W artykule dzielę się swoją wiedzą i przemyśleniami na temat sieci neuronowych.
         Moim zdaniem jednego z najbardziej przecenianych i jednocześnie niedocenianych bytów w informatyce.
          To jedna z rzeczy, o których wszyscy (aktualnie coraz częściej) mówią, ale niewielu tak naprawdę wie z czym się to je."

tags:
  - sztuczna inteligencja
  - machine learning

image:
  thumb: "photo-blackboard.jpg"

header:
  image_fullwidth: "photo-blackboard.jpg"

comments: true

---

Tak też było i ze mną. Nigdy nie miałem dostatecznie dobrego powodu do tego żeby wreszcie zgłębić temat.
Za każdym razem zatrzymywałem się na obrazku, przedstawiająym kółka połączone połączone w coś w rodzaju przepływu.

Zdarzało mi si widzieć, że ludzie reagowali ze szczerym podziwem za każdym razem kiedy ktoś wpomniał,
że napisał program rozpoznający np. gatunek rośliny tylko przy pomocy obrazka przedstawiającego
blaszkę liściową i jej użyłkowanie.

Oczywiście używając sami-wiecie-czego. Sieci neuronowych! Musiał to być chyba ktoś niesamowicie łebski, co nie?
Nie byłem tego taki pewien. Trudno mi było to określić, nie mając żadnej konkretnej wiedzy by móc to zwerfikować.


## Coursera, AlphaGO

Ale wreszcie stan rzeczy uległ zmianie za sprawą udziału w kursie [machine learning](https://www.coursera.org/learn/machine-learning),
który to szczerze polecam wypróbować. Kurs przygotowany przez pana nazywającego się Andrew Ng
--jednej z najbadziej rozpoznawalnych postaci w świecie szucznej inteligencji i zarazem współtwórcy
 serwisu Coursera. Doświadczenie bardzo podobne do uczenia się Scali w klasie Martina Odersky'ego.

Jako motywację, polecam obejrzenie poniższego filmiku o tym jak system ten został mistrzem Europy w go[^european_champion].

<div class="flex-video">
    <iframe width="420" height="315" src="//www.youtube.com/embed/g-dKXOlsf98" frameborder="0" allowfullscreen></iframe>
</div>

A zatem zbudowali oni wybitny silnik grający w go. Używając nie jednej, lecz dwóch sieci neuronowych [^two_networks].
Założę się, że teraz każdy już jest chętny do opanowania tego tematu. A zatem do dzieła.

## Genesis

Sieci neuronowe powstały za sprawą zainspirowania naturą. Jest to sprawdzone podejście w informatyce w radzeniu sobie
ze skomplikowanymi problemami. W końcu każdy na pewno słyszał o algorytmach genetycznych czy mrówkowych.

W naszym przypadku, jak sama nazwa sugeruje, idea polega na naśladowaniu zachowania neuronów w naszym mózgu.
Ale dla mnie, mogłaby to być równie dobrze jedynie interpretacja modelu sieci neuronowych.
A to dlatego, że wygląda on (model ów) jak płynne rozszerzenie regresji logistycznej.

Wiem, brzmi to mgliście. Za niedługo wszystko powinno się wyjaśnić.


## Regresja

Pojęcie to dotyczy badania zależności pomiędzy zmiennymi. Zmienne mogą mieć dowolne znaczenie i interpretację.
W większości przypadków reprezentują właściwości badanych przez nas obiektów.

W przypadku go możemy mieć na przykład zmienne \\(x_{i,j} \in \\{0,1,2\\}\\)  dla \\(i, j = 1,...,19\\)
reprezentujące stan gry[^go_board] i \\(y_1, y_2\\)  opisujące koordynaty następnego ruchu.
Naturalnie istnieje korelacja pomiędzy tym co na planszy, a kolejnym posunięciem, prawda?

## Regresja liniowa

Weźmy jednak odrobinę prostszy przykład, prawdopodobnie najpopularniejszy jeżeli chodzi o uczenie maszynowe.
Szacowanie cen nieruchomości! Przypuśćmy, że mamy dwie zmienne \\(x_1\\) and \\(x_2\\) reprezentujące
powierzchnię i dystans od centrum miasta w kilometrach. No i oczywiście cenę. Naszym zadaniem będzie oszacowanie jej wartości.

Pewnie nie potrzebujemy żadnych skomplikowanych wzorów.
Wręcz przeciwnie, spróbujmy podejścia najprostszego z możliwych. Takiego jak na przykład dodanie wartości zmiennych.

Niestety nie wygląda na to żeby dodanie 50 \\(m^2\\) i 2 kilometrów mogło dać 200000 zł.
Dlaczego więc nie wprowadzimy jakichś współczynników oraz jakiejś stałej?

W ten sposób moglibyśmy otrzymać następującą funkcję regresji[^regression_equation]:

$$ h_\Theta(x) = \Theta_0 + x_i\Theta_1 + x_2\Theta_2 $$

Nie jestem pewien czy taki był oryginalny proces jej otrzymania. Niewykluczone...

A tak na marginesie, warto zauważyć, że powyższy wzór może być zapisany jako \\(\sum_{i=0}^2 x_i\Theta_i\\), gdzie \\(x_0 = 1\\),
co odpowiada mnożeniu wektorów \\(x^T\Theta\\).

Muszę przyznać przy okazji, że mnożenie macierzy pojawia się w tym kursie
(patrz Coursera) na każdym kroku. Przynajmniej w zadaniach programistycznych. Nie to żebym narzekał. Spoko się je rozwiązywało.

A zatem szukamy teraz wartości \\(\Theta\\). W tym momencie na scenę powinny wejść
dane uczące i elementy w miarę zrozumiałej analizy matematycznej. Na szczęście, zarówno dla autora jak i odbiorcy, szczegóły te tutaj pominiemy.
  Wystarczy nam na razie fakt, że chodzi o zminimalizowanie błędu pomiędzy cenami przewidzianymi, a tymi prawdziwymi,
  pochodzącymi z danych, które zebraliśmy.

Po otrzymaniu wartości \\(\\Theta\\) dostajemy równanie hiperpłaszczyzny [^hyperplane], które daje nam estymację, której szukaliśmy.
Poniżej widzimy przykładową linię wyznaczającą cenę domu w zależności od jego powierzchni:

![Linear regression](/images/neural_networks/linear_regression.svg){: .center-image .scaled-down-70-image}

To co właśnie z grubsza, mniej lub bardziej dokładnie opisaliśmy nazywane jest regresją liniową.

## Regresja logistyczna

Regresja logistyczna to dość prosta adaptacja, która pozwoli nam poradzić sobie z problemami natury dyskretnej.
Jak na przykład decyzji czy będzie dziś padać czy nie, opartej na dostępnych danych pogodowych.

Będziemy chcieli teraz dostawać wyniki \\(0\\) lub \\(1\\). Ale przecież nasza funkcja estymująca \\(h_\Theta\\)
 może zwrócić nam jakąkolwiek liczbę. Na szczęście nie jest to duży problem. Przyjmiemy za wynik \\(1\\),
  wtedy i tylko wtedy, gdy \\(h_\Theta(x) \ge 0\\).

Wygodnie będzie nam posłużyć się tzw. funkcją *sigmoid*:

$$ g(t) = \frac{1}{1 + e^{-t}}  $$

której wykres wygląda tak:

![Logistic curve](/images/neural_networks/logistic-curve.svg){: .center-image .scaled-down-50-image}

Łatwo zauważyć, że jej wynik jest bliski jedynki i zeru dla wysokich i niskich argumentów odpowiednio.

A zatem wzór regresji logistycznej miałby taką formę: \\(h_\Theta(x) = g(x^T\Theta)\\).
Jej wynik możemy interpretować jako prawdopodobieństwo zajścia zdarzenia,
przyjmując \\(1\\), kiedy dostajemy wartość większą niż \\(0.5\\).

Taki rodzaj funkcji jest potocznie nazywany perceptronem jednowarstwowym.

## To be continued

Jesteśmy już blisko przystanku docelowego w naszej opowieści. Sieci neuronowe czają się tuż za rogiem.
Niejeden będzie zaskoczony. Zachęcam do zapoznania się z drugą części artykułu (*in progress*).


{% include alert warning='
<smaller><b>Disclaimer:</b> W artykule mogą występować matematyczne nieścisłości, wynikające
    z chęci przedstawienia myśli przewodniej w sposób możliwie najbardziej przystępny.</smaller>'
%}


[^two_networks]: Technicznie mogło ich być znacznie więcej.
[^go_board]: Plansza w go ma \\(19\times 19\\) punków, na których mogą zostać położone kamienie. Wartości \\(0, 1, 2\\) oznaczałyby brak kamienia, czarny lub biały odpowiednio.
[^regression_equation]: Oczywiście ten wzór nie musi tak wyglądać. Może mieć różne formy -- na przykład regresji wielominanowej: \\(h_\Theta(x) = \Theta_1 x_1^2 + \Theta_2 x_2^2\\).
[^hyperplane]: Hiperpłaszczyzna to podprzestrzeń o wymiarze o jeden mniejszym niż przestrzeń, w której się zawiera. Przykładowo dla 2-wymiarowej płaszczyny (powierzchnia i cena) będzie to prosta powierzchnia -> cena.
[^iff]: *iff* \\(\equiv\\) *if and only if*
[^bias_skipped]: Bias values have been skipped here to make the equation simpler.
[^european_champion]: Informacja wciąż stosowna, aczkolwiek przyćmiewa ją aktualnie jeszcze bardziej zadziwiający fakt: AlphaGo wygrało 4:1 z jednym z najlepszych współczesnych graczy -- Lee Sedolem.
