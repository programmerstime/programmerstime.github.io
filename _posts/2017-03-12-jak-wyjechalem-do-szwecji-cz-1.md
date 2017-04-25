---
layout: page

title: "Jak wyjechałem do Szwecji, cz. 1"
subheadline: "Zdalna część rekrutacji do szwedzkiej firmy Klarna"

teaser: "Od zawsze wiedziałem, że będę chciał wyjechać za granicę. Wyjechać i pomieszkać
w zupełnie innym miejscu przez bliżej nieokreśloną ilość czasu. Sprawdzić czy gdzie indziej
płynie on inaczej. <br><br>
Poznawać i przebywać wśród ludzi o odmiennym spojrzeniu na świat. Uwarunkowanym
innymi korzeniami, historią i kulturą, zwyczajami czy panującym tam klimatem.<br><br>

To także dla mnie jeden z filarów życiowej wolności: dzisiaj jestem tutaj, jutro mogę być gdzie zechcę..."

category: lifestyle

tags:
  - szwecja
  - kariera

image:
  thumb: "photo-stockholm_thumb.jpg"

header:
  image_fullwidth: "photo-stockholm.jpg"

comments: true

---


To bardzo szeroki dla mnie temat, z którym związanych przemyśleń mam mnóstwo.
Teraz jednak zajmę się jednym szczególnym aspektem -- zmianą pracy. A konkretnie
procesem rekturacji.

## Początek

Konkretne działania rozpocząłem zaraz po obronie magisterki.
Było to już ładnych parę lat po ukończeniu uczęszczania na studia.
Zwyczajnie nie potrafiłem zrobić tego w terminie. Do trzeciego roku
było sporo nauki, a od czwartego praca.

Większość w zasadzie działań odbyła się w [LinkedIn](https://se.linkedin.com/in/rafalpaliwoda).
Zacząłem od odpisania na kilkanaście nieodpowiedzianych wiadomości z ostatniego roku,
które dotyczyły zagranicznych ofert.

Zapytałem wtedy pewnego jegomościa czy nie ma czasem oferty z Holandii lub Niemiec.
On na to, że ma mnóstwo z Londynu (wiadomo) i jedną ze Szwecji.
No to już mieliśmy o czym rozmawiać, bo Skandynawia też mnie interesowała.

Poniżej materiał reklamowy na temat Sztokholmu jakim zostałem uraczony. Udany zresztą.


<div class="flex-video">
    <iframe width="420" height="315" src="//www.youtube.com/embed/CAkdWUjdJyA" frameborder="0" allowfullscreen></iframe>
</div>

Na marginesie, wolałbym być może do jakiegoś kraju typowo anglojęzycznego.
Niestety nie ma żadnego fajnego niedaleko. W Szwecji natomiast każdy po angielsku śmiga.


## Zdalna część procesu rekrutacji

Po odświeżeniu i wysłaniu mojego CV, nadszedł czas pierwszej rozmowy telefonicznej.
Była ona formalnością -- wstępnym rozpoznaniem gruntu. Sprawdzeniem czy intencje obu stron
pokrywają się ze sobą. No i tak naturalnie było.

Następnie dostałem do rozwiązania **test logiczny**. Nie pamiętam dokładnie,
ale posługiwał się on czymś w stylu kropek na szachownicy.
Dla zadanej sekwencji czterech bodaj ustawień, trzeba było dopasować piąte.
Zadanie dość proste, ale wymagało skupienia.

### Zadanie programistyczne

Kolejnym krokiem w procesie było **zadanie programistyczne**. Dotyczyło ono zaprogramowania
prostego RESTowego webserwisu z jednym endpointem -- */decision*,
oczekującym JSONa z danymi użytkownika: emailem, imieniem i nazwiskiem oraz kwoty.

Miał on zwracać decyzję kredytową zgodnie z pewną prostą logiką,
a kolejne wywołania winny zmniejszać dostępny limit kredytowy.

Uproszczeniem był brak wymagania, co do zapisu stanu serwisu w bazie danych.
Językiem miała być Java, a framework dowolny.

Wybrałem Jersey używając minimum potrzebnych technologii. Oryginalny kod dostępny
[na githubie](https://github.com/paliwodar/risk-decision-process).
Podobał się chyba w miarę. Istotnym okazało się dobre pokrycie testami
oraz zapewnienie prawidłowego działania podczas wielu jednoczesnych połączeń.

Poniżej serce aplikacji: kod odpowiedzialny za pobranie aktualnego limitu, podjęcie decyzji
i uwzględnienie ewentualnych zmian w repo.

{% highlight java %}
@POST
@Consumes(MediaType.APPLICATION_JSON)
@Produces(MediaType.APPLICATION_JSON)
public CreditDecisionDTO handleCreditDecisionProcess(CreditProposal creditProposal) {

  performArgumentChecks(creditProposal);
  CreditExposure creditExposure = creditExposureRepository.fetchCreditExposureForEmail(creditProposal.getEmail());

  CreditDecision creditDecision = creditDecisionMaker.makeCreditDecision(creditProposal, creditExposure.getExposure());

  if (creditDecision.isAccepted()) {
    creditExposure.increaseExposure(creditProposal.getAmount());
    creditExposureRepository.persistExposure(creditExposure);
  }
  return CreditDecisionDTO.from(creditDecision);
}
{% endhighlight %}


### Rozmowa

Zwieńczeniem tej części była Skype'owa rozmowa z moją obecną menadżerką.
Dostałem sporo pytań. Nie pamiętam zbyt wielu szczegółów, ale kojarzę, że nie na wszystkie
było łatwo odpowiedzieć.

#### Jak to bywało

Muszę przyznać, że do tej pory -- o ile mnie pamięć nie myli -- spotkałem się z dwoma rodzajami
rozmów rekrutacyjnych. Pierwszy typ, to pytania techniczne.
Jedyne jakie słyszałem na rozmowach np. w Microsofcie czy Amazonie.

Drugi to tzw. rozmowa *haerowa*, traktująca np. o tym co jest naszą największą wadą
lub gdzie widzimy siebie za 5 lat. Przeprowadzana głównie przez osoby,
które nie są decyzyjne, nie mają wiedzy o tym jak się pracuje w engineeringu,
a i też nie zajmują się poszukiwaniem talentów.

Zdarzyło mi się kilka razy, ładnych już parę lat temu, przyjechać do siedziby firmy
jedynie w celu przeprowadzenia tak mało znaczącej konwersacji.
Strzelam jednak, że dziś taką praktykę stosuje się coraz rzadziej.

#### Jak było teraz

No to tutaj miałem do czynienia z typem trzecim. Sprawdzającym samoorganizację,
sprawność w komunikacji, motywację, dopasowanie do zespołu, wartości którymi się kieruję.

Miałem na przykład opowiedzieć o tym, jakie działania podjąłbym, gdybym się zorientował,
że naszemu zespołowi nie uda się dotrzymać jakiegoś terminu.
Jak widzimy, to nie jakieś *rocket science*. Do przejścia.

Te pytania generalnie może i nie różniły się diametralnie od tych z drugiej grupy.
Ale fakt, że były zadawane przez osobę osobiście odpowiedzialną za budowanie kilku zależnych
od siebie zespołów, zmienia całkowicie kontekst.

A był to okres dynamicznego rozwoju firmy. Jak się później okazało, miałem dołączyć do zespołu
w podobnym czasie co dwóch kolegów z Brazylii oraz jeden z Argentyny i jeden z Macedonii.

*To be continued*