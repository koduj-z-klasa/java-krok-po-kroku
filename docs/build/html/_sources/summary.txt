Praktyczna aplikacja
=============================

Jako podsumowanie naszej nauki Javy napiszemy prostą grę - kółki i krzyżyk wykorzystując wcześniej poznane elementy. Zaczniemy od zdefiniowania widoku, następnie utworzymy prosty model aplikacji a na końcu połączymy całość w klasie kontrolera.


Projekt
--------
Zacznijmy od utworzenia nowego projektu wykorzystującego FXML w eclipse. Jako główny layout aplikacji (ostatnia zakładka tworzenia projektu) wybierz VBox.

.. image:: 08_summary/project.png
    :align: center

* Main.java - główna klasa aplikacji
* Model.java - model danych
* TicTacToeController - klasa kontrolera
* TicTacToe.fxml - widok aplikacji
* application.css - definicja css dla widoku - pokażemy jak w prosty sposób wykorzystać style css do poprawy wyglądu aplikacji


Widok
-------
Otwórz plik fxml w Scene builderze i odtwórz poniższą hierarchię layoutów i kontrolek uwzględniając także odpowiednie atrybuty fx:id.

.. image:: 08_summary/sb_hierarchy.png
    :align: center

Jako nadrzędy layout wykorzystujemy VBox, ponieważ pozwala on ustawiać kontrolki i layouty w pojedynczej kolumnie. My potrzebujemy jedną kolumnę z dwoma wierszami - w jednym z nich umieszczamy podstawowe menu, a w drugim layout typu GridLayout.

W Menu dostępne będą trzy opcje:

* New Game (nowa gra) - w menu File
* Close (wyjście z programu) - w menu File
* About (o programie) - w menu Help

Layout znajdujący się w drugim wierszu naszego VBoxa pozwala na utworzenie siatki o dowolnym rozmiarze - my potrzebujemy oczywiście siatkę 3x3, w której następnie umieścimy Buttony. Kolejne kolumny do GridPane można dodać po jego wcześniejszym zaznaczeniu i wybraniu opcji Add Row lub Add Column.

.. image:: 08_summary/add_column.png
    :align: center

Problem jaki teraz się pojawia polega na tym, że przyciski są prostokątne, nie wykorzystują całej przestrzeni w GridPane a my chcielibyśmy, żeby były kwadratowe i nie było między nimi przerw.

.. image:: 08_summary/size1.png
    :align: center

W tym celu zaznacz wszystkie przyciski w sekcji Document Hierarchy (np. z wciśniętym Shiftem) i przejdź do sekcji Layout po prawej stronie Scene Buildera. Tam ustaw porządany rozmiar przycisku za wprowadzając opcję Pref Width oraz Pref Height (np. na 100 px).

.. image:: 08_summary/size2.png
    :align: center

Przyciski zajmują już całą przestrzeń, ale nadal nie mają ustalonego przez nas rozmiaru 100x100. Jest to spowodowane tym, że kolumny i wiersze w GridPane mają zdefiniowane różne rozmiary i mają one wyższy priorytet niż rozmiar przycisków. Kliknij w każdy wiersz i kolumnę oraz wiersz GridPane i ustaw ich rozmiar (Min i Pref Width/Height) na *USE_COMPUTED_SIZES*. W tej sytuacji wykorzystane zostaną wymiary węzłów potomnych.

.. image:: 08_summary/size3.png
    :align: center

Zaznacz ponownie wszystkie przyciski i przejdźmy jeszcze na chwilę do ich sekcji Properties, gdzie ustawimy większą czcionkę (będzie na nich wyświetlany tylko X lub 0).

.. image:: 08_summary/size4.png
    :align: center

Dla lepszego ostatecznego wyglądu dodajmy jeszcze linie siatki w celu wyraźniejszego oddzielenia przycisków, a także zmieńmy wygląd kursora:

.. image:: 08_summary/size5.png
    :align: center

Jeżeli nie zrobiłeś tego na etapie tworzenia projektu, to dodatkowo ustaw odpowiednią ścieżkę do klasy kontrolera, czyli application.TickTackToeController.

Ostatecznie nasz plik fxml powinien mieć następującą postać:

*plik TicTacToe.fxml*

.. code-block:: xml
    :linenos:

    <?xml version="1.0" encoding="UTF-8"?>

    <?import javafx.scene.text.*?>
    <?import javafx.scene.control.*?>
    <?import java.lang.*?>
    <?import javafx.scene.layout.*?>
    <?import javafx.scene.layout.GridPane?>

    <VBox xmlns="http://javafx.com/javafx/8" xmlns:fx="http://javafx.com/fxml/1"
        fx:controller="application.TickTackToeController">
        <children>
            <MenuBar>
                <menus>
                    <Menu mnemonicParsing="false" text="File">
                        <items>
                            <MenuItem fx:id="newGameMenu" mnemonicParsing="false"
                                text="New Game" />
                            <MenuItem fx:id="closeMenu" mnemonicParsing="false" text="Close" />
                        </items>
                    </Menu>
                    <Menu mnemonicParsing="false" text="Help">
                        <items>
                            <MenuItem fx:id="aboutMenu" mnemonicParsing="false" text="About" />
                        </items>
                    </Menu>
                </menus>
            </MenuBar>
            <GridPane gridLinesVisible="true">
                <columnConstraints>
                    <ColumnConstraints />
                    <ColumnConstraints />
                    <ColumnConstraints />
                </columnConstraints>
                <rowConstraints>
                    <RowConstraints />
                    <RowConstraints />
                    <RowConstraints />
                </rowConstraints>
                <children>
                    <Button fx:id="button00" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.hgrow="ALWAYS" GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button01" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.columnIndex="1" GridPane.hgrow="ALWAYS" GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button02" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.columnIndex="2" GridPane.hgrow="ALWAYS" GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button10" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.hgrow="ALWAYS" GridPane.rowIndex="1" GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button11" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.columnIndex="1" GridPane.hgrow="ALWAYS" GridPane.rowIndex="1"
                        GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button12" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.columnIndex="2" GridPane.hgrow="ALWAYS" GridPane.rowIndex="1"
                        GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button20" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.hgrow="ALWAYS" GridPane.rowIndex="2" GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button21" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.columnIndex="1" GridPane.hgrow="ALWAYS" GridPane.rowIndex="2"
                        GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                    <Button fx:id="button22" contentDisplay="CENTER"
                        mnemonicParsing="false" prefHeight="100.0" prefWidth="100.0" text="X"
                        GridPane.columnIndex="2" GridPane.hgrow="ALWAYS" GridPane.rowIndex="2"
                        GridPane.vgrow="ALWAYS">
                        <font>
                            <Font size="48.0" />
                        </font>
                    </Button>
                </children>
            </GridPane>
        </children>
    </VBox>


Model
----------
Klasa modelu w naszym przypadku jest bardzo prosta. Jedynym jej zadaniem będzie przechowywanie aktualnego stanu aplikacji, a to najłatwiej będzie zrealizować za pomocą dwuwymiarowej tablicy. Możemy wykorzystać dowolny typ liczbowy, np. int ponieważ potrzebujemy 3 stanów każdego przycisku:

* -1 - kółko
* 0 - puste pole
* 1 - krzyżyk

*plik Model.java*

.. code-block:: java
    :linenos:

    package application;

    public class Model {
        public static final int X = 1;
        public static final int O = -1;
        public static final int BLANK = 0;
        
        public static final int SIZE = 3;
        private int[][] table;
        private int activePlayer;
        
        public void setValue(int x, int y, int value) {
            table[x][y] = value;
        }
        
        public int getValue(int x, int y) {
            return table[x][y];
        }
        
        public int getActivePlayer() {
            return activePlayer;
        }
        
        public void switchPlayer() {
            activePlayer = -activePlayer;
        }

        /*
         * Konstruktor
         */
        public Model() {
            table = new int[SIZE][SIZE];
            activePlayer = O;
        }
        
        /*
         * Metoda sprawdzająca kto wygrał
         */
        public int getWinner() {
            int winner = BLANK;
            
            //sprawdzamy wiersze
            for(int row=0; row < SIZE; row++) {
                for(int col=1; col < SIZE; col++) {
                    if(table[row][col] !=  table[row][col-1]) {
                        //przerywamy sprawdzanie tego wiersza
                        break;
                    } else if(col == SIZE-1) {
                        winner = table[row][col];
                        return winner;
                    }
                }
            }
            
            //sprawdzamy kolumny
            for(int row=0; row < SIZE; row++) {
                for(int col=1; col < SIZE; col++) {
                    if(table[col][row] !=  table[col-1][row]) {
                        //przerywamy sprawdzanie tej kolumny
                        break;
                    } else if(col == SIZE-1) {
                        winner = table[col][row];
                        return winner;
                    }
                }
            }
            
            //sprawdzamy pierwszą przekątną
            for(int i=1; i<SIZE; i++) {
                if(table[i][i] != table[i-1][i-1]) {
                    break;
                } else if(i == SIZE-1) {
                    winner = table[i][i];
                    return winner;
                }
            }
            
            //sprawdzamy drugą przekątną
            for(int i=0; i < SIZE-1; i++) {
                if(table[i][SIZE-1 - i] != table[i+1][SIZE-2 - i]) {
                    break;
                } else if(i == SIZE-2) {
                    winner = table[i][i];
                    return winner;
                }
            }
            
            return winner;
        }
    }

Kontroler
-------------

*plik TicTacToeController.java*

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
    import javafx.scene.control.MenuItem;
    import javafx.scene.control.TextArea;
    import javafx.scene.input.MouseEvent;
    import javafx.stage.Popup;

    public class TicTacToeController implements Initializable {

        @FXML
        private MenuItem newGameMenu;

        @FXML
        private Button button02;

        @FXML
        private Button button10;

        @FXML
        private Button button21;

        @FXML
        private Button button20;

        @FXML
        private Button button01;

        @FXML
        private Button button12;

        @FXML
        private Button button00;

        @FXML
        private Button button11;

        @FXML
        private Button button22;

        @FXML
        private MenuItem closeMenu;

        @FXML
        private MenuItem aboutMenu;

        private Model model;
        private Button[][] buttons;

        @Override
        public void initialize(URL arg0, ResourceBundle arg1) {
            model = new Model();
            buttons = new Button[][] { { button00, button01, button02 },
                    { button10, button11, button12 },
                    { button20, button21, button22 } };
            addButtonAction();
            addMenuAction();
        }

        private void newGame() {
            model = new Model();
            for (int i = 0; i < buttons.length; i++) {
                for (int j = 0; j < buttons[i].length; j++) {
                    buttons[i][j].setText("");
                }
            }
        }

        private void addButtonAction() {
            for (int i = 0; i < buttons.length; i++) {
                for (int j = 0; j < buttons[i].length; j++) {
                    final int x = i, y = j;
                    buttons[i][j].setOnAction(new EventHandler<ActionEvent>() {

                        @Override
                        public void handle(ActionEvent event) {
                            String buttonText = buttons[x][y].getText();
                            if ("".equals(buttonText)) {
                                model.setValue(x, y, model.getActivePlayer());
                                updateView();
                                model.switchPlayer();
                            }

                            if (model.getWinner() != Model.BLANK) {
                                showWinnerPopup();
                            }
                        }
                    });
                }
            }
        }

        private void addMenuAction() {
            newGameMenu.setOnAction(new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    newGame();
                }
            });

            closeMenu.setOnAction(new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    Platform.exit();
                }
            });

            aboutMenu.setOnAction(new EventHandler<ActionEvent>() {
                @Override
                public void handle(ActionEvent event) {
                    String aboutText = "Gra w kółko i krzyżyk\n"
                            + "Koduj z klasą 2014";
                    createPopup(aboutText);
                }
            });
        }

        /**
         * Aktualizujemy wygląd przycisków
         */
        private void updateView() {
            for (int i = 0; i < Model.SIZE; i++) {
                for (int j = 0; j < Model.SIZE; j++) {
                    if (model.getValue(i, j) == Model.X) {
                        buttons[i][j].setText("X");
                    } else if (model.getValue(i, j) == Model.O) {
                        buttons[i][j].setText("O");
                    }
                }
            }
        }

        private void showWinnerPopup() {
            String winner = null;
            if (model.getWinner() == Model.X) {
                winner = "X";
            } else if (model.getWinner() == Model.O) {
                winner = "O";
            }

            String winnerText = "Wygrywa: " + winner;

            Popup popup = createPopup(winnerText);
            popup.addEventFilter(MouseEvent.MOUSE_CLICKED,
                    new EventHandler<MouseEvent>() {
                        @Override
                        public void handle(MouseEvent event) {
                            newGame();
                        }
                    });
        }

        private Popup createPopup(String text) {
            TextArea popupText = new TextArea(text);
            popupText.setPrefWidth(200);
            popupText.setPrefHeight(100);
            popupText.setEditable(false);

            Popup popup = new Popup();
            popup.setAutoFix(true);
            popup.getContent().addAll(popupText);

            popup.show(button00.getScene().getWindow());
            popup.addEventFilter(MouseEvent.MOUSE_CLICKED,
                    new EventHandler<MouseEvent>() {
                        @Override
                        public void handle(MouseEvent event) {
                            newGame();
                            popup.hide();
                        }
                    });

            return popup;
        }
    }

Gotowa aplikacja
------------------

*plik Main.java*

.. code-block:: java
    :linenos:

    package application;

    import javafx.application.Application;
    import javafx.fxml.FXMLLoader;
    import javafx.scene.Scene;
    import javafx.scene.layout.VBox;
    import javafx.stage.Stage;

    public class Main extends Application {
        @Override
        public void start(Stage primaryStage) {
            try {
                VBox root = (VBox) FXMLLoader.load(getClass().getResource(
                        "TicTacToe.fxml"));
                Scene scene = new Scene(root);
                scene.getStylesheets().add(
                        getClass().getResource("application.css").toExternalForm());
                primaryStage.setTitle("Tic-Tac-Toe");
                primaryStage.setScene(scene);
                primaryStage.show();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        public static void main(String[] args) {
            launch(args);
        }
    }










