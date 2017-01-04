---
layout: page

title: "Nie kupuj nowego routera Wi-Fi. Możesz mieć Raspberry Pi!"
subheadline: "Rzecz o potrzebie nieustannego samodoskonalenia"

teaser: "Potrzeba matką wynalazku. Taki mógłby być (pod)tytuł tego wpisu. Byłoby to jednak
    małe przekłamanie. Niestety niczego wynaleźć mi się nie udało. Skorzystałem z pewnego opisanego już tu i ówdzie rozwiązania.
    <br><br>Niestandardowego jednak. Takie lubię najbardziej. Otóż: nie zamieniłem starego routera bezprzewodowego na nowszy model,
    gdy ten odmówił posłuszeństwa. Kupiłem zamiast tego Rasperry Pi. Taki mały komputer. Mały, ale jary... czy jak tam to leciało..."

tags:
  - Raspberry Pi

image:
  thumb: "photo-berries.jpeg"

header:
  image_fullwidth: "photo-berries.jpeg"

comments: true

#sidebar: right
---

### To nie do końca tak

By nie narażać cierpliwości czytelnika na szwank, od razu powiem: router jest pod wieloma względami
lepszym wyborem, a pod zdecydowaną ich większością wyborem niegorszym.

Jedynym niemalże zagadnieniem,
które może nadać sens skonfigurowaniu własnego routera Wi-Fi (na w zasadzie dowolnym komputerze)
jest możliwość nauczenia się czegoś przy okazji. I ten cel sam w sobie uznaję za niesamowicie istotny.


## Mieć czy umieć?

Sprawa niezmiernie ważna szczególnie dla ludzi związanych z technologiami informatycznymi.
Niestety nie możemy tak jak np. mistrzowie sushi doskonalić się w jednej wąskiej dziedzinie
aż do osiągnięcia perfekcji.

To znaczy oczywiście nikt nam tego nie zabrania, ale nie ma
to większego sensu. A przynajmniej nie w szerszym ujęciu naszej kariery.

Przyjdzie bowiem prędziej czy później taki moment, w którym zorientujemy się,
że oferty pracy w naszym ulubionym języku czy technologii mają coraz więcej
wspólnego z utrzymaniem bieżącego systemu niż z rozwojem nowych funkcjonalności.
To samo dotyczy nowych paradygmatów czy metodologii.

Każdy się już pewnie w tym momencie ze mną zgodzi, że będziemy się musieli uczyć przez większość naszego
zawodowego życia. Być może z małymi przerwami na chwile odpoczynku.

Byle nie wypadać z obiegu na zbyt długi okres czasu.
Najlepszym wydaje się podejście, które pozwoli nam ubić dwa ptaszyska, przy użyciu tylko jednego kamienia:


>Umieszczaj korzyści edukacyjne na wysokim miejscu hierarchii wartości swojej.

Co przez to rozumiem? Otóż nie myśl tylko o tym, co materialnego możesz zyskać dokonując danego wyboru.
Bądź ponad to. Rozpartuj jakie mogą być korzyści długoterminowe. Jakie mogą być nieoczywiste korzyści
wybrania tego drugiego, pozornie bardziej żmudnego i mało opłacalnego wyjścia.

Czasem to czego się nauczysz przy okazji będzie o wiele bardziej cenne i zmieni wyniki ogólnego rozrachunku.
To o czym mówię to tytułowy, często opisywany w literaturze dylemat *mieć czy umieć*[^joke].


{% comment %}
Teraz winno nastąpić miażdzące uderzenie, bo jak to ktoś kiedyś powiedział:

>When zen ends asskicking begins

Oto i ono:

{% endcomment %}

## Lista zakupów

Oto lista składników, które potrzebujemy żeby odpalić co trzeba na naszym Raspberry:

<center>
<img class="t20" src="{{ site.url }}/images/raspberry/IMG_5542.jpg" alt="">
Raspberry Pi 3
<br>
<br>

<img class="t20" src="{{ site.url }}/images/raspberry/IMG_5519.jpg" alt="">
Karta sieciowa Edimax EW-7811Un
<br>
<br>

<img class="t20" src="{{ site.url }}/images/raspberry/IMG_5526.jpg" alt="">
Karta pamieci (8+)
<br>
<br>

<img class="t20" src="{{ site.url }}/images/raspberry/IMG_5531.jpg" alt="">
Może jakaś obudowa by się też przydała
<br>
</center>

## Instrukcja

Teraz praktyka. Niestety nie nastąpi tutaj teraz arcykompletny opis tego jak odpalić punkt dostępu Wi-Fi
na naszym Raspberry Pi. Będzie to jedynie najwierniejszy jak to tylko możliwe zarys.
Myślę że potrzebne szczegóły bez problemu znajdziecie w sieci.

Gorzej jest natomiast ze znalezienien
informacji o tym jak dobrze to działa. Tak aby móc ocenić czy to ma w ogóle sens.
Ja zaryzykowałem i z chęcią dzielę się wynikami w końcowej części artykułu.

#### 1. Rasbian
Zaczniemy od instalacji systemu opercyjnego. Raspery nie posiada dysku. Jego funkcje pelni
karta SD. W tym kroku pobieramy obraz ze strony [Raspbiana](https://www.raspberrypi.org/downloads/raspbian/)
 i nagrywamy go na naszej karcie. Polecam wersje lite. Opis instalacji znajdziemy
 [nieopodal](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

#### 2. Dostęp zdalny
Punkt istotny jeżeli nie mamy osobnej klawiatury i monitora, co miało zastosowanie w moim przypadku.
Nie usuwaj zatem od razu karty ze swojego komputera. Otwórz następujący plik:


{% include alert terminal="
sudo vim etc/wpa_supplicant/wpa_supplicant.conf"
%}

Dopisz na koniec treść następującą:

<pre>
network={
    ssid="network_ssid"
    psk="network_password"
}
</pre>

Krok ten może być kłopotliwy dla użytkowników Mac OS (w tym mnie), z powodu problemów z obsługą systemu plików ext4.
Nie zastanawiając się zbyt długo użyłem do niego mojego laptopa z Ubuntu.


#### 3. Uruchomienie Raspberry
Logujemy się do naszej maszyny za pomocą ssh. Domyślny użytkownik to *pi*, a hasło dla niego to *raspberry*.
Warto od razu zmienić. Następnie uruchamiamy komendy następujące:

{% include alert terminal="
sudo raspi-config \\
sudo apt-get update \\
sudo apt-get install hostapd isc-dhcp-server"
%}

Pierwsza to zmiana ustawień Raspberry --- strefa czasowa, język, etc. Druga pozwoli nam na poszerzenie
rozmiaru dysku tak aby została wykorzystana pełna przestrzeń karty pamięci. Obraz oferowany do pobrania
najwyraźniej zoptymalizowany jest pod kątem objętości.
Ostatnia linijka zainstaluje niezbędne pakiety.

#### 4. Konfigurujemy serwer DHCP

Następny plik wymagajnący edycji to **/etc/dhcp/dhcpd.conf**. Będziemy potrzebowali zakomentować
 linijki poniżej:

<pre>
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
</pre>

na korzyść następujących:

<pre>
# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
authoritative;
</pre>

Dopisując następnie na końcu poniższy fragment:

<pre>
subnet 192.168.42.0 netmask 255.255.255.0 {
        range 192.168.42.10 192.168.42.50;
        option broadcast-address 192.168.42.255;
        option routers 192.168.42.1;
        default-lease-time 600;
        max-lease-time 7200;
        option domain-name "local";
        option domain-name-servers 8.8.8.8, 8.8.4.4;
}
</pre>

Kolej na uzupełnienie <code>INTERFACES="wlan0"</code> w **/etc/default/isc-dhcp-server**.

Interpretację czynności opisanych w tym paragrafie pozostawiam czytelnikowi do samodzielnego obczajenia.


#### 5. Ustawiamy stałe IP

Na wszelki wypadek interfejs *wlan0*:
{% include alert terminal="sudo ifdown wlan0" %}

Edytujemy plik **sudo vim /etc/network/interfaces**. Usuwamy wszystkie wpisy dotyczące *wlan0*.
Dodajemy na koniec poniższy:

<pre>
allow-hotplug wlan0

iface wlan0 inet static
  address 192.168.42.1
  netmask 255.255.255.0
</pre>

Uruchamiamy interfejs ponownie:

{% include alert terminal="
sudo ifconfig wlan0 192.168.42.1"
%}

#### 6. Konfigurujemy hostapd

Teraz pora na hostapd, czyli Host access point daemon. Nazwa mówi sama za siebie.
Edytujemy plik **/etc/hostapd/hostapd.conf**. W swoim eksperymencie użyłem następującej konfiguracji:

<pre>
interface=wlan0
driver=rtl871xdrv
hw_mode=g
ssid=SSID
channel=1
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=PASSPHRASE
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
</pre>

Ponadto dodajemy wpis <code>DAEMON_CONF="/etc/hostapd/hostapd.conf"</code> do pliku **/etc/default/hostapd**

#### 7. NAT, czyli translacja pomiędzy *wlan0* i *eth0*

Dopisujemy <code>net.ipv4.ip_forward=1</code> do pliku **/etc/sysctl.conf**.

Poniższe komendy pozwolą nam na stworzenie minimalnej konfiguracji iptables potrzebnej
do działania routera:

{% include alert terminal="
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE \\
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state \ RELATED,ESTABLISHED -j ACCEPT \\
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
" %}

W razie potrzeby używania Raspberry połączonego bezpośrednio z Internetem polecam
zaopatrzyć się w łańuch reguł akceptujący tylko to, co niezbędne. Można użyć np.
[takiego skryptu](https://gist.github.com/paliwodar/6a18b700658aa885bd74204f377f3f74).

#### 8. Uruchomienie

Startujemy hostapd i serwer dhcp:

{% include alert terminal="
sudo service hostapd start \\
sudo service isc-dhcp-server start"
%}

Sprawdzamy stan serwisów, powinny być *active (running)*:

{% include alert terminal="
sudo service hostapd status \\
sudo service isc-dhcp-server status"
%}

Jeżeli wszystko się powiodło aktywujemy automatyczne uruchamianie przy starcie systemu:

{% include alert terminal="
sudo update-rc.d hostapd enable \\
sudo update-rc.d isc-dhcp-server enable"
%}

Wszystko powinno bardzo ładnie wyglądać i np. tak kolorowo świecić[^wieczko]:

<img class="t20" src="{{ site.url }}/images/raspberry/IMG_5549.jpg" alt="">

#### 9. Dodatki

Polecam wypróbowanie następujących narzędzi:

- watchdog -- proces pozwalający na automatyczny restart naszego Raspberry w przypadku np. zawieszenia
- fail2ban -- pozwoli na blokowanie adresów IP po kilku nieudanych próbach zalogowania
- wondershaper -- umożliwia podanie limitów transferu dla poszczególnych interfejsów sieciowych


## Podsumowanie

Zdaje się, że dość już technicznego mięsa poleciało. Pora teraz na konkluzję.
Otóż nareszcie autor niniejszego bloga dowiedział się ze szczegółami jak działa linuksowe *iptables*.

Było to konieczne, ponieważ Raspberry było połączone se światem beż żadnych pośredników.
Ponadto przybyło +1 do ogólnego obycia w konsoli i konfiguracji systemu.

<div>
<center>
<img class="t20" src="{{ site.url }}/images/raspberry/raspberry_speedtest.png" alt="">
</center>
</div>
<br>

Jak działa sam router możemy zobaczyć na obrazku powyżej. Czy to dobrze, oceńcie sami.
Dodam tylko, że po kablu osiągam około 100 Mb pobierania. Jeżeli posiadasz średnioszybkie
łącze, to prawdopodobnie prawie nie odczułbyś różnicy pomiędzy rozwiązaniem *out of the box*.

Prawie, ponieważ nie tylko prędkość okazała się tutaj istotna, ale także stabilność połączenia.
No i z tym niestety bywało gorzej. Przy trochę większym obciążeniu, jak np. podczas oglądaniu
 filmów w HD z *youtube* czy *netfliksa* przydarzały się permanentne zwiechy,
 na które lekarstwem było jedynie zrestartowanie Raspberry.

Problem okazał się tkwić
w zarządzaniu zasilaniem. Zabiegiem znakomicie redukującym tego typu problemy była
edycja pliku **/etc/modprobe.d/8192cu.conf** poprzez dodanie następującej treści:

<pre>
options 8192cu rtw_power_mgnt=0 rtw_enusbss=0
</pre>

### Mission Accomplished

Eksperyment się powiódł, a już niedługo pojawi się w mieszkaniu router z prawdziwego zdarzenia.
Będzie można wtedy przetestować kolejny z niezliczonej ilości pomysłów na wykorzystanie Raspberry.
No i oczywiście mnóstwo nowych rzeczy przy okazji ogarnąć!


{% comment %}
- speedtest-cli z poziomu Raspberry
- ceny przy zdjęciach składników
- speedtest normalny
{% endcomment %}



[^joke]: Na marne będzie szukać na ten temat wiecej informacji w sieci, zdaje się że najlepsze na co można trafić to: "KAFETERIA - PISZE SIĘ MIEĆ CZY MNIEĆ???" - to chyba jednak nie to...
[^wieczko]: Obudowa posiada także pokrywę, która dla lepszego efektu została zdjęta