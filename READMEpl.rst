================
Witamy w M220J
================

W tym kursie będziemy badać, jak korzystać z MongoDB w środowisku Java.
Będziemy patrzeć na:

- Różne wersje i smaki sterownika Java MongoDB
- Jak wyrazić operacje CRUD w kodzie Java
- Korzystanie z ram agregacyjnych do wyrażania zapytań analitycznych
- Budowanie backendu aplikacji w Javie, który współdziała z MongoDB


Rozpocznij
-----------

Na tym kursie skonfigurujemy aplikację `` MFlix``, która będzie
umożliwia użytkownikom wyświetlanie listy filmów i interakcji z nimi (dowolne podobieństwo z
inne popularne usługi przesyłania strumieniowego wideo to czysty przypadek!).

W gotowej wersji aplikacji MFlix użytkownicy będą mogli:

- Wyszukuj filmy według następujących kryteriów:

  - członkowie obsady
  - rodzaje gatunków
  - dopasowania tekstowe w tytule lub opisie
  - kraj pochodzenia

- Twórz fasetowane filtry podczas wyszukiwania filmów
- Utwórz konto i zarządzaj ich preferencjami
- Zostaw komentarz na temat swoich ulubionych (lub najmniej ulubionych) filmów

... wśród innych funkcji typowych dla każdej usługi przesyłania strumieniowego filmów.

Twoja praca, jako nowy członek zespołu, który tworzy MFlix, i wkrótce
Ekspert programisty MongoDB Java, ma zaimplementować komponent backend w
konkretnie warstwę obiektu dostępu do bazy danych (DAO). Nie martw się, twój zespół ma
już zbudowałeś wszystkie testy integracyjne i jednostkowe, po prostu musisz je wszystkie
zdać!


Struktura projektu
~~~~~~~~~~~~~~~~~

MFlix składa się z dwóch głównych elementów:

- * Frontend *: Wszystkie funkcje interfejsu użytkownika są już zaimplementowane dla Ciebie
  zawiera wbudowaną aplikację React, o którą nie musisz się martwić.

- * Backend *: * Projekt Java Spring Boot *, który zapewnia niezbędną usługę
  Aplikacja. Kodem zarządza plik definicji projektu Maven, który
  będziesz musiał załadować do swojego Java IDE.

Większość tego, co zaimplementujesz, znajduje się w
Katalog `` src / main / java / mflix / api / daos``, który zawiera całą bazę danych
metody łączenia.

Testy jednostkowe w `` src / testy / java / mflix / api / daos`` przetestują tę bazę danych
dostęp do metod bezpośrednio, bez konieczności przechodzenia przez interfejs API.

Interfejs użytkownika będzie uruchamiał te metody w ramach testów integracyjnych i dlatego
są wymagane do uruchomienia pełnej aplikacji.

Warstwa API jest w pełni zaimplementowana, podobnie jak interfejs użytkownika. Domyślnie aplikacja
będzie działać na porcie 5000, ale jeśli potrzebujesz go uruchomić na porcie innym niż 5000, ty
może edytować plik `` index.html`` w katalogu `` build``, aby zmodyfikować wartość
** window.host **. Możesz znaleźć `` index.html`` w
Katalog `` src / main / resources / build``.

Do interfejsu API używamy * Spring Boot *. Maven pobierze tę bibliotekę dla Ciebie.
Więcej na ten temat poniżej.


Warstwa bazy danych
~~~~~~~~~~~~~~

Będziemy używać * MongoDB Atlas *, oficjalnej bazy danych MongoDB jako usługi (DBaaS),
więc nie będziesz musiał samodzielnie zarządzać składnikiem bazy danych. Jednak będziesz
nadal muszę zainstalować MongoDB lokalnie, aby uzyskać dostęp do narzędzi wiersza poleceń, które oddziałują
z Atlas, aby załadować dane do MongoDB i potencjalnie przeprowadzić eksplorację
twoja baza danych z powłoką.

Poniższy zestaw kroków pomoże Ci przygotować się do tego kursu.


Zależności środowiska lokalnego
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

W tym kursie występują dwie główne zależności systemowe:


1. Java 1.8

  * Wersja Java, na której zbudowano ten kurs, to Java 1.8. Możesz pobrać
    odpowiednią wersję dla twojego systemu operacyjnego, klikając
    `tutaj <http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>` _

2. Maven

  * Używamy Maven do zarządzania zależnościami w projekcie MFlix. Kliknij tutaj aby
    pobierz `Maven <https://maven.apache.org/install.html>` _


Instalacja Java Project (MFlix)
~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~

Projekt `` mflix`` jest obsługiwany przez plik POM `` Maven``, który zajmuje się wszystkimi
wymagane zależności, a także udostępnianie poleceń `` testuj '' i `` uruchom ''
kontrolować nasz projekt. Oznacza to, że możesz uruchomić wszystkie testy i wdrożyć
backend `` mflix`` z linii poleceń za pomocą `Maven`.

Zalecamy jednak korzystanie z Java IDE, aby śledzić lekcje i
aby zrealizować ** Bilety ** przypisane do ciebie na kursie.

Możesz użyć dowolnego IDE, które ci się podoba, ponieważ nie musisz mieć określonego
produkt do ukończenia kursu.
Byłoby lepiej, gdyby twoje IDE obsługiwało pliki `Maven POM`, więc można je ustawić
zależności poprawnie, w przeciwnym razie będziesz musiał pobrać i zainstalować
ręcznie różne biblioteki i sterowniki używane w projekcie.

To powiedziawszy, wszystkie wykłady i przykłady tego kursu zostały opracowane przy użyciu
Wersja IntelliJ IDEA CE. Znajdziesz lekcję poświęconą konfigurowaniu i
badanie tego IDE dla kursu.

Po pobraniu i rozpakowaniu pliku `` mflix-java.zip '' znajdziesz plik
folder z projektem. Folder projektu zawiera kod aplikacji, czyli
Plik `` pom.xml``, który chcesz zaimportować do swojego IDE i zestawu danych
wymagane będzie zaimportowanie do Atlas.

.. blok kodu :: sh

  $ ls
  mflix README.rst
  $ cd mflix
  $ ls
  src pom.xml data


Instalacja MongoDB
**********************

Zalecane jest połączenie * MFlix * z * MongoDB Atlas *, więc nie musisz tego robić
masz serwer MongoDB działający na twoim hoście. Wykłady i laboratoria w
ten kurs zakłada, że ​​używasz klastra * Atlas * zamiast lokalnego
instancja.

To powiedziawszy, nadal musisz mieć zainstalowany serwer MongoDB
aby móc korzystać z dwóch zależności narzędzia serwera:

- `` sklep mongorestore ''

  - Narzędzie do importowania danych binarnych do MongoDB.

- `` mongo ''

  - Powłoka do eksploracji danych w MongoDB.

Aby pobrać narzędzia wiersza polecenia, odwiedź stronę
`Centrum pobierania MongoDB <https://www.mongodb.com/download-center#enterprise>` _
i wybierz odpowiednią platformę.


Klaster MongoDB Atlas
~~~~~~~~~~~~~~~~~~~~~

* MFlix * używa * MongoDB *, aby zachować wszystkie swoje dane.

Jednym z najprostszych sposobów na rozpoczęcie pracy z MongoDB jest użycie * MongoDB Atlas *,
hostowane iw pełni zarządzane rozwiązanie bazy danych.

Jeśli uczestniczyłeś w innych kursach uniwersyteckich MongoDB, takich jak M001 lub M121, możesz
masz już konto - możesz ponownie użyć tego klastra do tego kursu.

Pamiętaj, aby użyć ** bezpłatnego klastra poziomów ** dla aplikacji i kursu.

* Uwaga: Należy pamiętać, że niektóre aspekty interfejsu użytkownika Atlas mogły ulec zmianie od tego czasu
redakcja tego pliku README, dlatego niektóre zrzuty ekranu w tym pliku mogą
różnić się od rzeczywistego interfejsu użytkownika Atlas. *


Korzystanie z istniejącego konta Atlas MongoDB:
****************************************

Jeśli masz już utworzone konto * Atlas *, być może dlatego, że je masz
wziął jeden z naszych innych kursów uniwersyteckich MongoDB, możesz go zmienić
M220J.

Zaloguj się do swojego konta * Atlas * i utwórz nowy projekt o nazwie ** M220 **, klikając
w menu rozwijanym * Kontekst *:

.. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_create_project.png

Po utworzeniu tego nowego projektu pomiń następną sekcję i przejdź do
* Tworzenie sekcji Mflix * wolnej warstwy klastra *.


Tworzenie nowego konta Atlas MongoDB:
*************************************

Jeśli nie masz konta * Atlas *, śmiało i `stwórz Atlas
Konto <https://cloud.mongodb.com/links/registerForAtlas> `_ wypełniając
Wymagane pola:

.. obraz :: https://s3.amazonaws.com/university-courses/m220/atlas_registration.png


Tworzenie klastra poziomów bezpłatnych M0 ** mflix **:
*******************************************

* Uwaga: Musisz wykonać ten krok, nawet jeśli ponownie używasz konta Atlas. *

1. Po utworzeniu nowego projektu pojawi się monit o utworzenie pierwszego
   klaster w tym projekcie:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_create.png


2. Wybierz AWS jako dostawcę chmury w regionie, który ma etykietę
   ** Dostępny bezpłatny poziom **:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_provider.png


3. Wybierz * Poziom klastra * ** M0 **:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_tier.png


4. Ustaw * Nazwa klastra * na ** mflix **, klikając nazwę domyślną
   * Klaster0 * i kliknij * Utwórz klaster *:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_name.png


5. Po naciśnięciu * Utwórz klaster * nastąpi przekierowanie na konto
   deska rozdzielcza. Na tym pulpicie upewnij się, że projekt ma nazwę ** M220 **.
   Jeśli nie, przejdź do punktu menu * Ustawienia * i zmień nazwę projektu
   z domyślnego * Projektu 0 * na ** M220 **:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_project.png


6. Następnie skonfiguruj ustawienia bezpieczeństwa tego klastra, włączając * IP
   Biała lista * i * Użytkownicy MongoDB *:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_ipwhitelisting.png

  Zaktualizuj białą listę adresów IP, aby aplikacja mogła komunikować się z klastrem. Kliknij
  Karta „Bezpieczeństwo” na stronie „Klastry”. Następnie kliknij „Biała lista IP”, a następnie
  „Dodaj adres IP”. Na koniec kliknij „Zezwól na dostęp z dowolnego miejsca” i kliknij
  "Potwierdzać".

  * Pamiętaj, że w środowisku produkcyjnym bardzo ściśle kontrolujesz listę
  Adresy IP, które można połączyć z klastrem. *

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_allowall.png


7. Następnie utwórz użytkownika bazy danych MongoDB wymaganego do tego kursu:

  - nazwa użytkownika: ** m220student **
  - hasło: ** m220 hasło **

  Możesz tworzyć nowych użytkowników poprzez * Bezpieczeństwo * -> * Użytkownicy MongoDB * -> * Dodaj nowego użytkownika *

  Zezwól temu użytkownikowi na uprawnienie do ** odczytu i zapisu w dowolnej bazie danych **:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_application_user.png


8. Po utworzeniu użytkownika i wdrożeniu klastra możesz to zrobić
   `` Załaduj przykładowy zestaw danych ''. Spowoduje to załadowanie przykładowego zestawu danych Atlas zawierającego
   baza danych MFlix do Twojego klastra:

   .. obraz :: https://s3.amazonaws.com/university-courses/m220/load_sample_dataset.png

    ** Uwaga: Baza danych MFlix w przykładowym zbiorze danych nosi nazwę „sample_mflix”. **


9. Teraz możesz przetestować konfigurację
   łącząc się za pomocą powłoki `` mongo ''. Możesz znaleźć instrukcje dotyczące połączenia
   w sekcji * Connect * pulpitu nawigacyjnego klastra:

  .. obraz :: https://s3.amazonaws.com/university-courses/m220/cluster_connect_application.png

  Przejdź do klastra * Przegląd * -> * Połącz * -> * Połącz swoją aplikację *.
  Wybierz opcję odpowiadającą MongoDB wersja 3.3 + i skopiuj
  Identyfikator URI połączenia `` mongo ''.

  Poniższy przykład łączy się z * Atlas * jako użytkownik, którego wcześniej utworzyłeś, przy pomocy
  nazwa użytkownika ** m220student ** i hasło ** m220password **. Możesz uruchomić to polecenie
  z linii poleceń:

  .. blok kodu :: sh

    mongo "mongodb + srv: // m220student: m220password @ <YOUR_CLUSTER_URI>"

  Łącząc się z serwerem z hosta, potwierdziłeś, że
  klaster jest skonfigurowany i dostępny z lokalnej stacji roboczej.


Importowanie danych (opcjonalnie)
~~~~~~~~~~~~~~~~~~~~~~~~~~

** Uwaga: jeśli użyłeś Ładuj przykładowy zestaw danych, możesz pominąć ten krok. **

Polecenie `` mongorestore '' niezbędne do zaimportowania danych znajduje się poniżej.
Skopiuj polecenie i użyj ciągu * Atlas SRV *, aby zaimportować dane (w tym
nazwa użytkownika i hasło).

Zamień poniższy ciąg SRV na własny:

.. blok kodu :: sh

  # przejdź do katalogu mflix-java
  cd mflix-java

  # import danych do Atlasu
  mongorestore --drop --gzip --uri mongodb + srv: // m220student: m220password @ <YOUR_CLUSTER_URI> dane


Uruchamianie aplikacji
~~~~~~~~~~~~~~~~~~~~~~~

W katalogu `` mflix / src / main / resources`` można znaleźć plik o nazwie
`` application.properties``.

Otwórz ten plik i wprowadź ciąg połączenia * Atlas SRV * zgodnie z instrukcjami w
komentarz. Jest to informacja, której sterownik użyje do połączenia. Upewnić się
** nie ** aby zawinąć * połączenie SRL * Atlas między cytatami:

  spring.mongodb.uri = mongodb + srv: // m220student: m220password @ <YOUR_CLUSTER_URI>

Aby uruchomić MFlix, uruchom następujące polecenie:

.. blok kodu :: sh

  cd mflix
  mvn spring-boot: run

A następnie skieruj przeglądarkę na `http: // localhost: 5000 / <http: // localhost: 5000 />` _.

Zalecane jest użycie IDE dla tego kursu. Upewnij się, że wybierasz IDE to
obsługuje importowanie projektu Maven. Polecamy IntelliJ Community_, ale Ty
może korzystać z wybranego przez Ciebie produktu.

Pierwsze uruchomienie aplikacji może potrwać nieco dłużej z powodu
proces wstępnej konfiguracji.

.. _Społeczność: https://www.jetbrains.com/idea/download


Przeprowadzanie testów jednostkowych
~~~~~~~~~~~~~~~~~~~~~~

Aby uruchomić testy jednostkowe dla tego kursu, użyjesz `` JUnit``. Każde laboratorium
zawiera moduł testów jednostkowych, które można wywoływać indywidualnie za pomocą polecenia
jak następujące:

.. blok kodu :: sh

  cd mflix
  mvn -Dtest = test <TestClass>

Na przykład, aby uruchomić test ConnectionTest, twoje polecenie powłoki będzie:

.. blok kodu :: sh

  cd mflix
  mvn -Dtest = Test połączenia

Alternatywnie, jeśli używasz IDE, powinieneś być w stanie uruchomić testy jednostkowe
indywidualnie, klikając zielony przycisk odtwarzania obok nich. Zobaczysz to
zademonstrowane w trakcie, ponieważ będziemy używać IntelliJ.

Każdy bilet będzie zawierał polecenie uruchomienia określonych testów jednostkowych tego biletu.
Podczas uruchamiania testów jednostkowych lub aplikacji z powłoki, upewnij się, że
jesteś w tym samym katalogu, co plik `` pom.xml``.