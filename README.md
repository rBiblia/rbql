rBible Query Language – Przewodnik
==================================

## [Wstęp](#wstęp-1)
### [Cel](#cel-1)
### [Definicje](#definicje)

## [I. Podstawowe operacje](#i-podstawowe-operacje-1)
### [1.1. Wielkość liter](#11-wielkość-liter-1)
### [1.2. Przedrostek](#12-przedrostek-1)
### [1.3. Przyrostek](#13-przyrostek-1)
### [1.4. Łączenie modyfikatorów i przełączników](#14-łączenie-modyfikatorów-i-przełączników-1)
### [1.5. Fraza](#15-fraza-1)
### [Podsumowanie rozdziału I](#podsumowanie-rozdziału-i-1)

## [II. Operatory relacji](#ii-operatory-relacji-1)
### [2.1. Operator `i`](#21-operator-i-1)
### [2.2. Operator `lub`](#22-operator-lub-1)
### [2.3. Operator `albo`](#23-operator-albo-1)
### [2.4. Nawiasy grupujące](#24-nawiasy-grupujące-1)
### [2.5. Operator `nie`](#25-operator-nie-1)
### [Podsumowanie rozdziału II](#podsumowanie-rozdziału-ii-1)

## [III. Łączenie modyfikatorów i przełączników wraz z operatorami](#iii-łączenie-modyfikatorów-i-przełączników-wraz-z-operatorami-1)

## [IV. Funkcje dodatkowe](#iv-funkcje-dodatkowe-1)
### [4.1. Znak zapytania](#41-znak-zapytania-1)
### [4.2. Kontekst](#42-kontekst-1)

## [Podsumowanie](#podsumowanie-1)

## [V. Dodatek](#v-dodatek-1)
### [5.1. Dozwolone znaki w wyrazach](#51-dozwolone-znaki-w-wyrazach-1)
### [5.2. Więcej przykładów](#52-więcej-przykładów-1)
### [5.3. Rzadziej używane znaki na klawiaturze](#53-rzadziej-używane-znaki-na-klawiaturze-1)

## [Autorzy](#autorzy-1)

Wstęp
=====

Tryb wyszukiwania zaawansowanego w rBiblii został zaprojektowany na podstawie funkcjonalności programu **Effata**, który stworzony został przez [Józefa Kajfosza](https://www.docelu.biblia.info.pl) w latach '90. Do celów wyszukiwania został zaprojektowany specjalny język o nazwie **rBQL** (*rBible Query Language* – czyli język zapytań rBiblii). Składa się on z szeregu symboli/znaków które mają specjalne znaczenie oraz z liter wielu alfabetów, które następnie mogą posłużyć do budowania wzorców (wszystkie dozwolone znaki są zawarte na końcu tego zestawienia). Podręcznik ten został podzielony na kilka rozdziałów:

- *Rozdział I* omawia podstawową jednostkę wyszukiwania – atom (pojedyncze słowo) i jego możliwości rozbudowy.
- *Rozdział II* omawia sposoby budowania relacji pomiędzy atomami za pomocą dozwolonych operatorów: `i`, `lub`, `albo`, `nie` oraz nawiasów.
- *Rozdział III* przedstawia sposoby łączenia możliwości wyszukiwania z rozdziału I i II w bardziej skomplikowane wzorce.
- *Rozdział IV* pokazuje funkcje dodatkowe, pomocne podczas przeszukiwania.

Na końcu umieszczony został dodatek, który zawiera dozwolone znaki, przykłady poprawnych i niepoprawnych wzorców oraz umiejscowienie znaków specjalnych na klawiaturze.

Cel
---

Celem podręcznika jest wprowadzenie Użytkownika w tematykę wzorców, a dokładniej nauczenie go odczytywania oraz samodzielnego tworzenia zapytań. Poniższy schemat ukazuje budowę wzorca, który będzie omówiony szczegółowo w kolejnych rozdziałach.

![Przykładowy wzorzec](/images/pattern.jpg)

<a name="definicje"></a>Na początek objaśnienie definicji:

- `Słowo` – to pojedynczy ciąg znaków.
- `Fraza` – to słowa oddzielone spacjami, które tworzą spójny ciąg do odnalezienia.
- `Wzorzec` – to zbiór słów/fraz połączonych w relację za pomocą operatorów, opcjonalnie zakończony operatorem kontekstu.
- `Atom` – to podstawowa jednostka wyszukiwania, składa się ze słowa lub frazy oraz dodatkowo może składać się z:

    * przełącznika wielkości liter (np. `słowo/i`)
    * modyfikatora przedrostka (np. `*słowo`)
    * modyfikatora przyrostka (np. `słowo*`)
    * pytajnika, zastępującego jeden dowolny znak (np. `s?owo`)

Poszczególne elementy, z których zbudowane są wzorce będą kolejno omawiane w dalszej części.

I. Podstawowe operacje
======================

1.1. Wielkość liter
-------------------

Wyszukiwarka, bez wskazanego rozróżnienia, będzie poszukiwać słów stosując zadaną wielkość liter. Przykładowo przeszukując **Biblię warszawską** za pomocą słowa `bóg` otrzymamy 9 wyników.

Opisane zachowanie możemy zmienić za pomocą przełącznika `/i`.

```
bóg/i
```

***Pamiętaj: Przełączniki muszą wystąpić tuż za słowem/frazą, oznacza to, że pomiędzy wyrazem, a przełącznikiem nie może wystąpić spacja.***

Przełącznik `/i` jest ustawieniem przypisanym do słowa/frazy, oznacza to, że we wzorcu każde słowo/fraza posiada swój zestaw ustawień. A więc niektóre słowa/frazy będą wyszukiwane tak jak zostały wpisane natomiast inne – za pomocą przełącznika – bez względu na wielkość podanych liter.

*i – pochodzi z ang. [i]nsensitive – czyli niewrażliwy (na wielkość liter)*

**Poprawny wzorzec:**

```
bóg/i
```

Wyszukaj wersety w których występuje dokładnie słowo `bóg` [trzy literowe słowo `bóg`] nie zwracając uwagi na wielkość liter.

Otrzymane wyniki uwzględnią wersety z następującymi wyrazami:

- bóg
- Bóg
- bÓg
- bóG
- BÓg
- bÓG
- BÓG

Dzięki czemu dla **Biblii warszawskiej** otrzymamy już 1198 wyników.

**Niepoprawny wzorzec:**

```
bóg /i
```

Taki wzorzec jest niepoprawny i zostanie odrzucony przez program, ponieważ przełącznik nie znajduje się zaraz za wyrazem/frazą, ale istnieje między nimi spacja.

*Obecnie w rBiblii występuje tylko jeden przełącznik, ale w przyszłości może pojawić się ich więcej.*

1.2. Przedrostek
----------------

*Przedrostek czyli z łacińskiego `przymocowany na przedzie`.*

Czasami chcemy znaleźć wersety które zawierają ten sam wyraz, ale w odmienionej formie. Np. chcemy znaleźć słowo `wieczny`, ale także `przed-wieczny`, `od-wieczny`.

W tym celu możemy skorzystać z modyfikatora przedrostków – jego symbolem jest gwiazdka umieszczona tuż przed ciągiem.

**Poprawny wzorzec:**

```
*wieczny
```

Gwiazdka w tym miejscu oznacza: wyszukaj wersety w których znajdziesz słowa kończące się na `wieczny`, ale pozwól, żeby to słowo rozpoczynało się dowolnymi innymi znakami.

**Niepoprawny wzorzec:**

```
* wieczny
```

Jest on niepoprawny, ponieważ między modyfikatorem, a wyrazem występuje spacja.

* samo słowo `wieczny` - daje 30 wyników w **Biblii warszawskiej**
* słowo `*wieczny` - daje 31 wyników (1 werset zawiera słowo `odwieczny`)

***Pamiętaj: modyfikator przedrostka uwzględnia sam trzon jako trafiony werset, tzn, że w przypadku wzorca `*wieczny`, werset który zawiera dokładnie słowo `wieczny` będzie uwzględniony w wynikach wyszukiwania oraz wersety, które zawierają słowa kończące się na `wieczny`, czyli `odwieczny`, `przedwieczny` itp.***

1.3. Przyrostek
---------------

*Przyrostek czyli `dołączony` z j. łacińskiego.*

Czasami potrzebne jest wyszukanie wersetów zawierające szukane słowa, biorąc pod uwagę jego odmiany/końcówki.

Załóżmy, że chcemy wyszukać słowa: `Bogu`, `Boga`, `Bogiem`

Aby utworzyć działający wzorzec, należy przyjrzeć się odmianom i spróbować znaleźć wspólny trzon – w tym przypadku trzonem będzie `Bog`, a następnie dodać modyfikator przyrostków, czyli gwiazdkę umieszczoną zaraz po wyrazie.

**Poprawny wzorzec:**

```
Bog*
```

Taki wzorzec wyszuka wersety w których występuje słowo zaczynające się od `Bog` oraz kończące się na dowolnym zestawie znaków, np:

- Bog
- Bogowie
- Bogi
- Bogów
- Bogom
- Bogów
- Bogami
- Bogach itp.

Wszystkie wersety spełniające to kryterium zostaną zaliczone do wyników wyszukiwania.

**Niepoprawny wzorzec:**

```
Bog *
```

Taki wzorzec jest niepoprawny, ponieważ pomiędzy gwiazdką, a słowem występuje spacja.

***Pamiętaj: modyfikator przyrostka uwzględnia także sam trzon jako trafiony werset, tzn, że w przypadku wzorca `Bog*`, werset który zawierałby samo słowo `Bog` będzie uwzględniony w wynikach wyszukiwania.***

1.4. Łączenie modyfikatorów i przełączników
-------------------------------------------

Trzy poprzednie opisy pokazywały dodatkowe opcje, za pomocą których możemy rozszerzyć możliwości wyszukiwania słowa/fraz, tj. **przedrostek**, **przyrostek** oraz wyszukiwanie z pominięciem **wielkości liter**.

Teraz dowiesz się, że elementy te można ze sobą łączyć.

```
*wieczn*
```

Taki wzorzec będzie poszukiwał wersetów w których znajdzie:

- odwieczny
- odwieczna
- odwiecznemu
- przedwieczny
- przedwieczna
- przedwiecznemu
- wieczna
- wieczny
- wiecznemu itd.

Innymi słowy – będzie poszukiwał wersetów w których znajduje się wyraz składający się z ciągu `wieczn`, czyli ciąg `wieczn` musi znaleźć się gdzieś w wersecie, aby został zaliczony do wyników wyszukiwania.

```
*zmienn*/i
```

Taki wzorzec będzie poszukiwał wersetów w których znajdzie:

- Zmienny
- Odmienny
- odmienny
- przemienny
- przemienna
- Przemienny
- Przemienna
- zmienny
- Zmienna itd.

Innymi słowy – będzie poszukiwał wersetów w których znajdzie wyraz składający się z ciągu `zmienn` nie zważając na wielkość liter. Ciąg `zmienn` musi znaleźć się gdzieś w wersecie, aby został zaliczony do wyników wyszukiwania, nieważne czy dużą czy małą literą.

1.5. Fraza
----------

Frazy są to dwa (lub więcej) słowa oddzielone spacją. Program będzie poszukiwał dokładnie takiego ciągu w Biblii.

```
Jan Chrzciciel
```

Taki wzorzec będzie poszukiwał wersetów w których słowo `Chrzciciel` występuje zaraz po słowie `Jan` z dokładnie jedną spacją pomiędzy nimi.

**W Biblii warszawskiej** daje to 6 wyników.

Poszczególne słowa można uzupełniać o przedrostki i przyrostki:

```
Jan* Chrzciciel*
```

Odnajdzie wersety w kótrych występuje:

- Jan Chrzciciel
- Janowi Chrzcicielowi
- Janem Chrzcicielem itp.

***Pamiętaj: przełącznik wielkości liter (`/i`) ma zastosowanie do całej frazy. Oznacza to, że w przypadku frazy `Jan Chrzciciel`, nie można zastosować `Jan/i Chrzciciel`, aby słowo `Jan` było wyszukiwane niezależnie od wielkości liter, natomiast słowo `Chrzciciel` tak jak zostało podane.***

Modyfikator wielkości liter może wystąpić tylko na końcu frazy:

```
Jan Chrzciciel/i
```

Wyszuka frazę `Jan Chrzciciel` nie zważając czy będzie składać się z wielkich czy małych liter, czyli np. `Jan chrzciciel` także zostanie zaliczony.

Podsumowanie rozdziału I
------------------------

W pierwszym rozdziale poznaliśmy podstawową jednostkę wyszukiwania, jaką jest *słowo*. Następnie nauczyliśmy się, że *słowo* można uzupełniać o dodatkowe opcje, które mają wpływ na wyniki wyszukiwania. Należą do nich:

- wyszukiwanie nie zwracające uwagi na wielkość liter (`słowo/i`)
- przedrostki (`*słowo`)
- przyrostki (`słowo*`)
- frazy (`słowo1 słowo2`)
- kombinacje powyższych (`słowo* *słowo/i`)

Umiejętne stosowanie wyżej wymienionych kombinacji przyczyni się do otrzymywania trafniejszych wyników.

II. Operatory relacji
=====================

W tej części przyjrzymy się w jaki sposób można łączyć ze sobą słowa. Poznamy kilka operatorów, które służą do budowania relacji między atomami. Ich umiejętne użycie, może w rezultacie dać ciekawe studium wybranego tematu. Każdy z operatorów posiada specjalny symbol, który jest wprowadzany za pomocą klawiatury. Umiejscowienie znaków specjalnych na klawiaturze zostało przedstawione w dodatku na końcu tego przewodnika.

2.1. Operator `i`
-----------------

**Symbol:**

```
&
```

**Opis działania:**

Werset zostanie uwzględniony w wynikach wyszukiwania, jeżeli oba słowa/frazy znajdujące się po lewej i prawej operatora `&` występują w wersecie.

**Miejsce na klawiaturze:**

```
Shift + 7
```

**Przykład zastosowania:**

```
Pan&Bóg
```

*(czyt. Pan i Bóg)* – wyszuka wersety, w których występuje zarówno słowo `Pan` jak również `Bóg`.

W **Biblii warszawskiej** daje to 466 wyników.

2.2. Operator `lub`
-------------------

**Symbol:**

```
|
```

**Opis działania:**

Werset zostanie uwzględniony w wynikach wyszukiwania, jeżeli którykolwiek lub obydwa ze słów/fraz znajdujący się po lewej bądź prawej operatora `|` występują w wersecie.

**Miejsce na klawiaturze:**

```
Shift + \
```

**Przykład zastosowania:**

```
Pan|Bóg
```

*(czyt. Pan lub Bóg)* – wyszuka wersety, w których występuje słowo `Pan` lub `Bóg` lub `Pan` i `Bóg` naraz.

W **Biblii warszawskiej** daje to 3959 wyników.

2.3. Operator `albo`
--------------------

**Symbol:**

```
^
```

**Opis działania:**

Werset zostanie uwzględniony w wynikach wyszukiwania, jeżeli jedno albo drugie słowo/fraza znajdujące się po lewej bądź prawej operatora `^` występują w wersecie.

**Miejsce na klawiaturze:**

```
Shift + 6
```

**Przykład zastosowania:**

```
Pan^Bóg
```

*(czyt. Pan albo Bóg)* – wyszuka wersety, w których występuje samo słowo `Pan` albo samo słowo `Bóg`, ale odrzuci te wersety w których słowa `Pan` i `Bóg` występują jednocześnie.

W **Biblii warszawskiej** daje to 3493 wyników.

2.4. Nawiasy grupujące
----------------------

**Symbol:**

```
( oraz )
```

**Opis działania:**

Za ich pomocą można łączyć słowa/frazy w jedną grupę, aby budować bardziej skomplikowane relacje.

**Miejsce na klawiaturze:**

```
Shift + 9
Shift + 0
```

**Przykład zastosowania:**

```
(Bóg^Jezus)&Duch
```

*(czyt. (Bóg albo Jezus) i Duch)* – wyszukaj wersety w których `Bóg` jest razem z `Duch` albo `Jezus` jest razem z `Duch` przy `Bóg`, `Jezus` i `Duch` nie może wystąpić jednocześnie.

Taka konfiguracja daje 6 znalezionych wyników w **Biblii warszawskiej**.

**Niepoprawne wzorce nawiasów:**

```
((Bóg^Jezus)&Duch
```

We wzorcu znajdują się dwa nawiasy otwierające, natomiast tylko jeden nawias zamykający.

```
(Bóg^Jezus))&Duch
```

We wzorcu znajdują się dwa nawiasy zamykające, natomiast tylko jeden nawias otwierający.

2.5. Operator `nie`
-------------------

**Symbol:**

```
~
```

**Opis działania:**

Werset nie zostanie zaliczony do wyników wyszukiwania, jeśli znajduje się w nim zanegowane słowo/fraza.

**Miejsce na klawiaturze:**

```
Shift + `
```

Operator negacji występuje zawsze przed słowem/frazą lub nawiasem otwierającym. Dzięki zastosowaniu tego operatora można wyłączyć ze zbioru wyników wyszukiwań wersety zawierające podane słowo/frazę lub zestaw słów/fraz.

**Przykłady zastosowania:**

```
~Pan&Bóg
```

*(czyt. nie Pan i Bóg)* – wyszuka wszystkie wersety, które zawierają słowo `Bóg` oraz jednocześnie nie zawierają słowa `Pan`.

727 wyników w **Biblii warszawskiej**.

```
Pan&~Bóg
```

*(czyt. Pan i nie Bóg)* – znajdzie wersety w których znajduje się `Pan`, ale nie `Bóg`.

2766 wyników dla **Biblii warszawskiej**.

```
~(Pan&Bóg)
```

*(czyt. nie Pan i Bóg)* – znajdzie wszystkie wersety w których wyrazy `Pan` i `Bóg` nie występują jednocześnie.

30697 wyników dla **Biblii warszawskiej**.

**Przykłady niepoprawnych wzorców:**

```
Pan~&Bóg
```

*(Pan nie i Bóg)* – ponieważ operator negacji, nie może występować, przed innym operatorem.

```
Pan&Bóg~
```

*(Pan i Bóg nie)* – ponieważ operator negacji, nie może występować jako koniec słowa.

***Pamiętaj: choć nie ma to większego sensu, dozwolone jest stosowanie większej ilości negacji. Parzysta ilość negacji da wynik, jakby jej nie było (negacja negacji daje powrót do początku), natomiast nieparzysta liczba negacji, da ten sam rezultat co pojedyńcza negacja.***

***Modyfikator przedrostka, przyrostka nie wyklucza trzonu w wynikach wyszukiwań – tzn. dla wzorca `*wieczny` – trzon to wieczny, więc wersety które zawierają dokładne słowo `wieczny` są uwzględnione w wynikach wyszukiwania. Jeśli chcielibyśmy otrzymać wersety zawierające tylko i wyłącznie słowo z jakimkolwiek przedrostkiem, wykluczając wersety, które zawierają sam trzon (np. `wieczny`), możemy użyć wzorca operatora negacji:***

```
*wieczny&~wieczny
```

***Wyszukaj wersety w których słowo `wieczny` poprzedzone jest jakimś przedrostkiem, ale wyklucz takie wersety, w których występuje tylko słowo `wieczny`.***

**Dzięki temu uwzględnimy wyrazy:**

- przedwieczny
- odwieczny itp.

**A wykluczymy wyraz:**

- wieczny (jako całe jedno słowo)

Podsumowanie rozdziału II
-------------------------

Ten rozdział przedstawiał metody łączenia ze sobą słów/fraz za pomocą 5 operatorów: `i`, `lub`, `albo`, nawiasów i `negacji`. Pokazane zostały ich przykładowe zastosowania. Zrozumienie zagadnień z tego rozdziału jest kluczowe do tworzenia bardziej skomplikowanych wzorców. Aby w pełni zrozumieć i móc przewidzieć wynik utworzonego wzorca, przydatna jest wiedza z zakresu działania operatorów logicznych `NOT`, `AND`, `OR` oraz `XOR`.

III. Łączenie modyfikatorów i przełączników wraz z operatorami
==============================================================

Ten rozdział jest dużo bardziej zaawansowany. Wymaga zrozumienia dwóch poprzednich rozdziałów. Od teraz można przejść do bardziej praktycznych zastosowań. Skupimy się tutaj na kilku przykładach wraz z ich opisem.

**Przykład 1**

```
syn* człow*/i&przyjd*/i
```

Ten wzorzec będzie poszukiwał wersetów, które zawierają frazę `syn człow` z różnymi końcówkami, np. `syna człowieczego` itp., nie zważając na wielkość liter plus werset będzie posiadał słowo zaczynające się na `przyjd` i kończące się na dowolnym ciągu znaków, pomijając wielkość liter. Dzięki temu powinniśmy otrzymać wersety, w których mowa jest o przyjściu `Syna Człowieczego`.

W **Biblii warszawskiej** daje to 13 wersetów, lecz nie wszystkie dotyczą tego tematu.

**Przykład 2**

```
Duch* Jezus*|Duch* Chryst*|Duch* Syn*/i
```

Ten wzorzec poszukuje w tekstach biblijnych trzech fraz wraz z ich odmianiami: `Duch Jezusa`, `Duch Chrystusa` lub `Duch Syna`. Dzięki temu możemy otrzymać wersety w których `Duch Boży`, `Duch Święty` nazwany jest `Duchem Jezusa`.

Otrzymamy 6 takich wersetów.

**Przykład 3**

```
(dzień/i|dniu/i)&(pań*/i|chryst*/i)
```

Za pomocą tego wzorca możemy starać się odnaleźć wersety które mówią o `przyjściu pańskim/Chrystusa`. Pierwszy nawias określa, że werset musi zawierać słowo `dzień` lub słowo `dniu` z pominięciem wielkości znaków. Drugi nawias określa, że werset musi posiadać wyraz rozpoczynający się na `pań` i kończący dowolnym ciągiem (np. `pański` itp.) lub słowo rozpoczynające się na `chryst`, a kończące się dowolnym ciągiem z pominięciem wielkości znaków. Relacja pomiędzy nawiasami to `i`, więc minimalnie w wersecie musi wystąpić po jednym wyrazie z obu nawiasów.

Odnajduje to 17 wersetów, lecz nie wszystkie pasują do tematu.

**Przykład 4**

```
(abyś poznał|poznacie|poznają|aby poznali|poznasz/i|abyś wiedział|poznały|abyście poznali)
&~(zwierzęta poznają|poznają go|wtedy wszyscy|poznały mężczyzny|poznały współżycia|Piłat|nader obfitą)
```

Ten wzorzec może wydawać się skomplikowany. W rzeczywistości pierwszy nawias określa pewien zbiór słów które mają się pojawić w wersecie, drugi nawias wyklucza z tego zbioru, pewne frazy/słowa które nie pasują do tematu, a występowały w wynikach wyszukiwania. Wzorzec ten służy do odnalezienia fragmentów w których Bóg poprzez swoje działanie chce nas czegoś nauczyć, chce abyśmy wyciągneli jakiś wniosek z jakiejś sytuacji, abyśmy coś o Nim poznali lub coś zrozumieli.

**Biblia warszawska** daje tutaj 140 wyników, ale nie wszystkie są trafne.

IV. Funkcje dodatkowe
=====================

4.1. Znak zapytania
-------------------

Jest to opcja, która wzbogaca atomy. Umieszczenie w którymś miejscu słowa/frazy pytajnika da możliwość zastosowania tzw. `dzikiej karty`. W tym miejscu słowa może pojawić się dowolny jeden znak.

**Przykład zastosowania:**

```
Je?us
```

Taki wzorzec będzie poszukiwał wersetów w których znajduje się pięcioliterowe słowo rozpoczynające się od `Je`, kończące na `us`, a pomiędzy nimi znajdzie się jeden dowolny znak.

Wynikiem może być np. `Jezus`, `Jesus` itp.

***Ciekawostka: stosując same znaki zapytania np. `????` otrzymamy wersety w których występują 4 literowe słowa. W ten sposób możemy spróbować odnaleźć najdłuższe słowo w wybranym tłumaczeniu.***

4.2. Kontekst
-------------

Kontekst znajduje się na końcu wzorca. Reprezentowany jest za pomocą znaku małpy oraz następującej po nim liczby z przedziału 1-9.

```
@2
```

Podając liczbę w kontekscie (1-9) można ustalić w jaki sposób program będzie przeszukiwał Biblię. Domyślnie program sprawdza werset po wersecie sprawdzając, czy spełniony jest wpisany przez użytkownika wzorzec.

W przypadku kontekstu `@2`, program złączy werset 1 z 2 i na takim połączeniu dwóch wersetów, sprawdzi, czy wzorzec jest spełniony. Następnie złączy werset 2 i 3 oraz sprawdzi czy wzorzec występuje w tym układzie, następnie sklei werset 3 i 4 itd.

W przypadku kontekstu `@3` przeszukiwane są sklejone wersety 1, 2 i 3, następnie 2, 3 i 4 itd.

Jeśli wzorzec zostanie spełniony w kontekscie 1-3, te trzy wersety zostają dodane do wyników wyszukiwania, a kolejne przeszukiwanie rozpoczyna się od pierwszego kontekstu występującego po ostatnim wyniku (aby nie powielać wyników wyszukiwania), czyli w tym wypadku 4-6.

Dzięki kontekstowi możemy poszerzyć obszar poszukiwań konkretnego wzorca w interesującym nas przekładzie, aby być może otrzymać ciekawsze wyniki.

Dla przykładu, jeśli szukany przez nas wzorzec, nie trafi się przeszukując pojedyńcze wersety, może okazać się, że występuje on w szerszym kontekscie i wtedy można użyć tej opcji.

**Przykład zastosowania:**

```
Boga&powstało
```

Daje werset w którym występuje `Boga` i `powstało` jednocześnie. W **Biblii warszawskiej** daje to 1 werset. Poszerzając kontekst do dwóch wersetów, otrzymamy 4 wersety:

```
Boga&powstało@2
```

Podsumowanie
============

To już koniec. Mamy nadzieję, że tematyka zaawansowanego wyszukiwania została przedstawiona w miarę prostym językiem. Instruktaż ten z pewnością będzie jeszcze aktualizowany w przyszłości, a uwagi są mile widziane.

Próg wejścia dla osób nietechnicznych nie jest wcale taki niski. Trzeba opanować parę terminów i koncepcji, ale kiedy to będzie za nami, zaawansowany tryb wyszukiwania może przynieść wiele korzyści, a tworzenie nowych wzorców będzie czymś naturalnym.

Stopień skomplikowania wzorców nie został dotychczas ograniczona programowo, można dowolnie je łączyć jeżeli tylko sprzęt, na którym działa aplikacja jest w stanie stawić czoła poszukiwaniom.

Na zakończenie prosimy, aby nie zatrzymywać *perełek* dla siebie. Zachęcamy do [podsyłanie ciekawszych wzorców](https://kontakt.toborek.info), abyśmy mogli utworzyć swego rodzaju bazę, z której korzystać będą mogli wszyscy.

Owocnego przeszukiwania Biblii!

V. Dodatek
==========

5.1. Dozwolone znaki w wyrazach
-------------------------------

- litery angielskiego alfabetu (duże i małe) a-z
- polskie litery diakrytyczne (`ąęśćż` itp.)
- znak apostrofu ' – potrzebny dla angielskich tłumaczeń np. `Christ’s`
- znaki cyrylicy
- greckie litery alfabetu
- rumuńskie litery alfabetu
- `?`
- spacja
- `*`
- `/i`

5.2. Więcej przykładów
----------------------

**Poprawne wzorce:**

```
Bóg|Ojciec
```

Wyszukaj wersety w których występuje słowo `Bóg` lub `Ojciec` lub oba naraz.

```
Bóg&(Ojciec|Król/i)
```

Wyszukaj wersety w których występuje słowo `Bóg` wraz ze słowem `Ojciec`

lub gdzie występuje słowo `Bóg` wraz ze słowem `Król` przy czym słowo `Król` wyszukuj nie zważając na wielkość liter

lub gdzie występuje słowo `Bóg` wraz ze słowem `Ojciec` wraz ze słowem `Król` przy czym słowo `Król` wyszukuj nie zważając na wielkość liter.

```
syn* człow*/i
```

Wyszukaj wersety w których występuje np. fraza `syn człowieczy` w różnej formie tzn. `syn człowieczy`, `synowi człowieczemu`, `synem człowieczym` itd.

**Niepoprawne wzorce:**

```
Chrystus */i
```

Wzorzec ten jest niepoprawny, ponieważ gwiazdka przyrostka nie przylega do wyrazu, lecz znajduje się między nimi spacja, poprawna forma to `Chrystus*/i`.

```
* wieczny
```

Wzorzec jest niepoprawny, ponieważ gwiazdka przedrostka nie przylega do wyrazu, pomiędzy nimi znajduje się spacja, poprawna forma to `*wieczny`.

```
(dni*|dzień)&&ostatecz*
```

Powodem błędu jest wystąpienie dwukrotnie znaku `&` (`i`) – operatory relacji (`&`, `|`, `^`) mogą występować tylko raz między wyrazami.

```
przymierze&(Pan|Bóg))
```

Liczba nawiasów otwierających i zamykających nie jest równa, brakuje tutaj nawiasu otwierającego.

5.3. Rzadziej używane znaki na klawiaturze
------------------------------------------

- `~` – *(tylda)* znak negacji

![negacja](/images/char_negation.png)

- `&` – i

![i](/images/char_and.png)

- `|` – lub

![lub](/images/char_or.png)

- `^` – albo

![albo](/images/char_xor.png)

Autorzy
=======

- [Kacper Kość](https://github.com/jaspre) (implementacja rBQL, autor przewodnika)
- [Rafał Toborek](https://github.com/clash82) (twórca rBiblii, korekty w przewodniku)
- [Józef Kajfosz](https://www.docelu.biblia.info.pl) (pomysłodwaca i twórca programu *Effata*)

Strona domowa programu: [rbiblia.toborek.info](https://rbiblia.toborek.info)

Przewodnik dostępny jest bezpłatnie w [trybie online](https://github.com/rBiblia/rbql-docs).
Wszelkie poprawki i propozycje w tym przewodniku będą bardzo mile widziane – prosimy utworzyć `Pull Request` lub utworzyć zgłoszenie poprzez `Issue`.

Praca nad programem, tłumaczeniami oraz dokumentacją pochłania środki materialne. Jeżeli możesz, prosimy o [dobrowolne wsparcie projektu](https://rbiblia.toborek.info/donation/).

> Całe Pismo przez Boga jest natchnione i pożyteczne do nauki, do wykrywania błędów, do poprawy, do wychowywania w sprawiedliwości, aby człowiek Boży był doskonały, do wszelkiego dobrego dzieła przygotowany. [2Tym 3:16]

&copy; 2020 Soli Deo gloria
