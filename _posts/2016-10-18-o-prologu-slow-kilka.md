---
layout: page

title: "O Prologu słów kilka"
subheadline: "W 2 minuty dookoła programowania logicznego"

teaser: "Artykuł opierający sie na materiałach sięgających czasów studenckich autora tego bloga, czyli dobrą dekadę wstecz.
         Wtedy to jednym z wykładów do wyboru było programowanie logiczne. Jest to niecodzienny rodzaj programowania.
         Mała szansa, że choćby usłyszymy o nim w statystycznym projekcie w firmie.
         <br><br>
         Czuję jednak wielką chęć na wniesienie swojego wkładu w rozpowszechnianiu prologowych idei.
         Muszę przy okazji przyznać, że go bardzo lubię. Szczególnie za ten deklaratywny styl.
         <br><br>W programie prologowym nie mówimy krok po kroku, co należy zrobić żeby dane zadanie
         czy problem rozwiązać. Zamiast tego po prostu *deklarujemy* jakie warunki powinno spełniać jego rozwiązanie.
         Prolog sam znajdzie interesujące nas instancje."

category: dev

tags:
  - programowanie
  - języki
  - prolog

image:
  thumb: "photo-medusas.jpeg"

header:
  image_fullwidth: "photo-medusas.jpeg"

comments: true

---


##Termy
Prolog jest beztypowy, a jedyną strukturą danych jaką wykorzystuje są termy. Termy można na nasze potrzeby
opisać następująco:

    Term = X | f(t1, t2, ..., tn)

gdzie <var>X</var> jest zmienną (w Prologu zmienne rozpoczynają się wielkimi literami lub symbolem&nbsp;'_'),
<var>f</var> jest symbolem funkcyjnym o arności n, a <var>t1, ..., tn</var> są także termami.

Symbole o arności 0 nazywamy stałymi i rozpoczynamy małą literą lub bierzemy w apostrofy.
Jak widać sprawdza się tutaj staropolskie przysłowie głoszące, że wszystko jest termem : ).

##Klauzule hornowskie
Innym potrzebnym zagadnieniem są podstawy rachunku zdań. Formuły rachunku zdań budujemy za pomocą funktorów
zdaniotwórczych (fałsz, prawda, koniunkcja, alternatywa, itd.) oraz zmiennych zdaniowych.

Klauzulą nazywamy
alternatywę literałów (zmiennych zanegowanych lub nie). Klauzulą hornowską natomiast, taką klauzulę,
w której co najwyżej jedna zmienna jest niezanegowana (można klauzulę tę zapisać w postaci implikacji).

W ogólności problem spełnialności formuły zbudowanej z koniukcji klauzul jest NP-zupełny.
Między innymi dlatego Prolog ogranicza się do klauzul Horna, w przypadku których analogiczny problem można
rozwiązać w wielomianowym czasie.

Przykładem takiej klauzuli może być np.:

$$ a \lor \neg b \lor \neg c $$

co jest równoznaczne z następującą implikacją (celowo napisaną ,,w lewo''):

$$ a \Leftarrow b \land c $$

która to z kolei w składni Prologa jest odwzorowana jest w sposób następujący: <code>a&nbsp;:-&nbsp;b,&nbsp;c</code>.
A zatem programy prologowe to z grubsza implikacje obliczane ,,od końca''.


##Unifikacja
Najważniejszą chyba sprawą, z którą należałoby się zaznajomić u progu prologowej przygody jest unifikacja.
Przez unifikację termów *t* oraz *u* rozumiemy operację znalezienia takiego przyporządkowania
zmiennych, żeby termy *t* i *u* były równe.

W Prologu na początku pod każdą zmienną może być podstawiony jakikolwkiek inny term.
Dwie stałe mogą być zunifikowane wtedy i tylko wtedy, gdy są równe.

Dwa termy, które nie są stałymi mogą być zunifikowane wtedy i tylko wtedy, gdy ich najbardziej zewnętrzne
symbole funkcyjne są tej samej arności oraz gdy kolejne argumenty tych symboli są parami unifikowalne.
Przykłady unifikacji:

<pre>
A = A : powiodła się  (tautologia)
abc = abc : powiodła się
abc = xyz : nie powiodła się
f(A) = f(B) : A jest unifikowane z B
f(g(A), A) = f(B, xyz) : unifikacja A z xyz i B z termem g(xyz)
f(A) = A : nieskończona unifikacja A z f(f(f(f(...))))
</pre>

Prolog unifikuje (podstawia) w sposób leniwy — tylko tyle ile musi. Być może algorytm znajdowania
najogólniejszego unifikatora zostanie przedstawiony w ewentualnej kolejnej części artykułu.

##Rozmowa z interpreterem
Zanim przejdziemy do prostych programów spróbujmy przetestować wyżej wspomnianą teorię w interpreterze Prologa.

{% include alert terminal="
?- 1 + 2 * 3 = X * Y. <em style='color: orange'>% zapytanie</em>\\
No <em style='color: orange'>% odpowiedź</em>\\
\\
?- 1 + 2 * 3 = X + Y. \\
X = 1 \\
Y = 2 * 3 \\
\\
?- A = f(A). \\
A = f(f(f(f(f(f(f(f(f(f(...))))))))))"
%}
Prolog ma z góry zdefiniowane priorytety i łączliwość operatorów. Stąd powyższe reakcje.

##Pierwsze programy
Najważniejszą niemalże częścią programu Prologowego jest baza reguł.
Składa się ona z faktów i reguł, które podane są w formie implikacji
opisanych kilka paragrafów powyżej. Baza taka może wyglądać np. następująco:

{% highlight prolog %}
mezczyzna(adam).
mezczyzna(bartek).
mezczyzna(czarek).

matka(anna, bartek).
matka(anna, czarek).
matka(anna, celina).

brat(X, Y) :- matka(M, X), matka(M, Y), mezczyzna(X).
{% endhighlight %}

Jak widzimy jest to baza relacji rodzinnych, czyli taki sztandarowy przykład.
Jest tam między innymi predykat *brat/2* opisujący relację ,,bycia bratem’‘.
Jego definicja oznacza: ,,X jest bratem Y, jeżeli M jest matką X i M jest matką Y oraz X jest mężczyzną’‘.

Zadajemy Prologowi zapytanie:

{% include alert terminal="
?- brat(X, czarek). \\
X = bartek"
%}

Spróbujmy prześledzić zapytanie *brat(X, czarek)*. Ograniczymy się przy tym do pierwszego rezultatu.
Najpierw przebiega powiedziona próba unifikacji termu *matka(M, X)* z pierwszym z faktów dotyczących matki,
czyli termem *matka(anna, bartek)*. Dokonują się tutaj podstawienia *M=anna* oraz *X=bartek*.

Następnie term *matka(anna, czarek)* jest unifikowany w sposób trywialny z identycznym termem.
Podobna rzecz dzieje się z termem mężczyzna(bartek). Wynikiem unifikacji jest *X=bartek*.
Czujemy niesamowitą satysfakcję z rozumienia tego narzędzia. Czujemy?

W przykładzie tym wystąpiła pewna ciekawa sytuacja. Otóż okazuje się, że wg naszego predykatu relacja
bycia bratem jest zwrotna, co nie powinno mieć miejsca. Chciałoby się dodać, że *brat(X,Y)* będzie tylko wtedy,
gdy *X* jest różny od *Y*. W SWI-Prologu napiszemy:

    \+(X = Y)


Niestety okazuje się, że w ,,czystym’‘ Prologu mamy niemałe problemy z wyrażeniem nieprawdy.
Postaramy się przyjrzeć tej sprawie w przyszłości.

##Przykład — własna arytmetyka
Załóżmy, że chcemy zdefiniować własne liczby naturalne przy pomocy symbolu 0 oraz symbolu funkcyjnego *s/1*
oznaczającego następnika, a także operację dodawania w tej reprezentacji. Będzie to wyglądało na przykład tak:

{% highlight prolog %}
nat(0).                % 0 jest liczbą naturalną
nat(s(X)) :- nat(X).   % następnik liczby naturalnej także
{% endhighlight %}

Możemy w łatwy sposób sprawdzić działanie predykatu:

{% include alert terminal="
?- nat(0) \\
Yes\\
?- nat(s(0)) \\
Yes\\
?- nat(s(s(s(0))))\\
Yes"
%}

O powyższych liczbach możemy myśleć, że to 0, 1 oraz 3.
Operacja dodawania wyglądać może następująco:

{% highlight prolog %}
add(0, X, X) :- nat(X).
add(s(X), Y, s(Z)) :- add(X, Y, Z).
{% endhighlight %}

Reguły te możemy dla łatwiejszego zrozumienia wyrazić także w *zwykłej* arytmetyce:

    0 + X = X jeżeli X jest liczbą naturalną
    (X + 1) + Y = Z + 1 jeżeli X + Y = Z


Przykładowe zastosowanie:

{% include alert terminal="
?- add(s(0), s(0), K). \\
K = s(s(0))"
%}


Raz jeszcze przeanalizujemy działanie Prologa. Najpierw term *add(s(0), s(0), K)* jest unifikowany
z *add(0, X, X)* z niepowodzeniem, później z termem *add(s(X), Y, s(Z))*.

Rezultatem są podstawienia *X=0, Y=s(0)* oraz *K=s(Z)*, później przechodzimy do przesłanki *add(X,Y,Z)*,
która zgodnie z podstawieniami ma postać *add(0, s(0), Z)*. Jest ona unifikowana z *add(0, X’, X’)*.
Rezultat — *X’=s(0),Z=s(0)*. A zatem *K=s(s(0))*.

<var>X’</var> pojawiło się nagle w celu uniknięcia posługiwania się tą samą zmienną w różnych znaczeniach.
Widzimy, że taki sposób analizy jest dość kłopotliwy. Po nacisnięciu klawisza ; zamieniamy rezultat
obliczeń na fałsz, co wznawia dalsze obliczenia Prologa.

W ten sposób widzimy, że Prolog przechodzi
w głąb pewne drzewo, w którego wierzchołkach znajdują się kolejne predykaty. Bardzo dobrą praktyką
jest narysowanie takiego drzewa (po jakimś czasie będziemy w stanie to zrobić w pamięci na pierwszy
rzut oka na program :). Być może pojawi się taka rzecz tutaj w przyszłości.


##Do przeczytania wkrótce
Mam nadzieję, że niniejszy wpis rozbudził ciekawość i przybliżył w przystępny sposób idee leżące
u podstaw programowania (logicznego i nie tylko). Postaram się opisać moje nieco bardziej zaawansowane
przygody z Prologiem wkrótce. [Zapraszam](/o-prologu-arytmetyka-i-rekursja).
