Wprowadzenie do języka Java
============================

.. highlight:: java

W lekcji tej dowiesz się:
* jak używać środowiska eclipse
* co powstaje w wyniku kompilacji kodu źródłowego napisanego w języku Java
* jak uruchamiać swoje programy
* czym są zmienne
* jakie są podstawowe typy danych w Javie
* jak wykonywać podstawowe operacje arytmetyczne

Podczas lekcji nauczyciel powinien krótko omawiać daną sekcję pod względem teoretycznym, a następnie omawiać prezentowane przykłady, prosząc z wyprzedzeniem uczniów o sugestie tego jak zapisać dalszy fragment kodu źródłowego. Uczniowie powinni pisać kod jednocześnie na swoich komputerach, aby móc obserwować wynik działania aplikacji. Lekcja kończy się zadaniem do samodzielnego wykonania, które należy wykonać na zajęciach lub dokończyć samodzielnie w domu.

Eclipse IDE
------------

Eclipse to najpopularniejsze środowisko programistyczne wśród programistów Java. Jego głównymi zaletami z punktu widzenia osoby początkującej jest wykrywanie błędów w trakcie pisania kodu a także podpowiadanie składni wraz z możliwością generowania i prostej modyfikacji (refaktoryzacji) kodu źródłowego. Wszystko to składa się przede wszystkim na znaczną oszczędność czasu.

Przy pierwszym uruchomieniu środowiska zostaniemy zapytani o wskazanie folder "workspace" - jest to folder, w którym przechowywane będą tworzone przez nas projekty. Zalecamy zostawić tę lokalizację domyślną.

.. image:: 01_wprowadzenie/eclipse_overview.png

Na powyższym zrzucie ekranu widać domyślny widok, który zastaniemy po uruchomieniu środowiska eclipse. Można w nim wyróżnić 4 główne obszary:

#. Package Explorer - w tym miejscu będziemy widzieli wszystkie projekty, które zapisane są w folderze **workspace** oraz ich strukturę w postaci rozwijanego drzewa
#. Obszar oznaczony numerem 2 to główna część robocza - w tym miejscu będziemy edytowali kod źródłowy aplikacji
#. Outline - to skrótowy podgląd danego pliku i elementów w nim zawartych (zmienne, metody/funkcje)
#. W dolnej części ekranu znajduje się kilka zakładek. Najważniejsza z nich to **Problems**, która pokazuje wszelkie błędy i ostrzeżenia występujące w kodzie źródłowym. W tym miejscu zobaczymy także dodatkową zakładkę **Console** z wydrukami generowanymi przez nasze aplikacje.

Pierwszy Projekt
-----------------

W celu utworzenia nowego projektu wybieramy opcje **File -> New -> Java Project**

.. image:: 01_wprowadzenie/eclipse_first_project.png

Wpisujemy dowolną nazwę projektu w polu **Project name** a także wybieramy wersję maszyny wirtualnej (JRE), na jakiej ma być uruchomiony nasz program (domyślnie JavaSE-1.8). Klikamy Finish.

W obszarze Project Explorer pojawi się nowo utworzony projekt:

.. image:: 01_wprowadzenie/hello_world.png

Widzimy tu folder **src**, w którym umieszczane będą pliki z kodem źródłowym, a także dołączoną wirtualną maszynę, na której nasz projekt będzie uruchamiany.

Teraz należy utworzyć plik, w którym będziemy edytowali kod źródłowy. W dalszej częściu kursu będziemy mówili krótko o tworzeniu nowej **klasy**. Klasa jest pojęciem związanym z programowaniem obiektowym, które będzie głównym zagadnieniem kolejnej lekcji.

Kliknij prawym przyciskiem na folderze src i wybierz opcję **New -> Class**. Można także posłużyć się wygodnym skrótem i skorzystać z przycisku z symbolem C, który znajdziemy na górnym pasku nawigacyjnym.

.. image:: 01_wprowadzenie/new_class.png

W kreatorze klasy wymagane jest podanie jedynie nazwy klasy. My jednak zaznaczymy także opcję przy **public static void main(String[] args)**.

.. Note::
    Nazwy klas rozpoczynaj zawsze wielką literą a jeżeli nazwa składa się z kilku wyrazów to je także rozpoczynaj wielką literą, np. NazwaTwojejKlasy albo ThisIsMyClass


.. image:: 01_wprowadzenie/class.png

W naszym przypadku nazwą klasy jest **FirstClass**.

.. attention::
    Zapamiętaj, że w Javie nazwy klasy, ale także zmiennych, czy metod o których niebawem się dowiesz mają znaczenie. "NazwaKlasy" i "nazwaKlasy" będą potraktowane jako dwa zupełnie różne elementy.


W utworzonej przez nas klasie został wygenerowany następujący kod źródłowy:

    public class FirstClass {
    
      public static void main(String[] args) {
        // TODO Auto-generated method stub
    
      }
    
    }

asdf

.. _Centrum Edukacji Obywatelskiej: http://www.ceo.org.pl/