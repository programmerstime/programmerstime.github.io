---
layout: page

title: "Jak założyć profesjonalnego bloga za darmo: Jekyll i GitHub Pages, cz. 1"
subheadline: "Nie tylko dla blogerów"

teaser: "Prawdopodobnie inauguracyjny wpis na niniejszym blogu. O czymże więc miałoby być,
        jeżeli nie o początku przygody z technicznymi aspektami jego założenia.
        W moim przypadku wybór padł na Github Pages. Rozwiązanie pełne zalet.
        Bardzo wygodne dla kogoś korzystającego z gita na co dzień. Jeszcze lepsze dla pozostałych."

tags:
  - blog
  - git
  - jekyll

image:
  header: ""
  thumb: "photo-typewriter-2.jpeg"
  caption: "Caption?"
  url: ""

header:
  image_fullwidth: "photo-typewriter-2.jpeg"

comments: true

---

A to za sprawą faktu, że opanujemy podstawy tego systemu kontroli wersji i przyzwyczaimy się do pracy z nim
zupełnie za darmo. Za darmo zresztą będzie cały nasz blog -- nie licząc kosztu samej domeny.
To oczywiście nie wszystkie zalety.

Zastanawiając się nad wyborem silnika blogowego tak naprawdę rozważałem tylko dwie opcje:
Wordpressa oraz GitHub Pages. Jak na typowego backend developera przystało.
O ile o pierwszym słyszeli prawie wszyscy, w tym autor tego bloga ---
frontendowo-webmasterski laik, to już szczegóły drugiej opcji mogą nie być wszystkim znane.
U większości jednak powinny występować przynajmniej pozytywne skojarzenia. A jeżeli nie występują,
to po lekturze tego artykułu z pewnością będą. A zatem do dzieła!


<h2>Git...</h2>

Nie wydaje mi się, żeby zamieszczenie tutaj szczegółowych informacji na temat podstaw gita
 miało jakiś głębszy sens.
Lepiej może zrobić to <a href="http://pl.wikipedia.org/wiki/Git_(oprogramowanie)">wikipedia</a>.
W praktyce natomiast dobrze pokaże to np. <a href="https://www.codecademy.com/learn/learn-git">CodeCademy</a>.

{% include alert text='
<h6>Dla początkujących</h6>
<smaller>Przydałoby się jednak być może chociaż zawrzeć informacje absolutnie niezbędne.
Git zatem systemem kontroli wersji jest. Jest to coś koniecznego przy pisaniu jakiegokolwiek projektu.
System taki pozwala na śledzenie zmian w kodzie źródłowym oraz ułatwia łączenie zmian
napisanych przez wielu programistów niezależnie.</smaller>'
%}

Być może kiedyś powstanie artykuł opisujący temat bardziej rzetelnie.
Ja ze swojej strony mogę powiedzieć że:

> Gdyby mi zaproponowano pracę w projekcie wykorzystującym system kontroli wersji istotnie inny[^inny] niż git,
to bezwzględnie odmówiłbym.
<cite>autor tego bloga</cite>

Powinno mówić samo za siebie.


<h2>GitHub...</h2>

GitHub to serwis hostingowy dla repozytoriów gita. Jeden z najbardziej popularnych.
Posiada co najmniej 14 milionów użytkowników i ponad 35 milionów repozytoriów.
Możemy mieć zatem względą pewność, że nasze dane nagle nie wyparują.

W bezpłatnej opcji dostępne są tylko publiczne repozytoria. Są one jednak wystarczające
dla wielu programistów dla większości niekomercyjnych projektów.
Nie jest to zatem coś czym należy się martwić. W końcu treści bloga i tak są udostępniane.
Chyba że ktoś ma jakiś niesamowity szablon, którego nie chciałby pokazywać innym.
Ja bazuję na <a href="https://phlow.github.io/feeling-responsive/">feeling responsive</a>,
którego nieco dla moich potrzeb zmodyfikowałem. Stawiam hipotezę, że publiczne repozytorium
jest dla bloga wystarczające.



<h2> GitHub Pages!</h2>
Opowieści ciąg dlaszy. Pora teraz na GitHub Pages. Funkcjonalność oferującą
hosting statycznej strony projektu bezpośrednio z poziomu repozytorium GitHuba.
Czyż nie jest to bardzo miłe z ich strony? Nie dość, że przechowują nasz kod
oferując znakomite wsparcie do pracy z nim, to jeszcze hostują jego stronę.

Oczywiście są tutaj ograniczenia. Są to 1 GB na repozytorium strony,
100 GB ruchu sieciowego oraz 100,000 żądań http miesięcznie. Dla niektórych może to być niewiele,
ale na początek na pewno wystarczy. Ponadto możemy podpiąć swoją domenę.
W przeciwieństwie np. do wordpress.com --- bez żadnych dodatkowych opłat.

<h2>Instruktaż</h2>

Pora ubrudzić sobie ręce i zobaczyć jak to wygląda w praktyce.

<div class="flex-video">
    <iframe width="420" height="315" src="//www.youtube.com/embed/ggX5nag3aGg" frameborder="0" allowfullscreen></iframe>
</div>

Powyżej możecie zobaczyć krótki filmik demonstrujący proces stworzenia najprostszej możliwej strony.
Mającej na celu tylko jedno -- przywitanie się ze światem.

W celu dodania zawartości naszej strony użyliśmy następujących komend gita:

{% include alert terminal="
git clone [url repozytorium]

git add index.html

git commit -m 'dodano index.html'

git push origin master"
%}

Wykonują one kolejno:

1. Skopiowanie repozytorium na nasz komputer
2. Dodanie pliku index.html do kontroli wersji
3. Wysłanie pliku index.html do naszego lokalnego repozytorium z podanym komentarzem
4. Wysłanie zmian do repozytorium githuba


<h2> CDN... </h2>

O tym co dalej, dowiemy się niebawem w kolejnym odcinku. Zajmiemy się w nim Jekyllem.
Spróbujemy wtedy opisać gdzie możemy znaleźć odpowiedni dla nas szablon oraz jak go użyć na naszej stronie. Zapraszam!


[^inny]: Istotnie inny, czyli bardziej SVN niż np. Mercurial
[^publiczne]: Nie jestem tutaj pewien czy uczynienie go prywatnym zmieniłoby sytuację w przypadku repozytorium strony

