---
layout: page

title: "Bezpiecznie i wygodne przechowywanie danych w Dropbox: Truecrypt vs EncFS"
#subheadline: ""

teaser: "Większość z nas przechowuje chociaż część swoich danych w różnego rodzaju ,,chmurach'',
         których mamy teraz bogaty wybór. Niniejszy wpis traktuje o aspektach bezpiecznego z nich korzystania,
         które przedstawione zostaną na podstawie Dropboksa, a znajdą z pewnością
         także zastosowanie w przypadku innych rozwiązań.
         <br><br>
         Interesującym nas zagadnieniem jest szyfrowanie danych. A w zasadzie jego brak.
         Bo o ile pliki przesyłane w bezpieczny sposób, to już przechowywane są w swojej zwyczajnej postaci.
         <br><br>I tutaj pomimo zapewne wysokich standardów bezpieczeństwa, teoretycznie,
         mogą uzyskać do nich dostęp osoby trzecie. Co Dropbox oczywiście przyznaje..."

category: dev

tags:
  - dropbox
  - bezpieczeństwo
  - encfs

image:
  thumb: "photo-skrytki.jpeg"

header:
  image_fullwidth: "photo-skrytki.jpeg"

gallery:
    - image_url: veracrypt/create_container.png
    - image_url: veracrypt/container_location.png
    - image_url: veracrypt/veracrypt_algorithms.png
    - image_url: veracrypt/veracrypt_volume_size.png
gallery_row: 2

gallery2:
    - image_url: dropbox/private_dropbox.png
    - image_url: dropbox/dropbox_private.png
gallery2_row: 2

comments: true

---
## O co tak naprawdę chodzi?

W centrum pomocy możemy przeczytać następującą treść:

>Jak w przypadku większości serwisów online, mamy kilku pracowników,
którzy muszą mieć możliwość uzyskiwania dostępu do danych użytkowników z
powodów określonych w naszej polityce prywatności (np. gdy jest to nakazane przez prawo).
Ale to rzadki wyjątek, nie reguła.
<cite>[Dropbox: Bezpieczeństwo i prywatność](https://www.dropbox.com/help/topics/security_and_privacy)</cite>

Abstrahuję tutaj zupełnie od prawdopodobieństwa takiego zdarzenia oraz
zasadności potrzeby uchronienia się przed nim. To taka wzmianka dla każdego,
kto powiedziałby, że jak takie dane cenne, to po jakiego je tam wrzucać?

Pan Dropbox natomiast odparłby na to treścią następującą:

>Chociaż Dropbox nie oferuje szyfrowania po stronie klienta, użytkownicy mogą
swobodnie dodawać własne zabezpieczenia. Istnieje wiele aplikacji firm trzecich,
które zapewniają szyfrowanie zarówno na poziomie plików jak i ich nośnika.
<cite>[Dropbox: Bezpieczeństwo i prywatność](https://www.dropbox.com/help/28)</cite>

Jednocześnie wspominając na dodatek, że rozważa się wprowadzenie takich mechanizmów w przyszłości.
Mamy zatem dowód, że to całkiem niegłupi tok myślenia.
No i właśnie niektóre sposród tych aplikacji firm trzecich
tutaj poruszymy. A konkretnie dwa.


## TrueCrypt

Znane i do niedawna powszechnie używane narzędzie open source do szyfrowania, którego rozwój został
niedawno zakończony. Na jego stronie znajdziecie informację, że jego używanie jest potencjalnie niebezpiecznie
z powodu możliwych luk w zabezpieczeniach.

Ma jednak (ponoć) godnych następców.
Jednym z jego nich jest VeraCrypt. W artykule używam nazwy pierwotnego
projektu głównie ze względu na jego dużo większą rozpoznawalność.

Pozwala on na tworzenie zaszyfrowanych partycji i kontenerów. Te ostatnie były użyteczne dla mnie
podczas korzystania z Dropboksa. Kontener ma postać zwykłego pliku, który możemy umieścić
w katalogu w chmurze.

Otwieramy go montując jako samodzielny dysk przy użyciu VeraCrypt.
Po zamontowaniu możemy edytować zawartość. Po zakończeniu dokonywania zmian,
odmontowujemy dysk, by upewnić się że wszystkie zmiany zostały zapisanie w pliku naszego kontenera.


### Istrukcja

Poniżej skrócona wizualizacja tego jak stworzyć taki kontener.

{% include gallery %}

Wybieramy rekomendowaną opcję kontenera w postaci pliku.
Następnie wybieramy lokalizację, algorytmy szyfrowania oraz rozmiar folderu.
Tak utworzony kontener możemy zamontować uzyskując dostęp do jego zawartości.

### Mankamenty tego rozwiązania

Niestety podwyższony poziom bezpieczeństwa musimy okupić w tym przypadku akceptacją kilku niedogodności.
Pierwszą z nich jest **brak możliwości jednoczesnej edycji naszego kontenera**.
Kontener TC to czarna skrzynka.

Choćbyśmy zmienili jeden plik, to po zaszyfrowaniu
zawartość kontenera może się zmienić nie do poznania. Naturalnym więc jest,
że próba synchronizacji zawartości po dwóch jednocześnych modyfikacjach[^modyfikacja]
zwyczajnie prowadzi do otrzymania losowej papki zmielonych bajtów.

Z drugiej strony, do szczególnie wrażliwych danych raczej będziemy sięgać nieczęsto, a jak już, to dokonując
 tylko jednej edycji jednocześnie.

Innym -- i to niestety dużo bardziej uciążliwym ograniczeniem -- jest **brak możliwości
synchronizacji plików znajdujących się poza katalogiem Dropboksa**. Otóż, dość wygodnym
rozwiązaniem jest np. stworzenie linka symbolicznego do innego folderu, jak np. pulpitu
czy dokumentów, tak aby foldery te miały swoje aktualne kopie w chmurze[^synchronizacja].

Niestety rozwiązanie z linkami w tym przypadku nie zadziała, gdyż VeraCrypt nie ,,podąży'' za nimi
podczas zapisu kontenera. Pewnie można by to obejść tworząc linki w drugą stronę,
tzn. z pulpitu do jakiegoś katalogu w naszym kontenerze, ale wtedy musielibyśmy
 go mieć zamontowanego przez cały czas. No i tutaj pojawia się kolejny problem.

Z moich obserwacji wynika, że tak **zamontowany kontener jest widoczny dla wszystkich
użytkowników** zalogowanych na danym komputerze. Być może da się tej kwestii jakoś
zaradzić. Ja spędziłem nad tym kwadrans bez osiągnięcia pożądanego rezultatu.

I wreszcie ostatnią sprawą jest utrata takich funkcjonalności jak
**możliwość przywrócenia wcześniejszych wersji plików** czy **współdzielenia ich**.

Podsumowując -- działa, ale trochę zbyt topornie.

## EncFS

W praktyce używanie EncFS okazuje się być podobne do TrueCrypta.
Istnieje jednak kilka różnic, które pozwalają na otrzymanie
trochę lepszego *user experience*.
Otóż EncFS zachowuje strukturę plików i katalogów, szyfrując także oczywiście ich nazwy.

Dzięki temu może obsługiwać jednoczesną edycję, współdzielenie i wersjonowanie
plików. Wszystko niestety za cenę obniżonego poziomu zabezpieczeń -- o tym za chwilę.


### Instrukcja

Sama instalacja jest bardzo prosta. W przypadku Mac OS wystarczy użyć brew:

{% include alert terminal="
brew install homebrew/fuse/encfs" %}

Potem sprawę załatwia w zasadzie jedna komenda:

{% include alert terminal="
encfs ~/Dropbox/Private ~/PrivateDropbox" %}

Konsekwencje wykonania kodu powyższego mogą być dwie: rozpoczęcie procesu tworzenia
zaszyfrowanego woluminu bądź jego zamontowanie.

W przypadku pierwszym konieczne będzie
odpowiedzenie na kilka pytań np. o to czy katalogi powinny zostać utworzone,
a także prośba o wybranie opcji spośród: *expert configuration mode* oraz *pre-configured
paranoia mode*.

Wybranie tej drugiej wydaje się dość bezpiecznym krokiem.
W przypadku drugim będzie chodziło o zamontowanie utworzonego wcześniej kontenera.
Wystarczające będzie wtedy podanie hasła.


Pojawić się może pytanie: dlaczego pierwsza ścieżka prowadzi do katalogu Dropboksa?
Otóż jest to katalog, w którym przechowywana będzie zaszyfrowana wersja naszych plików.
Dostępna powinna tam być zawsze.

Zamontowanie woluminu umożliwi nam dostęp do
plików i folderów w postaci odszyfrowanej -- w lokalizacji podanej jako drugi argument.
Istotna jest tutaj jedna, dość oczywista zasada: nigdy nie modyfikujemy ręcznie tego pierwszego folderu!


### Usprawnienia warte uwagi

Po pierwsze, warto skonfigurować automatyczne montowanie woluminu EncFS podczas każdego logowania.
Jestem pewien, że nie jest to trudne do znalezienie dla dowolnego systemu operacyjnego.
Osobiście korzystałem z [następującej instrukcji](https://www.andreagrandi.it/2015/10/11/how-to-configure-encfs-on-osx-10-10-yosemite/).

I druga rzecz. Niesamowicie wygodnym rozwiązaniem jest *automatyczna* synchronizacja specjalnych folderów,
takich jak Documents. Proponuję na to sposób następujący. W katalogu <code>~/PrivateDropbox</code>,
umieszczamy folder <code>PrivateDocuments</code>. Następnie tworzymy link symboliczny
do tego folderu w naszych dokumentach -- w moim przypadku <code>~/Drocuments/DroppedDocuments</code>.

I teraz ilekroć zapisuję jakiś plik w moich dokumentach, to mogę zdecydować czy
 powinien się on w zaszyfrowanej wersji pojawić na Dropboksie. Jeżeli tak,
 to plik ów ląduje w <code>DroppedDocuments</code>. I już! Poniżej screenshoty katalogów
 <code>~/Dropbox/Private</code> oraz <code>~/PrivateDropbox</code>.

{% include gallery2 %}

### Bezpieczeństwo

W kwiestach bezpieczeństwa naturalnym mankamentem jest fakt, że EncFS zachowuje niezmienioną
strukturę katalogów i plików. Każy mający dostęp do zaszyfrowanego katalogu
może zobaczyć ile mamy plików, ich rozmiary, prawa dostępu, daty modyfikacji, itd.

Może to się komuś nie podobać, ale w przypadku nie-tak-znowu-łatwego dostępu
do moich danych na Dropboksie, nie uważam to za powód do zmartwień.

Ponadto w 2014 roku przeprowadzony został [audyt bezpieczeństwa EncFS](https://defuse.ca/audits/encfs.htm),
który wykazał on istnienie potencjalnych zagrożeń.


## Nie jest jeszcze idealnie, ale da się żyć

Podsumowania długiego nie będzie. Rozwiązanie znakomicie spełnia oczekiwania autora,
nie planuje on zatem dużych zmian w tym temacie w najbliższej przyszłości.
Ale jakby ktoś chciał obadać temat, to być może warto rzucić okiem np. na
[CryFS](https://www.cryfs.org/comparison).


[^modyfikacja]: Nawet zupełnie różnych plików w obrębie tego samego kontenera
[^synchronizacja]: Opisuję to rozwiązanie w dalszej części artykułu w kontekście EncFS.