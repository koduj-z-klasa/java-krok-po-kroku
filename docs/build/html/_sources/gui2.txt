Graficzny interfejs użytkownika cz.2
=====================================

W tej lekcji dowiesz się:

* w jaki sposó obsługiwać zdarzenia przycisków
* jak dodać dodać obsługę zdarzeń myszy lub klawiatury
* czym są klasy anonimowe


Obsługa zdarzeń
----------------
W poprzedniej lekcji dowiedziałeś się w jaki sposób stworzyć prosty graficzny interfejs użytkownika korzystając z Javy FX, na czym polega architektura MVC i jak powiązać kontrolki zdefiniowane w pliku fxml ze zmiennymi w klasie kontrolera, a także jak z poziomu kodu Javy ustawiać ich właściwości na przykładzie metody *setText()*.

Problemem jest jednak to, że zdefiniowane menu nawigacyjne nie reaguje w żaden sposób na akcje wykonywane przez użytkownika, zliczanie słów oraz wyrazów także jest zakodowane na sztywno i nie reaguje na to co użytkownik wprowadza do pola tekstowego.

Rozwiązaniem tych problemów jest podpięcie pod poszczególne z kontrolek odpowiednich zdarzeń, które w odpowiedzi wykonają pewną akcję. Istnieje kilka metod na realizację tego zadania, jednak my skupimy się na dwóch.


Podpięcie zdarzeń przycisków
-----------------------------
Jednymi z najczęściej powtarzających się opcji w niemal każdej aplikacji jest odpowiedź aplikacji na akcję użytkownika polegającą na wciśnięcie zwykłęgo przycisku (w JavieFX reprezentowany przez klasę Button). W naszym przykładowym programie nie posiadamy na tę chwilę żadnego przycisku, jednak nic nie stoi na przeszkodzie, żeby go dodać.

*hierarchia Editor.fxml*

.. image:: 07_fx2/button-hierarchy.png
    :align: center

*plik Editor.fxml*

.. code-block:: xml
    :linenos:

    <?xml version="1.0" encoding="UTF-8"?>

    <?import javafx.scene.control.*?>
    <?import java.lang.*?>
    <?import javafx.scene.layout.*?>
    <?import javafx.scene.layout.BorderPane?>

    <BorderPane xmlns="http://javafx.com/javafx/8" xmlns:fx="http://javafx.com/fxml/1"
        fx:controller="application.EditorController">
        <top>
            <MenuBar BorderPane.alignment="CENTER">
                <menus>
                    <Menu mnemonicParsing="false" text="File">
                        <items>
                            <MenuItem mnemonicParsing="false" text="Close" />
                        </items>
                    </Menu>
                    <Menu mnemonicParsing="false" text="Edit">
                        <items>
                            <MenuItem mnemonicParsing="false" text="Delete" />
                        </items>
                    </Menu>
                    <Menu mnemonicParsing="false" text="Help">
                        <items>
                            <MenuItem mnemonicParsing="false" text="About" />
                        </items>
                    </Menu>
                </menus>
            </MenuBar>
        </top>
        <center>
            <TextArea fx:id="mainTextArea" prefHeight="400.0" prefWidth="500.0"
                BorderPane.alignment="CENTER" />
        </center>
        <bottom>
            <HBox alignment="CENTER_LEFT" BorderPane.alignment="CENTER">
                <children>
                    <Label fx:id="lettersCountLabel" text="Label" />
                    <Separator orientation="VERTICAL" />
                    <Label fx:id="wordsCountLabel" text="Label" />
                    <Separator orientation="VERTICAL" />
                    <Button fx:id="clearButton" mnemonicParsing="false" text="Clear" />
                </children>
            </HBox>
        </bottom>
    </BorderPane>

Przycisk dodaliśmy w dolnej części aplikacji ustawiając na nim domyślny tekst "Clear" oraz nadając fx:id *clearButton* - posłuży on nam do wyczyszczenia zawartości pola tekstowego.

Musimy także zdefiniować zmienną odpowiadającą fx:id przycisku w klasie kontrolera, abyśmy mogli dodać do niego obsługę zdarzenia. Robimy to używając adnotacji @FXML i nazwy zmiennej zgodnej z fx:id.

*plik EditorController.java*

.. code-block:: java
    :linenos:

    package application;

    import java.net.URL;
    import java.util.ResourceBundle;

    import javafx.fxml.FXML;
    import javafx.fxml.Initializable;
    import javafx.scene.control.Button;
    import javafx.scene.control.Label;
    import javafx.scene.control.TextArea;

    public class EditorController implements Initializable {

        @FXML
        private TextArea mainTextArea;

        @FXML
        private Label wordsCountLabel;

        @FXML
        private Label lettersCountLabel;
        
        @FXML
        private Button clearButton;

        @Override
        public void initialize(URL arg0, ResourceBundle arg1) {
            String mainText = "To jest długi tekst,\n"
                    + "który zostanie wyświetlony\n"
                    + "w głównym oknie aplikacji";
            String letters = "Ilość liter: 50";
            String words = "Ilość słów: 12";
            
            mainTextArea.setText(mainText);
            lettersCountLabel.setText(letters);
            wordsCountLabel.setText(words);
        }
    }

Jak widzisz wszystko robimy analogicznie jak w poprzedniej lekcji, czyli:

#. Dodajemy kontrolkę do pliku fxml w Scene Builderze.
#. Nadajemy jej odpowiednie fx:id.
#. Tworzymy zmienną odpowiadającą fx:id w klasie kontrolera oznaczając ją jednocześnie adnotacją @FXML.


Obsługa zdarzenia przycisku
---------------------------
Obsługa zdarzenia przycisku jest najłatwiejszą z tych, które poznamy. Jest to czynność tak często powtarzana, że przycisk (klasa Button) posiada specjalną metodę dedykowaną do tego celu o nazwie **setOnAction()**. Podpięcie akcji przycisku najlepiej jest zrealizować w metodzie initialize(), która będzie wywołana przy uruchamianiu aplikacji.

*plik EditorController.java (metoda initialize())*

.. code-block:: java
    :linenos:

    @Override
    public void initialize(URL arg0, ResourceBundle arg1) {
        //reszta kodu bez zmian
        clearButton.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent arg0) {
                //kod obsługi zdarzenia
                mainTextArea.setText("");
            }
        });
    }

Jak widzisz na przycisku wywołaliśmy metodę **setOnAction()**, w której utworzyliśmy obiekt **EventHandler** z parametrem **ActionEvent**. Problem polega na tym, że jeśli pzyjrzysz się temu kodowi bliżej, to zauważysz, że występuje tutaj dziwne zagnieżdżenie nawiasów, a dodatkowo gdzieś podczas tworzenia obiektu implementujemy jeszcze metodę **handle()**. Jest to spowodowane tym, że EventHandler jest tak naprawdę interfejsem, a to oznacza, że nie możemy utworzyć obiektu na jego podstawie. Rozwiązaniem jest utworzenie **anonimowej klasy wewnętrznej**, czyli takiej klasy, która implementuje interfejs EventHandler i nie posiada swojej nazwy. ActionEvent jest typem zdarzenia jakie chcemy obsłużyć - w tym przypadku odpowiada ono wciśnięciu przycisku.

W metodzie *handle()* możemy zamieścić kod obsługi zdarzenia, czyli wywołać odpowiednie metody, ustawić tekst w innych kontrolkach itp. W naszym przypadku przycisk ma wyczyścić pole tekstowe - wywołujemy więc metodę *mainTextArea.setText("");* co oznacza ustawienie pustego Stringa jako jej zawartości.

**Uruchom aplikację i przetestuj działanie zaimplementowanego zdarzenia przycisku**.

**Porada**

Pisanie kodu klasy anonimowej byłoby dosyć męczące, szczególnie biorą pod uwagę nagromadzenie nawiasów i zagnieżdżeń kodu. Na szczęście większość kodu można wygenerować półautomatycznie.

Krok 1 - wpisz po prostu "clearButton.setOna..." i Ctrl+Spacja

.. image:: 07_fx2/step1.png
    :align: center

Krok 2 - zacznij wpisywać "new EventHandler" i Ctrl+Spacja

.. image:: 07_fx2/step2.png
    :align: center

Krok 3 - wciśnij Ctrl+1 i wybierz opcję "add unimplemented methods"

.. image:: 07_fx2/step3.png
    :align: center


Obsługa zdarzeń klawiatury i myszy
-----------------------------------
Obsługa zdarzeń przycisków jest prosta - wystarczy zapamiętać jedną metodę *setOnAction()* oraz sposób tworzenia klasy anonimowej - praktycznie nic się tam nie zmienia. Jeżeli jednak chcemy obsłużyć zdarzenia klawiatury, czy myszy (wciskanie przycisków, ruch kursora) musimy skorzystać z innej metody. W naszym edytorze tekstu chcemy mieć funkcjonalność zliczania znaków oraz słów. W tym celu zdarzenie należy podpiąć pod obiekt typu **TextArea** jednak tym razem posłużymy się metodą **addEventFilter()**.

*plik EditorController.java (metoda initialize())*

.. code-block:: java
    :linenos:

    @Override
    public void initialize(URL arg0, ResourceBundle arg1) {
        
        final String lettersCount = "Ilość znaków: ";
        final String wordsCount = "Ilość słów: ";
        
        //domyślne etykiety ustawione na 0
        lettersCountLabel.setText(lettersCount + 0);
        wordsCountLabel.setText(wordsCount + 0);
        
        clearButton.setOnAction(new EventHandler<ActionEvent>() {
            @Override
            public void handle(ActionEvent arg0) {
                //kod obsługi zdarzenia
                mainTextArea.setText("");
                
                //po wciśnięciu przycisku zmieniamy tekst etykiet na domyślny
                lettersCountLabel.setText(lettersCount + 0);
                wordsCountLabel.setText(wordsCount + 0);
            }
        });
        
        mainTextArea.addEventFilter(KeyEvent.KEY_TYPED, new EventHandler<KeyEvent>() {
            @Override
            public void handle(KeyEvent event) {
                //licznik znaków z pominięciem spacji
                int letters = mainTextArea.getText().trim().length();
                lettersCountLabel.setText(lettersCount + letters);
                
                //licznik słów
                int words = mainTextArea.getText().split(" ").length;
                wordsCountLabel.setText(wordsCount + words);
            }
        });
    }

W wierszu 23 dodaliśmy zdarzenie do obiektu mainTextArea. Metodzie addEventFilter należy przekazać dwa argumenty:

* typ zdarzenia jakie chcemy obsłużyć - u nas jest to KeyEvent.KEY_TYPED co odpowiada wciśnięciu i zwolnieniu dowolnego klawisza klawiatury
* obiekt EventHandler (podobnie jak w metodzie setOnAction), jednak z parametrem generycznym zgodnym z typem zdarzenia - w naszym przypadku typem tym jest KeyEvent.

W wierszach 26-32 definiujemy co ma się stać po wciśnięciu dowolnego klawisza. Liczbę znaków zliczamy usuwając najpierw białe znaki metodą *trim()* z klasy String, a następnie pobierając długość takiego napisu metodą *length()*. Ilość słów zliczamy podobnie, jednak najpierw pobrany tekst dzielimy na jednowymiarową tablicę słów, które były rozdzielone znakiem spacji " ". Właściwość length tablicy jest więc równa ilości słów w naszym tekście.

Teksty etykiet (Label) podobnie jak to było z obiektem TextArea ustawiamy za pomocą metody *setText()*.

Dostępne zdarzenia klawiatury, które możemy obsługiwać to:

* KeyEvent.KEY_PRESSED - wciśnięcie klawisza
* KeyEvent.KEY_RELEASED - zwolnienie klawisza
* KeyEvent.KEY_TYPED - wciśnięcie i zwolnienie klawisza

Jeżeli chcielibyśmy obsłużyć zdarzenia myszy, zrobimy to bardzo podobnie, jednak wykorzystamy klasę MouseEvent. Wybrane typy zdarzeń:

* MouseEvent.MOUSE_CLICKED - wciśnięcie i zwolnienie przycisku myszy
* MouseEvent.MOUSE_ENTERED - najechanie kursorem nad daną kontrolkę, do którego podpięte jest zdarzenie
* MouseEvent.MOUSE_DRAGGED - przeciągnięcie myszy z wciśniętym przyciskiem myszy

.. note::

Metodę *addEventFilter()* można wywołać na niemal dowolnym obiekcie JavyFX, więc w identyczny sposób można obsługiwać zdarzenia pól tekstowych, etykie, buttonów, czy też menu.


Ćwiczenie
----------
Dodaj do programu następujące opcje:

* Exit z menu File, któe powoduje zamknięcie aplikacji. Aplikację JavyFX można zakończyć wywołując metodę *Platform.exit()*
* Big Letters / Small Letters w menu Edit - opcje powodujące podmianę tekstu na wielkie lub małe litery

.. image:: 07_fx2/zad1.png
    :align: center

*plik Editor.fxml*

.. code-block:: xml
    :linenos:

    <?xml version="1.0" encoding="UTF-8"?>

    <?import javafx.scene.control.*?>
    <?import java.lang.*?>
    <?import javafx.scene.layout.*?>
    <?import javafx.scene.layout.BorderPane?>

    <BorderPane xmlns="http://javafx.com/javafx/8" xmlns:fx="http://javafx.com/fxml/1"
        fx:controller="application.EditorController">
        <top>
            <MenuBar BorderPane.alignment="CENTER">
                <menus>
                    <Menu mnemonicParsing="false" text="File">
                        <items>
                            <MenuItem fx:id="closeMenuItem" mnemonicParsing="false"
                                text="Close" />
                        </items>
                    </Menu>
                    <Menu mnemonicParsing="false" text="Edit">
                        <items>
                            <MenuItem fx:id="bigMenuItem" mnemonicParsing="false"
                                text="BIG LETTERS" />
                            <MenuItem fx:id="smallMenuItem" mnemonicParsing="false"
                                text="small letters" />
                        </items>
                    </Menu>
                </menus>
            </MenuBar>
        </top>
        <center>
            <TextArea fx:id="mainTextArea" prefHeight="400.0" prefWidth="500.0"
                BorderPane.alignment="CENTER" />
        </center>
        <bottom>
            <HBox alignment="CENTER_LEFT" BorderPane.alignment="CENTER">
                <children>
                    <Label fx:id="lettersCountLabel" text="Label" />
                    <Separator orientation="VERTICAL" />
                    <Label fx:id="wordsCountLabel" text="Label" />
                    <Separator orientation="VERTICAL" />
                    <Button fx:id="clearButton" mnemonicParsing="false" text="Clear" />
                </children>
            </HBox>
        </bottom>
    </BorderPane>

*plik EditorCntroller.java*

.. code-block:: java
    :linenos:

    package application;

    import java.net.URL;
    import java.util.ResourceBundle;

    import javafx.application.Platform;
    import javafx.event.ActionEvent;
    import javafx.event.EventHandler;
    import javafx.fxml.FXML;
    import javafx.fxml.Initializable;
    import javafx.scene.control.Button;
    import javafx.scene.control.Label;
    import javafx.scene.control.MenuItem;
    import javafx.scene.control.TextArea;
    import javafx.scene.input.KeyEvent;

    public class EditorController implements Initializable {
        
        @FXML
        private TextArea mainTextArea;

        @FXML
        private Label wordsCountLabel;

        @FXML
        private Button clearButton;

        @FXML
        private Label lettersCountLabel;

        @FXML
        private MenuItem closeMenuItem;

        @FXML
        private MenuItem bigMenuItem;
        
        @FXML
        private MenuItem smallMenuItem;

        @Override
        public void initialize(URL arg0, ResourceBundle arg1) {
            
            final String lettersCount = "Ilość znaków: ";
            final String wordsCount = "Ilość słów: ";
            
            //domyślne etykiety ustawione na 0
            lettersCountLabel.setText(lettersCount + 0);
            wordsCountLabel.setText(wordsCount + 0);
            
            clearButton.setOnAction(new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent arg0) {
                    //kod obsługi zdarzenia
                    mainTextArea.setText("");
                    
                    //po wciśnięciu przycisku zmieniamy tekst etykiet na domyślny
                    lettersCountLabel.setText(lettersCount + 0);
                    wordsCountLabel.setText(wordsCount + 0);
                    
                }
            });
            
            mainTextArea.addEventFilter(KeyEvent.KEY_TYPED, new EventHandler<KeyEvent>() {
                @Override
                public void handle(KeyEvent event) {
                    //licznik znaków z pominięciem spacji
                    int letters = mainTextArea.getText().trim().length();
                    lettersCountLabel.setText(lettersCount + letters);
                    
                    //licznik słów
                    int words = mainTextArea.getText().split(" ").length;
                    wordsCountLabel.setText(wordsCount + words);
                }
            });
            
            closeMenuItem.addEventHandler(ActionEvent.ACTION, new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    //zamknięcie aplikacji
                    Platform.exit();
                }
            });
            
            bigMenuItem.addEventHandler(ActionEvent.ACTION, new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    String text = mainTextArea.getText();
                    text = text.toUpperCase();
                    mainTextArea.setText(text);
                }
            });
            
            smallMenuItem.addEventHandler(ActionEvent.ACTION, new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    String text = mainTextArea.getText();
                    text = text.toLowerCase();
                    mainTextArea.setText(text);
                }
            });
            
        }
    }









