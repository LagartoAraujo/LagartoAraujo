#include <iostream>
#include <conio.h> // para _getch()

// Tamanho do labirinto
const int largura = 20;
const int altura = 10;

// Posição inicial do quadrado
int x = 1;
int y = 1;

// Direção inicial do quadrado
enum Direcao { DIREITA, BAIXO, ESQUERDA, CIMA };
Direcao direcaoAtual = DIREITA;

// Labirinto
char labirinto[altura][largura] = {
    "###################",
    "#                 #",
    "#                 #",
    "#                 #",
    "#                 #",
    "#        ####     #",
    "#        #        #",
    "#        #        #",
    "#        #        #",
    "###################"
};

// Função para exibir o labirinto e o quadrado
void exibirLabirinto() {
    system("cls"); // Limpa a tela
    for (int i = 0; i < altura; ++i) {
        for (int j = 0; j < largura; ++j) {
            if (i == y && j == x) // Desenhar o quadrado
                std::cout << "O";
            else
                std::cout << labirinto[i][j]; // Desenhar o labirinto
        }
        std::cout << std::endl;
    }
}

// Função para verificar se o próximo movimento é válido
bool movimentoValido(int novoX, int novoY) {
    if (novoX >= 0 && novoX < largura && novoY >= 0 && novoY < altura &&
        labirinto[novoY][novoX] != '#')
        return true;
    return false;
}

int main() {
    char tecla;
    bool gameOver = false;

    exibirLabirinto();

    // Loop principal do jogo
    while (!gameOver) {
        // Verificar se uma tecla foi pressionada
        if (_kbhit()) {
            tecla = _getch();
            if (tecla == ' ') { // Barra de espaço pressionada
                // Mudar a direção do quadrado para a direita
                switch (direcaoAtual) {
                    case DIREITA: direcaoAtual = BAIXO; break;
                    case BAIXO: direcaoAtual = ESQUERDA; break;
                    case ESQUERDA: direcaoAtual = CIMA; break;
                    case CIMA: direcaoAtual = DIREITA; break;
                }
            }
        }

        // Movimento do quadrado
        switch (direcaoAtual) {
            case DIREITA: x++; break;
            case BAIXO: y++; break;
            case ESQUERDA: x--; break;
            case CIMA: y--; break;
        }

        // Verificar colisão com as paredes
        if (!movimentoValido(x, y)) {
            // Movimento inválido, voltar à posição anterior
            switch (direcaoAtual) {
                case DIREITA: x--; break;
                case BAIXO: y--; break;
                case ESQUERDA: x++; break;
                case CIMA: y++; break;
            }
        }

        // Verificar se o quadrado alcançou a posição final
        if (x == largura - 1 && y == altura - 1) {
            gameOver = true;
            std::cout << "Parabéns, você chegou ao fim do labirinto!" << std::endl;
        }

        // Exibir o labirinto atualizado
        exibirLabirinto();

        // Delay para controle de velocidade
        // Você pode ajustar o valor conforme necessário
        for (int i = 0; i < 10000000; ++i);

    }

    return 0;
}
