Jogo-da-Velha
=============

//Adaptado de: http://pontobyte.blogspot.com.br/2011/06/java-jogo-da-velha.html
//Alunos: Clayton Porto, Caio, Leomar
package jogodavelha;

import java.util.Random;
import java.util.Scanner;

public class JogoDaVelha {

    public static String[][] Tabuleiro = new String[3][3]; // TABULEIRO DO JOGO
    public static Integer Linha = 0;
    public static Integer Coluna = 0;
    public static Integer Valid = 0;
    public static int cont = 2; // CONTADOR DE JOGADAS, INICIA COM 2, SENDO QUE 2 JOGADAS FORAM FEITAS PELO USUÁRIO
    public static int x;
    public static int y;
    public static Random gerador = new Random();

    public static void MostraTabuleiro(String[][] x) {
        System.out.println("\n");
        for (int l = 0; l < x.length; l++) {
            for (int c = 0; c < x.length; c++) {
                System.out.print(x[l][c] + "\t");
            }
            System.out.println("\n");
        }
    }

    public static void MostraTabuleiro() { // MOSTRA UM EXEMPLO DO TABULEIRO A SER UTILIZADO
        System.out.println("\n--------------------------- EXEMPLO ---------------------------\n"
                + "[Linha 0][Coluna 0]" + " | [Linha 0][Coluna 1]" + " | [Linha 0][Coluna 2]\n\n"
                + "[Linha 1][Coluna 0]" + " | [Linha 1][Coluna 1]" + " | [Linha 1][Coluna 2]\n\n"
                + "[Linha 2][Coluna 0]" + " | [Linha 2][Coluna 1]" + " | [Linha 2][Coluna 2]\n\n");

    }

    public static String VerificaGanhador(String[][] x) {
        String[] Tab = new String[8];
        String Win = "null";
        if (cont == 9) {
            Win = "Velha";
        }
        Tab[0] = x[0][0] + x[0][1] + x[0][2];
        Tab[1] = x[1][0] + x[1][1] + x[1][2];
        Tab[2] = x[2][0] + x[2][1] + x[2][2];
        Tab[3] = x[0][0] + x[1][0] + x[2][0];
        Tab[4] = x[0][1] + x[1][1] + x[2][1];
        Tab[5] = x[0][2] + x[1][2] + x[2][2];
        Tab[6] = x[0][0] + x[1][1] + x[2][2];
        Tab[7] = x[0][2] + x[1][1] + x[2][0];
        for (int i = 0; i < Tab.length; i++) {
            if (Tab[i].equals("XXX")) {
                Win = "Player 1";
            } else if (Tab[i].equals("OOO")) {
                Win = "Player 2";
            }
        }
        return Win;
    }

    public static void proximaJogada() {
        x = gerador.nextInt(3); // GERA UM NÚMERO ALEATÓRIO QUE PERTENCE AO CONJUNTO {0,1,2} (CORRESPONDENTE AOS ÍNDICES DA MATRIZ)
        y = gerador.nextInt(3);
        if (Tabuleiro[x][y].equals("-")) { // COMPARA A POSSIÇÃO NA MATRIZ, COM OS NÚMEROS GERADOS ESTÁ VAZIA, CASO CONTRARIO O MÉTODO É CHAMADO NOVAMENTE
            if (cont % 2 == 0 && VerificaGanhador(Tabuleiro).equals("null")) { // VERIFICA SE A QUANTIDADE DE JOGADAS É PAR E SE O TABULEIRO AINDA ESTÁ SEM JOGADOR
                Tabuleiro[x][y] = "X"; // O CONTADOR SENDO UM NÚMERO PAR, EQUIVALE A "X" NO TABULEIRO
                cont++; // INCREMENTA A QUANTIDADE DE JOGADAS
                proximaJogada(); // CHAMA NOVAMENTE O MÉTODO 
            }
            if (cont % 2 != 0 && VerificaGanhador(Tabuleiro).equals("null")) {
                Tabuleiro[x][y] = "O"; // O CONTADOR SENDO UM NUMERO IMPAR, EQUIVALE A "O" NO TABULEIRO
                cont++; // INCREMENTA A JOGADA
                proximaJogada(); // E MAIS UMA VEZ CHAMA O MÉTODO
            }
        } else {
            proximaJogada();
        }
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        for (int i = 0; i < 3; i++) { // PREENCHE TODOS OS CAMPOS DO TABULEIRO COM "-"
            for (int x = 0; x < 3; x++) {
                Tabuleiro[i][x] = "-";
            }
        }
        
        MostraTabuleiro(); // MOSTRA UM EXEMPLO DE COMO É O TABULEIRO ULTILIZADO NO JOGO, CONTENDO AS LINHAS E COLUNAS

        do {
            System.out.println("Jogador 1. Informe a LINHA: ");
            Linha = in.nextInt(); // PERGUNTA A LINDA DA MATRIZ A SER UTILIZADA
            while (Linha < 0 || Linha > 2) { // VERIFICA SE O NUMERO DIGITADO É MENOR QUE 0 E MAIOR QUE 2, CASO SEJA, INFORMA QUE A LINHA É INVALIDA E PEDE PARA SER DIGITADO NOVAMENTE
                System.out.println("Esta linha é inválida!.");
                System.out.println("Jogador 1. Informe a LINHA: ");
                Linha = in.nextInt();
            }
            System.out.println("Jogador 1. Informe a COLUNA: ");
            Coluna = in.nextInt(); // PERGUNTA A COLUNA DA MATRIZ A SER UTILIZADA
            while (Coluna < 0 || Coluna > 2) // VERIFICA SE O NUMERO DIGITADO É MENOR QUE 0 E MAIOR QUE 2, CASO SEJA, INFORMA QUE A COLUNA É INVALIDA E PEDE PARA SER DIGITADO NOVAMENTE
            {
                System.out.println("Esta coluna é inválida!.");
                System.out.println("Jogador 1. Informe a COLUNA: ");
                Coluna = in.nextInt();
            }
            if (Tabuleiro[Linha][Coluna].equals("-")) // VERIFICA SE A LINHA E A COLUNA QUE FORAM DIGITADAS ESTÁ VAZIA, SE SIM, COLOCA "X" E A VARIÁVEL VÁLID RECEBE 1
            {
                Tabuleiro[Linha][Coluna] = "X";
                Valid = 1;
            } else // CASO NÃO ESTEJA VAZIO, A VARIAVÉL VALID RECEBE 1 E TODO O "DO WHILE" E EXECUTADO NOVAMENTE
            {
                Valid = 0;
                System.out.println("Jogada inválida. Tente novamente.");
            }

        } while (Valid == 0);
        
        Valid = 0;
        MostraTabuleiro(Tabuleiro); // MOSTRA O TABULEIRO APÓS A PRIMEIRA JOGADA
        VerificaGanhador(Tabuleiro); // VERIFICA SE EXISTE UM GANHADOR
        
        do {
            System.out.println("Jogador 2. Informe a LINHA: ");
            Linha = in.nextInt();
            while (Linha < 0 || Linha > 2) {
                System.out.println("Esta linha é inválida!.");
                System.out.println("Jogador 2. Informe a LINHA: ");
                Linha = in.nextInt();
            }
            System.out.println("Jogador 2. Informe a COLUNA: ");
            Coluna = in.nextInt();
            while (Coluna < 0 || Coluna > 2) {
                System.out.println("Esta coluna é inválida!.");
                System.out.println("Jogador 2. Informe a COLUNA: ");
                Coluna = in.nextInt();
            }
            if (Tabuleiro[Linha][Coluna].equals("-")) { // VERIFICA SE A LINHA E A COLUNA QUE FORAM DIGITADAS ESTÁ VAZIA, SE SIM, COLOCA "O" E A VARIÁVEL VÁLID RECEBE 1
                Tabuleiro[Linha][Coluna] = "O";
                Valid = 1;
            } else {
                Valid = 0;
                System.out.println("Jogada inválida. Tente novamente.");
            }

        } while (Valid == 0);
        
        proximaJogada(); // CHAMA O MÉTODO COM RECURSIVIDADE, NA QUAL FARÁ TODAS AS PRÓXIMAS JOGADAS
        MostraTabuleiro(Tabuleiro); // AO FINAL DO MÉTODO COM RECURSIVIDADE, É MOSTRADO O TABULEIRO, CONTENDO A JOGADA GERADA
        System.out.println(VerificaGanhador(Tabuleiro) + " Vence!"); // MOSTRA O VENCEDOR, SE É O JOGADOR 1, JOGADOR 2 OU SE DEU VELHA
    }
}
