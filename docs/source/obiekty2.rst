Programowanie obiektowe 2
==========================

W tej lekcji dowiesz się:

* W jaki sposób odczytywać dane od użytkownika
* Na czym polega dziedziczenie
* Do czego wykorzystujemy słowo kluczowe super
* Czym jest przesłanianie metod
* Co to jest polimorfizm
* Do czego służy operator instanceof
* Czym są klasy abstrakcyjne i interfejsy
* Co oznacza słowo static


Odbieranie danych od użytkownika
---------------------------------
W naszych dotychczasowych programach skupialiśmy się na poznawaniu podstawowych mechanizmów języka Java, jednak brakowało w nich interakcji z użytkownikiem. Biblioteka wejścia/wyjścia w Javie jest dosyć rozbudowana jednak w tej części kursu poznamy podstawowe sposoby interakcji człowieka z komputerem.

Najprostszą klasą, która pozwoli odczytywać dane od użytkownika jest **Scanner**. Posiada ona zestaw użytecznych metod, które pozwolą nam wczytać zarówno liczby jak i napisy. Klasa ta jest dosyć uniwersalna, więc w zależności od tego co przekażemy w konstruktorze podczas inicjalizacji możemy za pomocą jej obiektu zarówno odczytywać dane od użytkownika, które będą wprowadzane z klawiatury, ale także odczytywać informacje zawarte w plikach.

Inicjalizacja obiektu Scanner w przypadku, gdy chcemy odczytać dane od użytkownika wygląda następująco:

.. code-block:: java
    :linenos:

    import java.util.Scanner;

    public class Example {
        Scanner sc = new Scanner(System.in);
    }

Przede wszystkim, aby korzystać z klasy Scanner w swoim kodzie, musisz ją najpierw zaimportować z pakietu java.util (np. wykorzystując skrót Ctrl+Shift+O w eclipse). Obiekt tworzony jest przez przekazanie w konstruktorze standardowego strumienia wejścia, którym jest *System.in*.

Następnie do odczytu danych należy skorzystać z jednej z metod zawartych w klasie Scanner:

* nextInt() - odczytanie kolejnej liczby typu int
* nextDouble() - odczytanie kolejnej liczby typu double (uwaga, domyślny separator dziesiętny jest zależny od ustawień maszyny wirtualnej, w Polsce domyślnie liczby należy wprowadzać rozdzielone przecinkiem)
* nextLine() - wczytanie kolejnego wiersza tekstu (String) zakończonego znakiem nowej linii '\n'
* analogicznie dla innych typów danych istnieją metody takie jak nextBoolean(), nextLong() itp.

Ważne jest to, że w przypadku, gdy odczytywać będziemy liczby, w buforze nadal pozostanie znak nowej linii, który należy wczytać za pomocą metody *nextLine()*. Przykład:

.. code-block:: java
    :linenos:

    import java.util.Scanner;

    public class SimpleInput {
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            
            System.out.println("Podaj pierwszą liczbę: ");
            //wczytujemy wartość zmiennej z klawiatury
            int num1 = sc.nextInt();
            //usuwamy znak nowej linii z bufora
            sc.nextLine();
            
            System.out.println("Podaj drugą liczbę: ");
            //wczytujemy drugą liczbę
            int num2 = sc.nextInt();
            //usuwamy znak nowej linii z bufora
            sc.nextLine();
            
            System.out.println("Suma podanych liczb to: " + (num1 + num2));
            
            System.out.println("Jak masz na imię? ");
            //wczytujemy napis
            String name = sc.nextLine();
            
            System.out.println("Witaj " + name + "!");
            
            //po zakończonym odczycie zamykamy strumień wejścia
            sc.close();
        }
    }

.. attention::
    Pamiętaj, że po zakończeniu pracy ze strumieniami wejścia lub wyjścia, szczególnie odczytu plików należy je zamykać za pomocą metody close(). Analogicznie będzie należało postępować z operacjami wyjścia, czyli np. zapisem danych do plików.

**Ćwiczenie** *(10 min)*

    Napisz prosty program do zarządzania książkami biblioteki. Powinien się on składać z dwóch klas:
    
    * Book - klasa reprezentująca pojedynczą książkę. Powinna posiadać pola reprezentujące numer ISBN, tytuł oraz autora
    * BookManager - główna klasa aplikacji, w której wczytasz od użytkownika dotyczące 1 książki, utworzysz na ich podstawie obiekt klasy Book i wyświetlisz odpowiednie informacje na ekranie. Zadbaj o utworzenie odpowiednich konstruktorów.

.. image:: 04_obiekty2/library1_project.png
    :align: center

*plik Book.java*

.. code-block:: java
    :linenos:

    public class Book {
        String isbn;
        String title;
        String author;

        Book(String isbn, String title, String author) {
            this.isbn = isbn;
            this.title = title;
            this.author = author;
        }

        String getBookInfo() {
            return isbn + " - " + title + " - " + author;
        }
    }

*plik BookManager.java*

.. code-block:: java
    :linenos:

    import java.util.Scanner;

    public class BookManager {
        public static void main(String[] args) {
            //tworzymy obiekt do odczytu danych
            Scanner sc = new Scanner(System.in);

            //prosimy użytkownika o podanie odpowiednich danych
            System.out.println("Podaj ISBN: ");
            String isbn = sc.nextLine();
            System.out.println("Podaj Tytuł: ");
            String title = sc.nextLine();
            System.out.println("Podaj autora: ");
            String author = sc.nextLine();
            
            //zamykamy strumień wejścia
            sc.close();

            //tworzymy obiekt Book i wyświetlamy informacje na ekranie
            Book book = new Book(isbn, title, author);
            System.out.println(book.getBookInfo());
        }
    }

Przykładowy wynik działania programu:

.. image:: 04_obiekty2/output1.png
    :align: center


Dziedziczenie
---------------------------------
W naszej aplikacji możemy teraz przechowywać i wczytywać od użytkownika informacje o książkach, jednak warto zauważyć, że przecież w bibliotece oprócz książek są także komiksy, gazety, ogólnie rzecz ujmując magazyny. W tym momencie należałoby stworzyć osobną klasę Magazine, która przechowywać będzie również numer ISBN, tytuł, oraz np. wydawnictwo. Problemem jest to, że pewne dane zaczynają się tutaj powielać, a tego w programowaniu zdecydowanie powinniśmy unikać.

W celu rozwiązania m.in. tego problemu powstał paradygmat programowania obiektowego, które pozwala budować hierarchię klas, dającą możliwość tworzenia kodu, który może być wykorzystywany wielokrotnie i być bazą do tworzenia jeszcze kolejnych klas.

.. image:: 04_obiekty2/inheritance.png
    :align: center

Na powyższym diagramie widać bazową klasę **Publication**, po której dziedziczą dwie kolejne klasy: **Book** oraz **Magazine**. Różnią się one tym, że książka posiada najczęściej jednego autora (np. Henryk Sienkiewicz), natomiast w przypadku gazety w tym miejscu pojawi się wydawnictwo (np. Ringier Axel Springer).

.. Note::
    Jeżeli istnieje pewna klasa **A**, a poniej dziedziczy pewna klasa **B**, to klasa **B** przejmuje wszystkie widoczne cechy klasy **A**.

Powyższa regułka oznacza, że w przypadku, gdy spojrzymy na powyższy diagram, klasa **Book**, czy **Magazine** będą posiadały także pola z klasy **Publication**, czyli *isbn* oraz *title*. W Javie dziedziczenie można osiągnąć stosując słowo kluczowe **extends** w definicji klasy, np.:

.. image:: 04_obiekty2/inherit.png
    :align: center

*plik Publication.java*

.. code-block:: java
    :linenos:

    public class Publication {
        String isbn;
        String title;

        Publication(String isbn, String title) {
            this.isbn = isbn;
            this.title = title;
        }
        
        String getInfo() {
            return title + " - " + isbn;
        }
    }

*plik Book.java*

.. code-block:: java
    :linenos:

    public class Book extends Publication {
        String author;

        Book(String isbn, String title, String author) {
            super(isbn, title);
            this.author = author;
        }

        String getInfo() {
            return super.getInfo() + " - " + author;
        }
    }


*plik Magazine.java*

.. code-block:: java
    :linenos:

    public class Magazine extends Publication {
        String publisher;

        Magazine(String isbn, String title, String publisher) {
            super(isbn, title);
            this.publisher = publisher;
        }

        String getInfo() {
            return super.getInfo() + " - " + publisher;
        }
    }

Zwróć uwagę, że pomimo iż w klasach *Book* oraz *Magazine* nie zadeklarowaliśmy pól *isbn* oraz *title* to mamy do nich dostęp w konstruktorze, czy metodach *getBookInfo()* i *getMagazineInfo()*, ponieważ dziedziczą one te cechy z klasy Publication.

Kolejną nowością jest zastosowanie specjalnej konstrukcji **super()**. Działa ona w sposób podobny do słowa kluczowego *this*, którego używaliśmy w przypadku, gdy posiadaliśmy kilka przeciążonych wersji konstruktora z tą różnicą, że wywołuje konstruktor nadklasy z odpowiednimi parametrami.

Ostatnia rzecz dotyczy **przesłaniania** metod. W klasie Publication zdefiniowaliśmy metodę *getInfo()*. Jeżeli w klasie dziedzicząsej (Book lub Magazine) zdefiniujemy metodę o takiej samej sygnaturze, to powiemy, że przesłania ona oryginalną metodę *getInfo()*. W celu wywołania metody nadklasy należy posłużyć się w takiej sytuacji zapisem **super.getInfo()**.

.. note::
    Pierwszą, niejawną instrukcją jaka jest wykonywana w podklasie pewnej klasy bazowej jest wywołanie konstruktora nadklasy poprzez **super()**. Jeżeli w klasie bazowej nie jest zdefiniowany konstruktor bezparametrowy to należy jawnie wywołać konstruktor z odpowiednimi argumentami poprzez zapis **super(lista_parametrow)**. Jeżeli chcesz natomiast wywołać w którejś z metod metodę z nadklasy wykorzystaj zapis **super.nazwaMetody(parametry)**.

.. note::
    Jeżeli chcesz zaznaczyć, że jakaś metoda jest przesłonięta możesz zastosować dodatkową adnotację *@Override*. W wielu sytuacjach będzie ona automatycznie wygenerowana przez eclipse, warto więc rozumieć co oznacza.
    ::
        @Override
        String getInfo() {
            return super.getInfo() + " - " + author;
        }


**Ćwiczenie** *(10 min)*
    W klasie BookManager utwórz tablice do przechowywania książek oraz magazynów. W każdej tablicy utwórz przynajmniej po jednym obiekcie danego typu, a następnie wyświetl je na ekranie.

*plik BookManager.java*

.. code-block:: java
    :linenos:

    import java.util.Scanner;

    public class BookManager {
        public static void main(String[] args) {
            // tworzymy obiekt do odczytu danych
            Scanner sc = new Scanner(System.in);

            // tworzymy tablice
            Book[] books = new Book[10];
            Magazine[] magazines = new Magazine[10];

            // prosimy użytkownika o podanie informacji o książce
            System.out.println("Podaj ISBN książki: ");
            String isbn = sc.nextLine();
            System.out.println("Podaj Tytuł książi: ");
            String title = sc.nextLine();
            System.out.println("Podaj autora książki: ");
            String author = sc.nextLine();

            books[0] = new Book(isbn, title, author);

            // prosimy użytkownika o podanie informacji o magazynie
            System.out.println("Podaj ISBN magazynu: ");
            isbn = sc.nextLine();
            System.out.println("Podaj Tytuł magazynu: ");
            title = sc.nextLine();
            System.out.println("Podaj wydawcę magazynu: ");
            String publisher = sc.nextLine();

            magazines[0] = new Magazine(isbn, title, publisher);

            // zamykamy strumień wejścia
            sc.close();

            // tworzymy obiekt Book i wyświetlamy informacje na ekranie
            System.out.println(books[0].getInfo());
            System.out.println(magazines[0].getInfo());
        }
    }

W porównaniu do wcześniejszego kodu zyskaliśmy teraz możliwosć przechowywania informacji zarówno o ksiażkach jak i innego rodzaju publikacjach. Ponieważ przechowujemy je w tablicach to dodatkowo dane są teraz bardziej spójne.


Polimorfizm
---------------------------------
Wcześniej wspomnieliśmy, że dziedziczenia warto używać między innymi po to, żeby zaoszczędzić konieczności powielania tego samego kodu. Problem w tym, że gdy tak jak w powyższym przykładzie stosujemy tablice oddzielnych typów to operacje na nich i tak będą powielane, np:
::
    System.out.println("Książki: ");
    for(int i=0; i < books.length; i++) {
        if(books[1] != null)
            System.out.println(books[i].getInfo());
    }

    System.out.println("Magazyny: ");
    for(int i=0; i < magazines.length; i++) {
        if(magazines[1] != null)
            System.out.println(magazines[i].getInfo());
    }

Lepiej by było tak naprawdę przechowywać wszystkie te dane w jednej wspólnej tablicy typu *Publication*, a następnie ewentualnie rozpoznać, czy dany element tablicy jest typu Book, czy Magazine.

Efekt taki można osiągnąć dzięki zastosowaniu **polimorfizmu** lub inaczej mówiąc wielopostaciowości. Polega to na tym, że do ogólnej, wspólnej referencji można przypisać obiekty różnych typów, które po typie tej referencji dziedziczą.

W naszym przypadku klasy Book i Magazine dziedziczą po klasie Publication, więc jak najbardziej możliwe jest zapisanie:
::
    Publication pub1 = new Book(argumenty_konstruktora);
    Publication pub2 = new Magazine(argumenty_konstruktora);

W podobny sposób możemy także utworzyć tablicę typu Publication, która będzie przechowywała zarówno książki jak i magazyny. Pamiętać należy jednak o dwóch rzeczach:

1. Zawsze mamy dostęp jedynie do metod z typu referencji. Jeżeli w klasie Book zdefiniujemy dodatkową metodę changeBookTitle(), a referencja będzie typu Publication, to nie będziemy mieli dostępu do takiej metody. Jeśżeli chcesz uzyskać dostęp do wszystkich metod (również tych nie zdefiniowanych w typie nadrzędnym) musisz skorzystać z rzutowania typu. Rzutowanie polega na wykorzystaniu nawiasu, np.:
::
    Publication pub = new Magazine(...);
    ((Magazine)pub).metodaZKlasyMagazine();

2. Metody będą wywoływane na rzecz typu obiektu, a nie typu referencji. Jeżeli zapisaliśmy *Publication pub1 = new Book(argumenty_konstruktora);* to wywołując metodę pub1.getInfo() wywołamy jej wersję z klasy Book, a nie Publication.


**Ćwiczenie** *(5 min)*

    Przerób klasę BookManager w taki sposób, aby przechowywać książki oraz magazyny w jednej wspólnej tablicy typu Publication[].

*plik BookManager.java*

.. code-block:: java
    :linenos:

    import java.util.Scanner;

    import java.util.Scanner;

    public class BookManager {
        public static void main(String[] args) {
            // tworzymy obiekt do odczytu danych
            Scanner sc = new Scanner(System.in);

            // tworzymy tablice
            Publication[] publications = new Publication[10];

            // prosimy użytkownika o podanie informacji o książce
            System.out.println("Podaj ISBN książki: ");
            String isbn = sc.nextLine();
            System.out.println("Podaj Tytuł książi: ");
            String title = sc.nextLine();
            System.out.println("Podaj autora książki: ");
            String author = sc.nextLine();

            publications[0] = new Book(isbn, title, author);

            // prosimy użytkownika o podanie informacji o magazynie
            System.out.println("Podaj ISBN magazynu: ");
            isbn = sc.nextLine();
            System.out.println("Podaj Tytuł magazynu: ");
            title = sc.nextLine();
            System.out.println("Podaj wydawcę magazynu: ");
            String publisher = sc.nextLine();

            publications[1] = new Magazine(isbn, title, publisher);

            // zamykamy strumień wejścia
            sc.close();

            // wyświetlamy informacje na ekranie
            System.out.println("Książki i magazyny: ");
            for (Publication p: publications) {
                if (p != null)
                    System.out.println(p.getInfo());
            }
        }
    }

Dzięki uproszczeniu sposobu przechowywania danych wyświetlanie danych odbywa się w jednej tylko pętli, dane są bardziej spójne, a kod bardziej przejrzysty.


Operator instanceof
--------------------
W poprzednim przykładzie brakuje w tej chwili trochę informacji o tym, czy dana pozycja w tablicy *publications* jest typu Book, czy Magazine. W niektórych sytuacjach chcielibyśmy mieć taką informację w celu zwiększenia przejrzystości. W Javie możemy sprawdzić rzeczywisty typ obiektu (nie typ referencji) za pomocą operatora **instanceof**. Jego składnia jest następująca:

.. code-block:: java

    obiekt instanceof NazwaKlasy

np.

.. code-block:: java

    Publication book = new Book("123456789", "W pustyni i w puszczy", "Henryk Sienkiewicz");
    if(book instanceof Book) {
        System.out.println("To jest książka");
    }

Na ekranie zobaczymy tekst "To jest ksiażka".

Sprawdzenie obiektu za pomocą operatora instanceof zwraca w wyniku true lub false, więc jak widać w powyższym przykładzie, może on być zastosowany jako warunek w instrukcji if.

**Ćwiczenie** *(5 min)*

    Przerób kod klasy BookManager w taki sposób, aby dane były wyświetlane w formie:
    ::
        Książka: 123456789 - W pustyni i w puszczy - Henryk Sienkiewicz
        Magazyn: 987654321 - Wprost - Wydawnictwo Wprost

*plik BookManager.java*

.. code-block:: java
    :linenos:

    import java.util.Scanner;

    public class BookManager {
        public static void main(String[] args) {
            // tworzymy obiekt do odczytu danych
            Scanner sc = new Scanner(System.in);

            // tworzymy tablice
            Publication[] publications = new Publication[10];

            // prosimy użytkownika o podanie informacji o książce
            System.out.println("Podaj ISBN książki: ");
            String isbn = sc.nextLine();
            System.out.println("Podaj Tytuł książi: ");
            String title = sc.nextLine();
            System.out.println("Podaj autora książki: ");
            String author = sc.nextLine();

            publications[0] = new Book(isbn, title, author);

            // prosimy użytkownika o podanie informacji o magazynie
            System.out.println("Podaj ISBN magazynu: ");
            isbn = sc.nextLine();
            System.out.println("Podaj Tytuł magazynu: ");
            title = sc.nextLine();
            System.out.println("Podaj wydawcę magazynu: ");
            String publisher = sc.nextLine();

            publications[1] = new Magazine(isbn, title, publisher);

            // zamykamy strumień wejścia
            sc.close();
            
            // wyświetlamy informacje na ekranie
            System.out.println("Książki i magazyny: ");
            for (Publication p: publications) {
                if (p != null)
                    if(p instanceof Book) {
                        System.out.println("Książka: " + p.getInfo());
                    } else if(p instanceof Magazine) {
                        System.out.println("Magazyn: " + p.getInfo());
                    }
            }
        }
    }


Klasy abstrakcyjne i interfejsy
--------------------------------
Nasza aplikacja jest już prawie gotowa, jednak warto się zastanowić nad jeszcze jedną rzeczą. Stworzyliśmy klasę Publication, która jest klasą bazową dla dwóch konkretnych typów danych, które wykorzystujemy. Ponieważ nie chcemy używać obiektów klasy Publication bezpośrednio, powinniśmy z niej zrobić jedynie pewną warstwę abstrakcji, czyli klasę, która jest jedynie klasą bazową dla innych typów, ale nie można tworzyć jej obiektów bezpośrednio.

W Javie można to osiągnąć wykorzystując **klasę abstrakcyjną**. Jeżeli jakaś klasa ma być abstrakcyjna, należy w jej sygnaturze dodać o tym informację za pomocą słowa kluczowego **abstract**.

*plik Publication.java*

.. code-block:: java
    :linenos:

    public abstract class Publication {
        String isbn;
        String title;

        Publication(String isbn, String title) {
            this.isbn = isbn;
            this.title = title;
        }
        
        String getInfo() {
            return title + " - " + isbn;
        }
    }

Klasy abstrakcyjne mają dwie ważne właściwości, o których musisz pamiętać:

#. Nie można utworzyć obiektu klasy abstrakcyjnej za pomocą operatora new. Typ abstrakcyjny może być jedynie typem referencji dla typów bardziej sprecyzowanych (dziedziczących po tej klasie abstrakcyjnej).

#. W klasie abstrakcyjnej mogą być zdefiniowane **metody abstrakcyjne**, czyli metody, które określają jedynie sygnaturę metody, ale nie posiadają implementacji. Jeżeli jakaś klasa posiada chociaż jedną metodę abstrakcyjną, to musi być oznaczona jako klasa abstrakcyjna. Każda klasa, która dziedziczy po klasie abstrakcyjnej musi posiadać konkretną implementację każdej metody abstrakcyjnej.

Przykład metody abstrakcyjnej:

*plik Publication.java*

.. code-block:: java
    :linenos:

    public abstract class Publication {
        String isbn;
        String title;

        Publication(String isbn, String title) {
            this.isbn = isbn;
            this.title = title;
        }
        
        String getInfo() {
            return title + " - " + isbn;
        }
        
        abstract void printInfo();
    }

Jak widzisz metoda *printInfo()* oznaczona jest słowek abstract. Nie posiada ona żadnej implementacji - nie posiada nawet nawiasów klamrowych i zakończona jest średnikiem.

.. image:: 04_obiekty2/abstract.png
    :align: center

Jak widzisz dodanie metody abstrakcyjnej powoduje błędy w naszym projekcie w klasach dziedziczących po Publication. W celu ich wyeliminowania powinniśmy zaimplementować metodę *printInfo()* w tych klasach, np.:

*plik Book.java*

.. code-block:: java
    :linenos:

    public class Book extends Publication {
        String author;

        Book(String isbn, String title, String author) {
            super(isbn, title);
            this.author = author;
        }

        String getInfo() {
            return super.getInfo() + " - " + author;
        }
        
        @Override
        void printInfo() {
            System.out.println(getInfo());
        }
    }

analogicznie w klasie Magazine:

*plik Magazine.java*

.. code-block:: java
    :linenos:

    public class Magazine extends Publication {
        String publisher;

        Magazine(String isbn, String title, String publisher) {
            super(isbn, title);
            this.publisher = publisher;
        }

        String getInfo() {
            return super.getInfo() + " - " + publisher;
        }

        @Override
        void printInfo() {
            System.out.println(getInfo());  
        }
    }

W Javie istnieją także klasy, które są w pełni abstrakcyjne nazywane **interfejsami**, czyli posiadają jedynie metody abstrakcyjne oraz ewentualnie zdefiniowane stałe wartości (oznaczone jako public static). Interfejsy definiujemy za pomocą słowa kluczowego **interface**, a implementujemy je (analogia do dziedziczenia) za pomocą słowa kluczowego **implements**. Na tym etapie nie będziemy się zagłębiali w stosowanie interfejsów w swoich programach, spójrz jedynie na przykład i zapamiętaj kilka rzeczy zapisanych poniżej.

Przykład interfejsu:

.. code-block:: java
    :linenos:

    public interface Moveable {
        void move();

        void stop();
    }

Klasa implementująca interfejs Moveable:

.. code-block:: java
    :linenos:

    public class Car implements Moveable {

        public void move() {
            System.out.println("Samochód start");
        }

        public void stop() {
            System.out.println("Samochód stop");
        }
    }

Ponieważ wszystkie metody interfejsu muszą być abstrakcyjne, to nie jest konieczne jawne używanie słowa kluczowego abstract - jest ono niejawnie dodawane automatycznie.

**Zapamiętaj**

#. W Javie można dziedziczyć tylko po jednej klasie (extends), nawet jeśli są to klasy abstrakcyjne.

#. Możesz implementować dowolną liczbę interfejsów (implements).


Dodatek - składowe statyczne
--------------------------------
Jako dodatek tej lekcji wytłumaczymy jescze krótko co oznacza słowo kluczowe **static**. W niektorych sytuacjach chcielibyśmy mieć dostęp do metody, czy pola klasy bez konieczności tworzenia obiektu. Przykładem takiej metody jest dobrze nam już znana metoda *main()*.

Jeżeli zdefiniujemy jakieś pole lub metodę jako statyczne, to możemy się do nich odwoływać bez tworzenia obiektu takiej klasy poprzez konstrukcję NazwaKlasy.nazwaPolaStatycznego lub NazwaKlasy.metodaStatyczna().

Najważniejszą rzeczą dotyczącą składowych statycznych klasy jest to, że w metodach statycznych możemy odwoływać się jedynie do innych elementów statycznych tej klasy.
