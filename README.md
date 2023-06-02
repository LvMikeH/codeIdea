# codeIdea

import java.util.Scanner;

import static java.lang.Math.abs;

public class Main {
    public static int factorial = 3;
    public static void main(String[] args) {
        // write your code here

        char[] grid = {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '};
        printGameState(grid);
        int column = 0;
        int row = 0;
        int t = 0;
        while (t < 9) {
            Scanner scanner = new Scanner(System.in);
            try {
                row = scanner.nextInt();
                column = scanner.nextInt();
            } catch (Exception e) {
                System.out.println("You should enter numbers!");
                continue;
            }

            if (isFight(grid, column, row)) {
                if (t % 2 == 0) {
                    grid[3 * (row-1) + column - 1] = 'X';
                } else {
                    grid[3 * (row-1) + column - 1] = 'O';
                }
                printGameState(grid);
                t++;
                if (t > 4) {
                    if (gameStateAnalyze(grid).equals("X wins")) {
                        System.out.println("X wins");
                        t = 9;
                    } else if (gameStateAnalyze(grid).equals("O wins")) {
                        System.out.println("O wins");
                        t = 9;
                    } else {
                        System.out.println("Draw");
                    }
                }
            }
        }
    }

    public static boolean isFight (char[] grid, int column, int row){
        if (column > 3 || column < 1 || row >3 || row < 1) {
            System.out.println("Coordinates should be from 1 to 3!");
            return false;
        } else if(grid[3 * (row-1) + column - 1] != ' ') {
            System.out.println("This cell is occupied! Choose another one!");
            return false;
        } else {
            return true;
        }
    }

    public static void printGameState(char[] grid) {
        int index;
        System.out.println("---------");
        for(int i = 0; i < factorial; i++) {
            System.out.print("| ");
            for(int j = 0; j < factorial; j++) {
                index = i * factorial + j;
                System.out.print(grid[index] + " ");
            }
            System.out.print("|\n");
        }
        System.out.println("---------");
    }

    public static String gameStateAnalyze(char[] grid) {
        int o = 0;
        int x = 0;
        int none = 0;
        for (char ch: grid) {
            switch (ch) {
                case 'X' -> x++;
                case 'O' -> o++;
                case '_' -> none++;
                default -> {
                }
            }
        }
        if (abs(x-o)>1 ) {
            return "Impossible";
        }
        if (none == 0) {
            return switch (inLine(grid)) {
                case "XInLine" -> "X wins";
                case "OInLine" -> "O wins";
                case "NotInLine" -> "Draw";
                default -> "Impossible";
            };
        } else {
            return switch (inLine(grid)) {
                case "XInLine" -> "X wins";
                case "OInLine" -> "O wins";
                case "NotInLine" -> "Game not finished";
                default -> "Impossible";
            };
        }
    }

    public static String inLine(char[] gr) {
        int xInline = 0;
        int oInline = 0;
        if ((gr[4] == gr[3] && gr[4] == gr[5]) || (gr[4] == gr[1] && gr[4] == gr[7])) {
            if (gr[4] == 'X'){
                xInline++;
            } else if (gr[4] == 'O') {
                oInline++;
            }
        }
        if ((gr[4] == gr[0] && gr[4] == gr[8]) || (gr[4] == gr[2] && gr[4] == gr[6])) {
            if (gr[4] == 'X'){
                xInline++;
            } else if (gr[4] == 'O') {
                oInline++;
            }
        }
        if ((gr[0] == gr[1] && gr[0] == gr[2]) || (gr[0] == gr[3] && gr[0] == gr[6])) {
            if (gr[0] == 'X'){
                xInline++;
            } else if (gr[0] == 'O') {
                oInline++;
            }
        }
        if ((gr[8] == gr[7] && gr[8] == gr[2]) || (gr[8] == gr[5] && gr[8] == gr[2])) {
            if (gr[8] == 'X'){
                xInline++;
            } else if (gr[8] == 'O'){
                oInline++;
            }
        }

        if (xInline > 1 || oInline > 1 || (xInline == 1 && oInline == 1)) {
            return "error";
        } else if (xInline == 1) {
            return "XInLine";
        } else if (oInline == 1) {
            return "OInLine";
        } else {
            return "NotInLine";
        }
    }
}
