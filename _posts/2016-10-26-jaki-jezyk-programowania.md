---
layout: page

title: "Jaki język programowania?"
subheadline: "Pytanie nie tak trudne jak się może wydawać"
teaser: "W tematach okołokomputerowych, chyba nawet częściej niż w ,,normalnym'' życiu,
         sporo jest miejsca do zażartych dyskusji typu: ,,o wyższości Świąt Bożego Narodzenia nad Świętami Wielkiej Nocy''.
         Oto jeden z bardziej popularnych tematów."

category: dev

tags:
  - kariera
  - programowanie

image:
  thumb: "photo-tools.jpeg"

header:
  image_fullwidth: "photo-tools.jpeg"

comments: true
---

>Jakiego języka programowania powinienem się uczyć?


To bodaj najczęściej zadawane pytanie na wszelkich forach dotyczących programowania.
I choć odpowiedzi padło już tysiące, naszła mnie ochota dorzucenia swoich pięciu centów.

Od niepamiętnych czasów nieświadomi użytkownicy Windows spierali sie z linuksiarzami (teraz obie grupy
zdają się być pogodzone przez Maca) czy miłośnicy Vima z sympatykami Emacsa.
Podobnie tytułowe pytanie jest nader często punktem zapalnym takich świętych wojen.

Ilekroć ktoś zwróci się z tego typu prośbą dostaje sporą dawkę ,,dobrych rad'',
przeczących sobie nawzajem, nierzadko przy akompaniamencie obelg wymienianych pomiędzy ich autorami.
Efektem czego jest jeszcze większa dezorientacja. Te cenne i celne odpowiedzi potrafią zginąć w stogu siana.

Tak przynajmniej było w dużej mierze kiedy ja zaczynałem swoją przygodę z programowaniem.
Na szczęście powstały serwisy typu Q/A jak np. [Stack Overflow](http://www.stackoverflow.com) -- pozwalające na sprawniejsze
oddzielanie ziaren od plew.

Odpowiedź skrajna, bardzo subiektywna, nie biorąca pod uwagę wszystkich
możliwych przypadków nie zostanie oceniona przez większość jako pomocna.

Dlaczego taka różnica zdań? Powodów jest wiele. Jednym z nich może być fakt,
że informatyka to niezmiernie szeroka dziedzina.
Programowanie programowaniu nierówne.

Dwie osoby mogą mieć zupełnie przeciwne poglądy,
będąc jednocześnie niezbyt daleko od prawdy. Bardzo dużo zależy tutaj od punktu widzenia.
Ktoś tworzący oprogramowanie dla systemów kontroli lotów będzie zwracał uwagę
na nieco inne aspekty niż programista aplikacji webowych.

Przedstawiam poniżej moje spojrzenie na wspomniane tematy. Pewnie także niepozbawione opinii.


<h2>Który język programowania jest najlepszy?</h2>

Spór sponsorowany przez powyższy tytuł uważam za zupełnie pozbawiony sensu.
Naśladując wiele wypowiedzi w tym temacie, staram się zawsze rozumieć język jako narzędzie.
Tak jak młotek czy wiertarka powinien on być dostosowany do konkretnego przypadku użycia.

Kiedy tego przypadku użycia nie ma -- sięgamy w odpowiedzi do naszych uczuć i osobistych preferencji.
A jak wiadomo, to są sprawy, których nie da się zanegować. Nie możemy przecież powiedzieć:

>Jesteś w wielkim błędzie, kiedy mówisz, że lubisz Pythona.

Ciekawie jest posłużyć się tutaj wszelkiego rodzaju analogiami. Niektórzy porównują języki do samochodów,
mówiąc, że C to superszybki samochód wyścigowy, potrafiący się zepsuć po każdych 50 kilometrach.

Java natomiast może być porównana do autobusu, który jest duży, ciężki i dobrze sobie radzi
z wszelkiego rodzaju niewielkimi kolizjami. Asembler jest wtedy gołym silnikiem, do którego nadwozie
zbudować trzeba samemu!


<h2>Który jezyk programowania jest najlepszy na początek?</h2>

Dodanie tego dopełnienia nadaje poprawny kontekst. Wiadomo wtedy, że przypadkiem użycia
języka programowania jest nauka programowania sama w sobie. Jakie zatem kryteria możemy sformułować w tej sytuacji?

Po zastanowieniu dochodzę do wniosku, że dużo może zależeć tutaj od okoliczności konkretnego przypadku.
Na przykład od poziomu chęci i motywacji danej osoby.

<h4>Motywacja i oczekiwania</h4>

Języki mają różną krzywą uczenia się.
Może ona być bez znaczenia dla osoby, która jest pewna, że będzie programować.
W przeciwnym przypadku język zbyt trudny na początek może zniechęcić do nauki w ogóle.

Teraz a propos oczekiwań. Niestety w tych czasach oczekiwania mamy bardzo duże.
Do wszystkiego. W szczególności komputerów, urządzeń mobilnych, do programów i aplikacji na nich zainstalowanych.

W takich warunkach spędzenie kilku godzin na pisaniu aplikacji konsolowej, która wypisuje
trójkąt Pascala, może się wydawać zwyczajnie mało atrakcyjne.
Można to zniwelować wybierając język/platformę oferujące
więcej wodotrysków, jeżeli tylko czujemy się wtedy bardziej zmotywowani.


<h4>Dla początkujących hakerów</h4>

Hakerskim adeptom poleca się jak najwięcej eksperymentować. Próbować języków wspierających
różne techniki programowania: obiektowe, funkcyjne, programowania logicznego, współbieżnego.

Ważne żeby wiedzieć, że różne podejścia do programowania istnieją i różne języki
oferują swoje mechanizmy do rozwiązywania określonych problemów.
Zrozumienie ich natomiast przynieść może nam wyłącznie korzyści.


{% include alert text='
<h6>Subiektywna opinia</h6>
<smaller>No dobra, tyle tekstu i zero konkretów. A zatem: bardzo dobrym wyborem na pierwszy język programowania
będą: Python, Java, Groovy, Ruby, Scala, Haskell, Go. Tak w ogóle to dobrze napisać cokolwiek
w każdym z nich. No i oczywiście także w Prologu i Clojure!</smaller>'
%}

W razie gdyby powyższa opinia wywołała zbyt duże emocje, przytaczam mit nr 1:

{% include alert info='
<b>Mit #1</b><br>
<smaller>To bardzo ważne od jakiego języka rozpoczniesz naukę programowania.</smaller>'
%}

A tak naprawdę, to warto pójść dużo dalej niż ,,napisanie czegokolwiek''.
Za szczególnie istotne uważam te elementy języka, które wyróżniają go spośród innych.
Może to być na przykład Software Transactional Memory w Clojure,
transformacje Groovym czy aktor(z)y w Scali.

<h4>Moje początki</h4>
Ja swoje pierwsze programistyczne kroki postawiłem w Basicu. Był to jednak niezbyt wiele znaczący
epizod. Kiedy znudzony grami na Amigę 2000, szperałem w ,,Workbenczu''. Udało się wtedy niemalże
wyłącznie przy pomocy instrukcji ,,if'' napisać króciutką, ściśle liniową, tekstową grę fabularną.

Jeszcze przed liceum w ręce wpadł mi Turbo Pascal, w którym zdarzyło się popełnić kilka prostych programów.
W szkole natomiast przepisywaliśmy programy Logo z jakichś kserówek. Nic nie zapowiadało spektakularnej kariery ;)

Następne wybory były dość proste -- w liceum Olimpiada Informatyczna nie wspierała zbyt wielu języków.
Dlatego też wyborem domyślnym dla ludzi nie różniących się istotnie ode mnie wiekem był C/C++.
Ot i cała historia.


<h2>W jaki język warto zainwestować?</h2>

Na to pytanie z kolei już nie tak łatwo odpowiedzieć, nie znając konkretnego przypadku.
Co kto programować lubi, jaki jest cel, ile chce zarabiać, i tym podobne...
Analiza wszystkich lub nawet chociaż większości wydaje się mało zachęcająca.

Mogę jednak przedstawić spojrzenie na temat z mojej perspektywy.
Postanowiłem zatem rzucić z rękawa radami następującymi poniżej.
Lista ta została sporządzona z myślą o kimś zainteresowanym szeroko pojętym (komerycyjnym?)
tworzeniem oprogramowania.

Nie obejmuje przypadków brzegowych, takich jak zastosowanie naukowe,
robotyka, programowanie dla NASA czy CERN.

1. Jeżeli interesuje Cię frontend, mobilne lub design/grafika - pomiń resztę punktów.
2. Pythona zawsze warto dobrze poznać.
3. Powyższe aplikuje się także do podstaw basha, przydawał się chyba w każdym projekcie.
4. Zainteresowanym programowaniem niskopoziomowym, realistyczną grafiką lub tworzeniem gier nie mam zbyt wiele do powiedzenia,
pozostałych do programowania w C/C++ w tych czasach subiektywnie zniechęcam[^c].
5. Ci po drugiej stronie mocy znają .Net -- C#, F# -- do roztropnego rozważenia.
6. Java nieustannie cieszy się popularnością, ciągle atrakcyjna sposobność, szczególnie biorąc pod uwagę punkt następny.
7. Inne języki oparte na JVM: Scala, Groovy, Clojure -- to z grubsza moje zainteresowania, nie reklamuję by nie zostać
posądzonym o stronniczość.
8. Ruby - kilka zespołów w mojej firmie używa, a i znajomych kilku mam.
9. SQL - wypada wiedzieć z czym się to je.
10. Mówią, że warto spróbować czegoś takiego jak Go czy Kotlin.

{% include alert warning='
<smaller><b>Disclaimer:</b> lista powyższa dostarczana jest ,,taką jaką jest" bez jakichkolwiek gwarancji jawnych
         ani domniemanych, w tym między innymi gwarancji przydatności.</smaller>'
%}


[^c]: Chyba że ktoś ma inny dobry powód. Słyszałem opinie, że każdy dobry programista musi nauczyć się kiedyś operowania wskaźnikami na funkcje w C. Trudno o większą bzdurę.