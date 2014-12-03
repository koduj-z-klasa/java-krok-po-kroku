Programowanie obiektowe
========================

**Czego się dowiesz**

* Czym jest programowanie obiektowe
* Jaka jest różnica między klasą a obiektem
* W jaki sposób definiować własne typy danych
* Czym są metody i konstruktory
* Czym jest przeciążanie metod i konstruktorów
* Do czego służy słowo kluczowe 
* Czym są pakiety i jakie są możliwe specyfikatory dostępu
* Jak porównywać obiekty


**Czym jest programowanie obiektowe?**

Czysto teoretycznie programowaniem obiektowym najłatwiej nazwać próbę odwzorowania bytów ze świata rzeczywistego w naszej aplikacji poprzez utworzenie nowych, bardziej złożonych typów danych.

Dużo prościej jest to jednak zrozumieć na konkretnych przykładach, do których za chwilę przejdziemy.


Klasy i obiekty
----------------

**Ćwiczenie**
Wyobraź sobie, że tworzysz prostą aplikację do obsługi sklepu komputerowego. Najważniejsze będzie w niej zdecydowanie przechowywanie informacji o produktach. Wypisz kilka cech produktu, które Twoim zdaniem będą istotne z punktu widzenia użytkownika tej aplikacji(sprzedawcy).

Przykłady cech przydatnych
::
  Nazwa produktu
  Cena
  Producent
  Model / nazwa kodowa
  Rok produkcji
  
Możliwe, że rzeczy, które wypisałeś są inne od pokazanych powyżej - to bardzo dobrze! Świadczyć to może o różnej wizji na aplikację. Na tę chwilę ograniczymy się do dwóch podstawowych informacji, czyli nazwy produktu oraz jego ceny. Te dwie podstawowe rzeczy na dobrą sprawę pozwolą nam znaleźć dowolny towar w bazie wszystkich produktów oraz dokonać sprzedaży na podstawie podanej ceny.

**Ćwiczenie** *(5 min)*

    Napisz prosty program, w którym w zmiennych przechowasz informacje o 2 różnych produktach (np. Monitor Samsung Syncmaster za 700zł oraz Laptop HP Probook 450 za 3000zł). Wydrukuj następnie informacje o tych produktach na ekranie. Wykorzystaj różne typy danych do przechowywania nazw produktów a inne do przechowywania ceny.

Przypomnijmy, że najpierw należy utworzyć nowy projekt poprzez menu File -> New -> Java Project, a następnie w folderze *src* dodać nową klasę np. korzystając z przycisku ze znakiem C na górnej belce nawigacyjnej. Nazwa klasy powinna być identyczna z nazwą pliku (eclipse zadba o to automatycznie) - w naszym przypadku będzie to plik Shop.java.

*plik Shop.java*

.. code-block:: java
    :linenos:

    public class Shop {
        public static void main(String[] args) {
            String product1Name = "Samsung Syncmaster";
            double product1Price = 700.0;

            String product2Name = "HP Probook 450";
            double product2Price = 3000.0;

            System.out.println("Produkty w sklepie: ");
            System.out.println(product1Name + ":" + product1Price);
            System.out.println(product2Name + ": " + product2Price);
        }
    }
    
Wszystko wygląda na pierwszy rzut oka w porządku, ale zastanów się teraz w jaki sposób zapisałbyś informację o kilkuset produktach w tym sklepie? Możliwe, że przychodzi Ci do głowy utworzenie tablic z imionami, nazwiskami itd. Nie jest to najgorszy pomysł, jednak ma jedną dużą wadę - dane nie są spójne i w żaden sposób ze sobą powiązane. Zmiana czegokolwiek, np dodanie 1 produktu, wiązałoby się z koniecznością aktualizacji innej tablicy z cenami, należałoby uważać na odwoływanie się do odpowiednich indeksów tablic itd.

Tutaj dochodzimy do sedna programowania obiektowego. Czy nie byłoby świetną sprawą możliwość utworzenia swojego typu danych "Product", który moglibyśmy wykorzystywać tak jak wartości typu int, czy double? W Javie możemy zdefiniować swój typ, zwyczajnie tworząc nową klasę, a w niej definiując zmienne typów prostych (lub wcześniej utworzonych typów obiektowych).

.. note::
    Klasą nazywamy zbiór cech oraz funkcjonalności obiektu, który chcemy odwzorować w pamięci komputera.

*plik Product.java*

.. code-block:: java
    :linenos:

    public class Product {
        String name;
        double price;
    }

Struktura projektu eclipse:

.. image:: 02_obiekty/pcshop_eclipse_1.png
    :align: center

Zauważ, że w klasie tej nie tworzymy po raz kolejny metody **main** - jest ona potrzebna tylko w jednej klasie w ramach całego projektu i to od niej rozpocznie się działanie programu. Pozostałe klasy będą miały natomiast za zadanie definicję nowego typu danych lub wydzielenie pewnych funkcjonalności (np. obliczeń, przetwarzania tekstu, pobierania danych z internetu itd.).
Na podstawie dowolnej klasy możemy utworzyć **obiekt**. Obiekty zawsze będziemy tworzyli poprzez zapis **new NazwaKlasy();** przekazując w nawiasie ewentualne parametry (o tym za chwilę).

.. note::
    Obiektem nazywamy konkretny egzemplarz danej klasy. Klasą nazwiemy "Produkt", ale obiektem " produkt laptop HP Probook 450 kosztujący 3000zł".

*plik Shop.java*

.. code-block:: java
    :linenos:

    public class Shop {
        public static void main(String[] args) {
            Product product1 = new Product();
            product1.name = "Samsung Syncmaster";
            product1.price = 700.0;
            
            Product product2 = new Product();
            product2.name = "HP Probook 450";
            product2.price = 3000.0;
            
            System.out.println("Produkty w sklepie: ");
            System.out.println(product1.name + ":" + product1.price);
            System.out.println(product2.name + ": " + product2.price);
        }
    }

Jak widzisz zmienne *name* i *price* z wcześniejszego kodu są teraz opakowane w **obiekty** typu Product. Do poszczególnych **pól klasy** odwołujemy się za pomocą operatora kropki, np. *"product1.name"*.

.. note::
    Jeżeli nie zainicjujesz poszczególnych pól obiektu, przyjmą one wartości domyślne. Dla typów liczbowych jest to 0 lub 0.0, dla typu char specjalna wartość pusta, a dla typów obiektowych (w tym String) jest to wartość null.

.. attention::
    Często powtarzającym się błędem w Javie jest **NullPointerException**. Oznacza on, że obiekt, do którego próbujesz się odwołać nie został utworzony, a jedynie zadeklarowany. Jeżeli zobaczysz go w eclipse sprawdż więc, czy przypisałeś do odpowiedniej zmiennej (referencji) obiekt utworzony za pomocą słowa new.

Na chwilę obecną może Ci się wydawać, że zrobiło się tylko więcej kodu, a program nadal robi to samo. Zwróć jednak uwagę, że dane są teraz bardziej spójne, a dzięki podejściu obiektowemu informacje o np. 100 produktach możemy przechowywać w tylko 1 tablicy typu *Product[]*.

*plik Shop.java*

.. code-block:: java
    :linenos:

    public class Shop {
        public static void main(String[] args) {
            Product[] products = new Product[2];
            
            products[0] = new Product();
            products[0].name = "Samsung Syncmaster";
            products[0].price = 700.0;
            
            products[1] = new Product();
            products[1].name = "HP Probook 450";
            products[1].price = 3000.0;
            
            System.out.println("Produkty w sklepie: ");
            System.out.println(products[0].name + ":" + products[0].price);
            System.out.println(products[1].name + ": " + products[1].price);
        }
    }

**Ćwiczenie** *(10 min)*

    Wyobraź sobie, że tworzysz aplikację do diagnostyki komputerowej samochodów. Zacznij od utworzenia klasy Car przechowującej informacje o marce producenta, modelu, roku produkcji i mocy silnika. W drugiej klasie o nazwie CarDiagnostic utwórz dwa obiekty klasy Car i wyświetl informacje o samochodach na ekranie.

Struktura projektu:

.. image:: 02_obiekty/car_project1.png
    :align: center

*plik Car.java*

.. code-block:: java
    :linenos:

    public class Car {
        String carBrand; // marka samochodu
        String model;
        int year; //rok produkcji
        int horsePower; // ilość koni mechanicznych
    }

*plik CarDiagnostic.java*

.. code-block:: java
    :linenos:

    public class CarDiagnostic {
        public static void main(String[] args) {
            Car audiA4 = new Car();
            audiA4.carBrand = "Audi";
            audiA4.model = "A4";
            audiA4.year = 2008;
            audiA4.horsePower = 170;

            Car vwGolf = new Car();
            vwGolf.carBrand = "Volkswagen";
            vwGolf.model = "Golf";
            vwGolf.year = 2010;
            vwGolf.horsePower = 130;

            System.out.println("Samochód 1: ");
            System.out.println(audiA4.carBrand + " " + audiA4.model
                    + ", rok produkcji: " + audiA4.year + ", moc: "
                    + audiA4.horsePower);

            System.out.println("Samochód 2: ");
            System.out.println(vwGolf.carBrand + " " + vwGolf.model
                    + ", rok produkcji: " + vwGolf.year + ", moc: "
                    + vwGolf.horsePower);
        }
    }


Metody
----------------------
Klasa Product potrafi już przechowywać informacje o nazwie i cenie produktu, jednak jak wspomnieliśmy w definicji klasy jest to także zbiór funkcjonalności. W programowaniu obiektowym funkcjonalności danej klasy realizuje się poprzez utworzenie **metod**. W naszym przykładzie funkcjonalnością może być na przykład zwrócenie przez obiekt klasy Product nazwy oraz ceny w czytelnej formie, dzięki czemu w metodzie println() nie będziemy musieli się odwoływać do poszczególnych pól.

*plik Product.java*

.. code-block:: java
    :linenos:

    public class Product {
        String name;
        double price;
        
        String getProductInfo() {
            return name + ": " + price;
        }
    }

W klasie Product utworzyliśmy metodę **getProductInfo**. Ponieważ zwraca ona opisową formę produktu musieliśmy zadeklarować jej typ jako String. Wynik metody należy zwrócić za pomocą słowa kluczowego **return**.

.. note::
    Ogólna postać metody to:
    ::
        typ_zwracany nazwaMetody(opcjonalne_argumenty_metody) { 
          //ciało metody między nawiasami klamrowymi
        }
    Elementami opcjonalnymi są jeszcze specyfikatory dostępu (np. public) oraz oznaczenie metody jako statycznej (static) - do tego dojdziemy jednak niebawem. Metody mogą zwracać wynik (np. String) i wtedy musi w nich występować instrukcja **return**, ale mogą także nie zwracać żadnego wyniku - sytuacja taka będzie miała miejsce, gdy metoda ma za zadanie np. wydrukować coś na ekranie za pomocą System.out.print(). Jeżeli metoda nie zwraca żadnego wyniku należy jako jej typ zwracany podać słowo kluczowe **void**. Przykładem metody, która nie zwraca żadnego wyniku jest metoda **main**, którą poznałeś już na samym początku kursu.

*plik Shop.java*

.. code-block:: java
    :linenos:
    
    public class Shop {
        public static void main(String[] args) {
            Product[] products = new Product[2];
            
            products[0] = new Product();
            products[0].name = "Samsung Syncmaster";
            products[0].price = 700.0;
            
            products[1] = new Product();
            products[1].name = "HP Probook 450";
            products[1].price = 3000.0;
            
            System.out.println("Produkty w sklepie: ");
            System.out.println(products[0].getProductInfo());
            System.out.println(products[1].getProductInfo());
        }
    }

Do metod, podobnie jak do pól klasy odołujemy się za pomocą operatora kropki, jednak oprócz samej nazwy metody nie możemy zapomnieć o dodaniu na końcu okrągłych nawiasów.


Konstruktory
-----------------
Ostatnią rzeczą, którą możemy uprościć w klasie Shop jest inicjalizacja zmiennych. W chwili obecnej w celu utworzenia jednego tylko obiektu potrzebujemy aż 3 linijek kodu - w przypadku tworzenia 100 obiektów, kod rozrasta się do 300 linii - jest to niedopuszczalne.

Tworzenie obiektów możemy jednak uprościć za pomocą specjalnych metod nazywanych **konstruktorami**.

.. note::
    Konstruktor to specjalna metoda, która nie ma zadeklarowanego żadnego zwracanego typu (nawet void), a jej nazwa jest identyczna z nazwą klasy, w której się znajduje (z uwzględnieniem wielkości liter). Podobnie jak każda inna metoda może przyjmować argumenty.

*plik Product.java*

.. code-block:: java
    :linenos:

    public class Product {
        String name;
        double price;
        
        //konstruktor przyjmujący 2 argumenty
        Product(String n, double p) {
            name = n;
            price = p;
        }
        
        String getProductInfo() {
            return name + ": " + price;
        }
    }

Nasz konstruktor przyjmuje dwa argumenty - jeden typu String, a drugi typu double. Wartości przekazane jako argumenty konstruktora przypisujemy następnie do pól klasy, czyli *name* oraz *price*. Teraz możemy uprościć tworzenie obiektów w klasie Shop do tylko jednej linijki dla każdego z nich:

*plik Shop.java*

.. code-block:: java
    :linenos:
    
    public class Shop {
        public static void main(String[] args) {
            Product[] products = new Product[2];
            
            products[0] = new Product("Samsung Syncmaster", 700.0);
            
            products[1] = new Product("HP Probook 450", 3000.0);
            
            System.out.println("Produkty w sklepie: ");
            System.out.println(products[0].getProductInfo());
            System.out.println(products[1].getProductInfo());
        }
    }

.. attention::
    Każda klasa posiada domyślnie jeden niejawny konstruktor (bez parametrów). Jeżeli jednak zdefiniujesz w swojej klasie chociaż jeden konstruktor przyjmujący dowolne argumenty, to konstruktor domyślny przestaje istnieć.

**Ćwiczenie** *(10 min)*

    Rozwiń aplikację z poprzedniego ćwiczenia (diagnostyka sanmochodu) o następujące informacje. W klasie Car dodaj konstruktor pozwalający zainicjować wszystkie pola klasy oraz dwie metody: getInfo(), która zwróci opisową formę danego samochodu, a także upgreade(), która zwiększa moc silnika o tyle koni mechanicznych ile przekażemy jako jej parametr. Przetestuj nowe funkcjonalności w klasie CarDiagnostic.

*plik Car.java*

.. code-block:: java
    :linenos:

    public class Car {
        
        String carBrand; // marka samochodu
        String model;
        int year; //rok produkcji
        int horsePower; // ilość koni mechanicznych
        
        //konstruktor do zainicjowania wszystkich pól
        Car(String cb, String m, int y, int hp) {
            carBrand = cb;
            model = m;
            year = y;
            horsePower = hp;
        }
        
        //zwiększenie mocy silnika
        void upgreade(int hp) {
            horsePower = horsePower + hp;
        }
        
        //zwrócenie opisowej formy samochodu
        String getInfo() {
            return carBrand + " " + model + "; " + year + "; " + horsePower + "HP";
        }
    }

*plik CarDiagnostic.java*

.. code-block:: java
    :linenos:

    public class CarDiagnostic {
        public static void main(String[] args) {
            //utworzenie obiektów
            Car audiA4 = new Car("Audi", "A4", 2008, 170);
            Car vwGolf = new Car("Volkswagen", "Golf", 2010, 130);
            
            //tuning
            audiA4.upgreade(30);
            vwGolf.upgreade(20);

            //wydruk informacji
            System.out.println("Samochód 1: ");
            System.out.println(audiA4.getInfo());

            System.out.println("Samochód 2: ");
            System.out.println(vwGolf.getInfo());
        }
    }


Przeciążanie metod i konstruktorów
-----------------------------------
Czasami może zdarzyć się sytuacja, w której nie będziemy mieli pełnych informacji o danym produkcie, który chcielibyśmy utworzyć. Przykładowo będziemy mieli jego nazwę, ale będziemy musieli jeszcze chwilę poczekać na ustalenie jej ceny w centrali. W takiej sytuacji możemy utworzyć kilka konstruktorów, które pozwolą nam zainicjować obiekt danej klasy. Gdy w klasie istnieje kilka konstruktorów lub metod o takiej samej nazwie, ale różniących się przyjmowanymi parametrami, to będziemy mówili o nich, że są dostępne w kilku **przeciążonych** wersjach.

*plik Product.java*

.. code-block:: java
    :linenos:

    public class Product {
        String name;
        double price;
        
        //konstruktory
        Product(String n, double p) {
            name = n;
            price = p;
        }
        
        Product(String n) {
            name = n;
        }
        
        String getProductInfo() {
            return name + ": " + price;
        }
    }

W powyższym przykładzie widzimy, że utworzyliśmy drugi konstruktor, który inicjuje jedynie nazwę produktu. Cena (price) przyjmie w takiej sytuacji wartość domyślną, którą dla typu double jest 0.0. W podobny sposób możemy przeciążać dowolne metody, które będą miały takie same nazwy, ale przyjmą różne parametry.

**Ćwiczenie** *(5 min)*

    W programie do diagnostyki samochodu dopisz dodatkowy konstruktor pozwoli zainicjować jedynie markę i model samochodu, pozostawiając rok produkcji i moc silnika wartościami domyślnymi. W klasie CarDiagnostic utwórz za pomocą tego konstruktora nowy obiekt i wyświetl informacje o nim na ekranie.

*plik Car.java*

.. code-block:: java
    :linenos:

    public class Car {
        
        String carBrand; // marka samochodu
        String model;
        int year; //rok produkcji
        int horsePower; // ilość koni mechanicznych
        
        Car(String cb, String m) {
            carBrand = cb;
            model = m;
        }
        
        Car(String cb, String m, int y, int hp) {
            carBrand = cb;
            model = m;
            year = y;
            horsePower = hp;
        }
        
        void upgreade(int hp) {
            horsePower = horsePower + hp;
        }
        
        String getInfo() {
            return carBrand + " " + model + "; " + year + "; " + horsePower + "HP";
        }
    }

*plik CarDiagnostic.java*

.. code-block:: java
    :linenos:

    public class CarDiagnostic {
        public static void main(String[] args) {
            Car audiA4 = new Car("Audi", "A4", 2008, 170);
            Car vwGolf = new Car("Volkswagen", "Golf", 2010, 130);
            Car opelCorsa = new Car("Opel", "Corsa");
            
            //tuning
            audiA4.upgreade(30);
            vwGolf.upgreade(20);

            System.out.println("Samochód 1: ");
            System.out.println(audiA4.getInfo());

            System.out.println("Samochód 2: ");
            System.out.println(vwGolf.getInfo());
            
            System.out.println("Samochód 3: ");
            System.out.println(opelCorsa.getInfo());
        }
    }


Słowo kluczowe this
--------------------
Zarówno aplikacja symulująca sklep z częściami komputerowymi jak i program symulujący diagnostykę samochodową posiadają już pewną bazową funkcjonalność. Czytelność kodu w kilku miejscach można jednak poprawić.

Dobrą praktyką jest stosowanie wszędzie tam gdzie to możliwe opisowych nazw zmiennych, jednak w naszym przypadku ciężko wymyślić zamiennik dla "name", czy "year" (stosowanie polskich nazw również nie jest dobrą praktyką). Na szczęście przewidziano również taką funkcjonalność języka i nazwy argumentów metod, czy konstruktorów mogą być identyczne z nazwami pól klasy, te drugie należy jednak poprzedzić dodatkowo słowem kluczowym **this**.

Tym sposobem nasza poprawiona klasa Product prezentuje się jak poniżej.

*plik Product.java*

.. code-block:: java
    :linenos:

    public class Product {
        String name;
        double price;
        
        //konstruktory
        Product(String name, double price) {
            this.name = name;
            this.price = price;
        }
        
        Product(String name) {
            this.name = name;
        }
        
        String getProductInfo() {
            return name + ": " + price;
        }
    }

Zapis *this.name = name* należy rozumieć jako "przypisz do pola tej (this) klasy o nazwie *name* wartość argumentu konstruktora o nazwie *name*".

Słowo kluczowe this ma także drugie zastosowanie, które pozwala wywoływać przeciążoną wersję konstruktora w innym konstruktorze. Ma to takie zastosowanie, że pozwala zaoszczędzić powtarzalnego kodu w przypadku, gdy w klasie zdefiniujemy np. 4 podobne konstruktory, w których spora część kodu źródłowego się powtarza. U nas nie zaoszczędzimy specjalnie kodu, jednak wygląda to następująco:

*plik Product.java*

.. code-block:: java
    :linenos:

    public class Product {
        String name;
        double price;
        
        //konstruktory
        Product(String name, double price) {
            this(name); //wywołanie innego konstruktora
            this.price = price;
        }
        
        Product(String name) {
            this.name = name;
        }
        
        String getProductInfo() {
            return name + ": " + price;
        }
    }

Jak widzisz w konstruktorze przyjmującym dwa argumenty wywołujemy w pierwszej kolejności konstruktor z jednym argumentem poprzez this(name). W ten sposób zainicjowaliśmy pole name, a następnie możemy zainicjować cenę, czyli pole price.

**Ćwiczenie** *(5 min)*

    Popraw klasę Car z poprzedniego zadania w taki sposób, aby nazwy argumentów konstruktorów miały bardziej znaczące nazwy (najlepiej takie same jak nazwy pól klasy). Wykorzystaj także słowo kluczowe this w celu wywołania w jednym z konstruktorów drugiego konstruktora i zaoszczędzić tym samym powtarzalnego kodu.

*plik Car.java*

.. code-block:: java
    :linenos:

    public class Car {
        
        String carBrand; // marka samochodu
        String model;
        int year; //rok produkcji
        int horsePower; // ilość koni mechanicznych
        
        Car(String carBrand, String model) {
            this.carBrand = carBrand;
            this.model = model;
        }
        
        Car(String carBrand, String model, int year, int horsePower) {
            this(carBrand, model);
            this.year = year;
            this.horsePower = horsePower;
        }
        
        void upgreade(int hp) {
            horsePower = horsePower + hp;
        }
        
        String getInfo() {
            return carBrand + " " + model + "; " + year + "; " + horsePower + "HP";
        }
    }


Pakiety i modyfikatory dostępu
-------------------------------
Istotnym elementem, który pomaga w organizacji większych projektów jest podział klas i plików źródłowych na pakiety. Pakiety są niczym innym jak dodatkowymi folderami, które pozwalają grupować wspólnie klasy, które odpowiadają za podobne funkcjonalności. W celu utworzenia pakietu wybieramy po prostu New -> Package. Nazewnictwo pakietów zazwyczaj odzwierciedla nazwę domeny autorów, czyli np. pl.org.ceo.kursjava.pakiet1 - gdzie pakiet1 powinien być jakąś znaczącą nazwą, kursjava nazwą projektu, a pl.org.ceo to odwrócona nazwa domeny Centrum Edukacji Obywatelskiej.

Gdy w kodzie projektu CarDiagnoser podzielimy klasy na dwa pakiety (przeciągnij klasy do odpowiednich pakietów):

.. image:: 02_obiekty/package.png
    :align: center

zauważamy błąd w klasie CarDiagnostic.

*plik CarDiagnostic.java*

.. code-block:: java
    :linenos:

    package pl.org.ceo.cardiagnoser.app;
    import pl.org.ceo.cardiagnoser.data.Car;

    public class CarDiagnostic {
    //kod bez zmian
    }

Pierwszą rzeczą, na którą warto zwrócić uwagę są dwie linie kodu, które eclipse dodał automatycznie:

* package oznacza pakiet, w którym umieszczona jest dana klasa
* import jest dyrektywą niezbędną w przypadku, gdy korzystamy z klas umieszczonych w innych pakietach. W naszym przypadku musieliśmy zaimportować klasę Car.

Błąd w klasie polega na tym, że konstruktor jest niewidoczny:

.. image:: 02_obiekty/constructor.png
    :align: center

Można sobie zadać pytanie, ale jak to niewidoczny, skoro przed chwilą z niego korzystaliśmy? Jest to spowodowane różnymi zasięgami widoczności pól metod i konstruktorów. W Javie istnieją cztery możliwe zasięgi:

* default - domyślny, czyli pakietowy zasięg dostępu
* public - publiczny, można się odwoływać z dowolnego miejsca w pakiecie i poza nim
* protected - zasięg ograniczony do danego pakietu
* private - zasięg tylko w ramach jednej klasy. Do tak oznaczonych pól, metod i konstruktorów nie można odwołać się nawet z klas w tym samym pakiecie

Ponieważ klasy Car i CarDiagnoser znajdują się w różnych pakietach odpowiednie konstruktory i pola musimy oznaczyć jako publiczne:

*plik Car.java*

.. code-block:: java
    :linenos:

    package pl.org.ceo.cardiagnoser.data;

    public class Car {
        
        public String carBrand; // marka samochodu
        public String model;
        public int year; //rok produkcji
        public int horsePower; // ilość koni mechanicznych
        
        public Car(String carBrand, String model) {
            this.carBrand = carBrand;
            this.model = model;
        }
        
        public Car(String carBrand, String model, int year, int horsePower) {
            this(carBrand, model);
            this.year = year;
            this.horsePower = horsePower;
        }
        
        public void upgreade(int hp) {
            horsePower = horsePower + hp;
        }
        
        public String getInfo() {
            return carBrand + " " + model + "; " + year + "; " + horsePower + "HP";
        }
    }

.. note::
    Nasze przykłady najczęściej będą na tyle proste, że dla uproszczenia wszystkie pliki będą znajdowały się w jednym pakiecie.


Ćwiczenie podsumowujące (20 minut)
-----------------------------------
Napisz prosty kalkulator, który będzie zgodny z podejściem programowania obiektowego. Niech składa się on z dwóch klas:

* Calculator - klasa, w której zdefiniujesz metody add(), subtract(), multiply() oraz divide() odpowiedzialne odpowiednio za dodawanie, odejmowanie, mnożenie i dzielenie. Każda z metod powinna przyjmować dwa argumenty typu double, na których wykonuje obliczenie i zwraca w wynik.
* Main - klasa z metodą main(), w której należy przetestować działanie poszczególnych metod. Argumentami metod powinny być dwie wcześniej zadeklarowane zmienne.

*plik Calculator.java*

.. code-block:: java
    :linenos:

    public class Calculator {
        double add(double a, double b) {
            return a + b;
        }

        double subtract(double a, double b) {
            return a - b;
        }

        double multiply(double a, double b) {
            return a * b;
        }

        double divide(double a, double b) {
            return a / b;
        }
    }

*plik Calculator.java*

.. code-block:: java
    :linenos:

    public class Main {
        public static void main(String[] args) {
            double a = 25;
            double b = 5;
            Calculator calc = new Calculator();
            
            System.out.println(a + "+" + b + " = " + calc.add(a, b));
            System.out.println(a + "-" + b + " = " + calc.subtract(a, b));
            System.out.println(a + "*" + b + " = " + calc.multiply(a, b));
            System.out.println(a + "/" + b + " = " + calc.divide(a, b));
        }
    }