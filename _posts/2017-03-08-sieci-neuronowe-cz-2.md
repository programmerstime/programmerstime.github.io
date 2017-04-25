---
layout: page

title: "Wprowadzenie do sieci neuronowych cz. 2"
#subheadline: ""

teaser: "To druga część pościgu za sieciami neuronowymi. Pojawienie się nowych postaci,
 skomplikowanie fabuły, zawiązanie akcji. Pełen emocji punkt kulminacyjny
  oraz zakończenie. Takie niby melancholijne, ale jednak podszyte wiarą i nadzieją.
 <br><br>Wszystko co potrzebne żeby opowiedzieć interesującą historię.
Prawie jak ,,Stary człowiek i morze''. Tylko sieci już nie te."

category: dev

tags:
  - sztuczna inteligencja
  - machine learning

image:
  thumb: "photo-lighttree2.jpg"

header:
  image_fullwidth: "photo-lighttree2-main.jpg"

comments: true

---

## Funkcje Boolowskie

Jest tu potrzebna odrobina cierpliwości. Sieci neuronowe czają się tuż za rogiem. Wierzcie mi -- będziecie zaskoczeni.
Teraz jednak pora wspomnieć o pewnej interpretacji funkcji Boole'a.

Spróbujemy je wyrazić za pomocą poznanej już w [poprzedniej części](/sieci-neuronowe-cz-1/), regresji logistycznej.
Weźmiemy pod lupę takie byty jak AND, OR oraz NOT. Łatwizna! Ale niekoniecznie z tej perspektywy.

#### Negacja

Mamy w tym przypadku jedną zmienną wejściową \\(x\in\\{0,1\\}\\). Wzór na regresję logistyczną powinien być tutaj dość prosty:
$$ g(\Theta_0 + x\Theta_1) = \neg x $$.

Czyli zwyczajnie:

$$g(\Theta_0 + \Theta_1) = 0 \quad g(\Theta_0) = 1,$$

co przez definicję funkcji *sigmoid* jest równe:

$$\Theta_0 + \Theta_1 < 0 \quad \Theta_0 > 0$$

Znalezienie odpowiednich wartości \\(\\Theta\\) nie powinno nam sprawić trudności.
Może ona na być na przykład następująca: \\(\Theta = [10, -20]\\).
Poniższa figura znakomicie obrazuje sytuację:

![Negation perceptron](/images/neural_networks/negation.svg){: .center-image .scaled-down-45-image}

#### AND

Rozgrzewkę mamy już za sobą. Przejdziemy więc od razu do sedna.

<!-- For the sake of completeness let's take a look at the table describing logical conjunction behaviour:

$$ \begin{array}{|c|c|c|}
		\hline
		x_1 & x_2 & x_1 \land x_2  \\
		\hline
		1 & 1 & 1 \\
		1 & 0 & 0 \\
		0 & 1 & 0 \\
		0 & 0 & 0 \\
		\hline
\end{array} $$

-->
Potrzebujemy zatem \\(\Theta_0 + x_1\Theta_1 + x_2\Theta_2 = x_1\land x_2\\) dla wszystkich kombinacji.
Przykładowym rozwiązaniem może być: \\(\Theta = [-30, 20, 20]\\).
Łatwo zauważyć, że \\(20x_1 + 20x_2 - 30 > 0\\) wtedy i tylko wtedy, gdy \\(x_1 = 1\\) and \\(x_2=1\\).

Graf wyrażający powyższe:

![And perceptron](/images/neural_networks/and.svg){: .center-image .scaled-down-45-image}

#### OR

Pominiemy teraz szczegóły. Przyjrzyjmy się jednak proponowanej wizualizacji logicznej alternatywy:

![Or perceptron](/images/neural_networks/or.svg){: .center-image .scaled-down-45-image}

## Troszkę bardziej wymagająca funkcja: XOR

XOR, czyli *exclusive disjunction* jest funkcją, która zwraca prawdę wtedy i tylko wtedy,
gdy argumenty do ktorych została zaaplikowana są różne. Tabela prawdy:

$$ \begin{array}{|c|c|c|}
		\hline
		x_1 & x_2 & x_1 \oplus x_2  \\
		\hline
		1 & 1 & 0 \\
		1 & 0 & 1 \\
		0 & 1 & 1 \\
		0 & 0 & 0 \\
		\hline
\end{array} $$

Na marginesie dodam, że XOR jest kanonicznym przykładem funkcji, która nie może być wyrażona przy użyciu
najprostszych form regresji liniowej. Może to brzmieć mgliście, wszystko wyjaśni jednak poniższy wykres:

![Xor_chart_empty](/images/neural_networks/xor_chart_empty.svg){: .center-image .scaled-down-50-image}

#### Regresja wielomianowa

Jak widzimy nie jesteśmy w stanie poprowadzić prostej segregującej punkty tego samego koloru.
Jakie mamy zatem wyjście? Możemy oczywiście wprowadzić więcej zmiennych używając regresji wielomianowej.

Przykładem równania mającego zastosowanie w naszym przypadku może być:
 $$ 2x_1 + 2x_2 - 5x_1x_2 - 1 > 0 $$ --- widoczne poniżej:

![Xor_chart](/images/neural_networks/xor_chart.svg){: .center-image .scaled-down-50-image}

#### Dopasowanie elementów układanki

Możemy jednakowoż spróbować alternatywnej metody rozwiązania problemu XOR. Wymagać będzie ono
dokładniejszego przyjrzenia się definicji naszej funkcji. A wyglądać może ona na przykład tak:

$$
\begin{align*}
x_1 \oplus x_2 &\equiv \neg (x_1 \land x_2) \land (x_1 \lor x_2)   %\\
	%&\equiv (\neg x_1 \lor \neg x_2) \land (x_1 \lor x_2)
\end{align*} $$

Spróbujmy teraz połączyć funkcje, które do tej pory udało nam się już rozpracować.
 Powinniśmy starać się otrzymać coś w tym stylu:

![Xor gates](/images/neural_networks/gates.svg){: .center-image .scaled-down-40-image}

Jest to schemat naszego rozwiązania posługujący się bramkami logicznymi.
Naszym zadaniem będzie przekonwertowanie go na język perceptronów.

Zdaje się, że ostatnią brakującą rzeczą jest zauważenie, że \\(\Theta_{\text{NAND}} = -\Theta_{\text{AND}}\\).

Mamy zatem wszystkie elementy, których potrzebujemy do budowy... SIECI NEURONOWEJ!

![Xor](/images/neural_networks/xor.svg){: .center-image .scaled-down-70-image}

## Reprezentacja

Dotarliśmy wreszczie do celu. Jest to kwintesencja obu wpisów poświęconym temu tematowi.
Wyglada na to, że sprawa jest wręcz banalnie prosta, prawda?

Lubię rozumieć sieć neuronową jako wielopoziomową regresję logistyczną.
Na każdym poziomie posługujemy się zmiennymi, będącymi rezultatem kalkulacji z poziomu poprzedniego.

Łatwo zatem zauważyć, że mechanizm ten jest rzeczywiście zbliżony do ludzkiego systemu nerwowego.
Możemy postrzegać parametry wejściowe sieci jako bodźce odczytywane przez nasze opuszki czy receptory smakowe.

Bodźce te, są następnie przetwarzane przez sieć neuronów, które nieustannie adaptują się w miarę konsumowania
 nieznanych jeszcze do tej pory danych.

Wracając do naszego matematycznego wątku -- niestety nie zagłębimy się tutaj w szczegóły.
Mam nadzieję, że lektura niniejszego artykułu ułatwi pracę z kolejnymi źródłami,
skoro naumiane już zostały podstawy.

Rzućmy jednak jeszcze okiem na reprezentację sieci neuronowych.
Okazuje się, że naturalnie jest opisywać \\(\Theta\\)  jako wektor macierzy -- reprezentujących
poszczególne warstwy. Przy takim podejściu sieć z powyższego obrazka będzie opisana w sposób następujący:

$$

\begin{align*}
\Theta^{(1)} &=    \begin{bmatrix}
    \color{purple}{30} & \color{olive}{-10} \\
    \color{purple}{-20} & \color{olive}{20} \\
    \color{purple}{-20} & \color{olive}{20}
    \end{bmatrix} \\
\Theta^{(2)} &=  \begin{bmatrix}
    \color{teal}{
    -30 \\
    20 \\
    20 }
    \end{bmatrix}
\end{align*}
$$

Całość obliczeń wyraża się ładnie wtedy następującym wzorem:

$$ h_\Theta(x) = g\left(g\left( x^T \Theta^{(1)} \right) \Theta^{(2)}\right)
$$

## Podsumowanie

Wspomnieć należy, że posługiwanie się wytrenowaną siecią neuronową, jest wględnie proste.
Przy ustalonych wartościach \\(\Theta\\), jest to tylko kwestią przeprowdzenia algorytmu *forward propagation*,
opisanym mniej więcej powyższym wzorem.

Z drugiej strony znalezienie tych wartośći (\\(\Theta\\)) nie jest już tak trywialną rzeczą.
Pełne zrozumienie tej części wymaga zapoznania się z bardziej lub mniej zaawansowaną analizą matematyczną.

Stąd nikt nie może powiedzieć, że to prościzna.
Opisanie tych trudniejszy części wykracza (nie)stety poza zakres niniejszego artykułu,
balansując przy tym na krawędzi kompetencji matematycznych autora.
