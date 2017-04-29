---
layout: page

title: "O Prologu... &ndash; arytmetyka i rekursja"
#subheadline: ""

teaser: "Mam nadzieję, że <a href='/o-prologu-slow-kilka'>poprzednim wpisem</a>
        udało mi się wzbudzić zainteresowanie tematem.
         Chciałbym dodać, że z Prologiem, podobnie jak z innymi językami o ciekawych paradygmatach,
         zetknąłem się po raz pierwszy na drugim semestrze informatyki na mojej uczelni.
         <br><br>
         Niniejszy wpis jest nieco krótszy, wynika to z tego, że jest on rezultatem połowienia planu pierwotnego.
         Tematem będzie wykorzystanie arytmetyki maszynowej w Prologu oraz rekursja. Całość bardzo elementarna.
         Następne zdecydowanie już takie nie będą."

category: dev

tags:
  - programowanie
  - języki
  - prolog

image:
  thumb: "photo-prolog2.jpg"

header:
  image_fullwidth: "photo-prolog2.jpg"

comments: true

---

Nie uważam się za znawcę Prologa. Ponadto dość sporo czasu minęło od ostatniego
*poważnego* programowania w Prologu. Pomimo to, ciągle czuję się dość pewnie
w temacie. I śmiało mogę stwierdzić, że nie zapomina się raz opanowanego
podejścia do rozwiązywania problemów.


## Prawdziwa arytmetyka

Arytmetyka jaką zadaliśmy [poprzednio](/o-prologu-slow-kilka) naturalnie nie mogłaby mieć większego zastosowania.
Chcielibyśmy żeby liczba 20000 zajmowała trochę mniej niz 20 KB i żeby operacje na niej były wykonywane efektywnie.
Prolog oczywiście takie rzeczy oferuje.

Istnieją operatory `<, >, >=, =<, =:=, =/=`. Po obu ich stronach muszą wystąpić termy, które obliczają się do liczb.
Mamy do dyspozycji także predykat `is/2`, którego działanie zobrazuję na przykładzie (SWI-Prolog):

{% include alert terminal="
?- X is 1+3. \\
X = 4 \\
Yes \\
\\
?- 1+3 is X. \\
ERROR: Arguments are not sufficiently instantiated \\
\\
?- 1+3 is 1+3.\\
No \\
\\
?- 4 is 1+3. \\
Yes"
%}

W poprzedniej części jako motywacja wystąpiło obliczanie silnii. Pokażę teraz jak można do tego podejść.
Nie znając zbytnio prologowej arytmetyki moglibyśmy napisać np. tak:

{% highlight prolog %}
fact(N,F) :- N=:=0, F=1.
fact(N,N*F) :- N>0, fact(N-1,F).
{% endhighlight %}

Wtedy wynikiem obliczania `4!` byłoby:

{% include alert terminal="
?- fact(4,X). \\
X = 4* ((4-1)* ((4-1-1)* ((4-1-1-1)*1)))"
%}

Nie jest to jednak wynik nas satysfakcjonujący, nie mówiąc już o złożoności jego otrzymania.
Kolejne podejście może wyglądać np. tak:

{% highlight prolog %}
fact(N,1) :- N=:=0.
fact(N,F) :- N>0,
             N1 is N-1,
             fact(N1,F1),
             F is N*F1.
{% endhighlight %}

Jest to implementacja zgodna ze specyfikacją.
Widzimy, że na każdym poziomie rekursji *N1* jest ukonkretniane z wartością *N-1*, potem *F1* z wartością
silnii dla argumentu *N1* i wynik *F* z rezultatem obliczenia iloczynu *N* i wartości silnii dla *N-1*.
Jednak możemy zaimplementować nieco efektywniej eliminując lewostronną rekursję.

## Rekursja ogonowa
Z pojęciem *tail recursion* zetknął się chyba każdy programista języka wspierającego rekursję.
Polega ona na wyeliminowaniu konieczności używania stosu. W językach funkcyjnych na ogół uzyskuje
się ją poprzez użycie funkcji pomocnicznej. Oto dwa przykłady rekursji nieogonowej i ogonowej
odpowiednio w odwracaniu listy (OCaml):

{% highlight ocaml %}
let rec reverse = function
    [] -> []
    | x::xs -> reverse xs @ [x]
;;
{% endhighlight%}

`[]` jest listą pustą, `x::xs` listą o głowe *x* i ogonie *xs*. Operator `@` łączy dwie listy.
Funkcja ta jest nieefektywna z dwóch powodów.

Pierwszym jest użycie `@` ,
którego czas działania jest liniowy względem pierwszego argumentu (co powoduje złożoność kwadratową)
oraz konieczność odkładania na stos częściowych obliczeń. Można to zilustorwać mniej więcej tak:

<pre>
reverse [1;2;3]
reverse [2;3] @ [1]
reverse [3] @ [2] @ [1]
reverse [] @ [3] @ [2] @ [1]
[] @ [3] @ [2] @ [1]
[3] @ [2] @ [1]
[3;2] @ [1]
[3;2;1]
</pre>

Lepszym jest poniższe rozwiązanie:

{% highlight ocaml %}
let reverse xs =
  let rec aux acc = function
    [] -> acc
    | x::xs -> aux (x::acc) xs
  in
    aux [] xs
;;
{% endhighlight %}

Działanie wygląda tak:

<pre>
reverse [1;2;3]
aux [] [1;2;3]
aux [1] [2;3]
aux [2;1] [3]
aux [3;2;1] []
[3;2;1]
</pre>

## Rekursja w Prologu

W Prologu na ogół rozwiązanie jest analogiczne, tyle że skutkiem jest zwiększenie arności predykatu o 1.
Tak może wyglądać efektywna implementacja silnii:

{% highlight prolog %}
fact(N,F) :- fact(N,1,F).
fact(N,F,F) :- N =:= 0.
fact(N,A,F) :- N>0,
               N1 is N-1,
               A1 is A*N,
               fact(N1,A1,F).
{% endhighlight %}

Dodany został środkowy argument, który pełni funkcję akumulatora. Widzimy, że na ogół rekursję ogonową
otrzymuje się przez przesunięcie wywołania rekurencyjnego na sam koniec — dlatego jest też zwana prawostronną.

Jako dodatek jeszcze reverse w Prologu:

{% highlight prolog %}
reverse(Xs,Ys) :- reverse(Xs,[],Ys).
reverse([],Xs,Xs).
reverse([X|Xs],Ys,Zs) :- reverse(Xs,[X|Ys],Zs).
{% endhighlight %}

Pojawiły się tutaj listy — `[X|Xs]` oznacza listę o głowie *X* i ogonie *Xs*.

## Zwiastun

Notka nieco przegadana i dość elementarna, ale w ten sposób zapewniłem sobie ciągłość i możliwość
zajęcia się rzeczami naprawdę ciekawymi bez szkody dla czytelnika.

Następny wpis (*in progress*) będzie traktował o sposobach manipulacji termami i podtermami oraz o strukturach
danych takich jak listy i tablice(!). Zapraszam.
