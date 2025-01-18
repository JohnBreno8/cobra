1. Configuração da Tela

stdscr = curses.initscr()
curses.curs_set(0)
sh, sw = stdscr.getmaxyx()
w = curses.newwin(sh, sw, 0, 0)
w.keypad(1)
w.timeout(100)

curses.initscr(): Inicializa a tela para usar a biblioteca curses.

curses.curs_set(0): Esconde o cursor para melhorar a visualização.

stdscr.getmaxyx(): Pega o tamanho da tela (linhas sh e colunas sw).

curses.newwin(sh, sw, 0, 0): Cria uma nova janela com o tamanho da tela.

w.keypad(1): Ativa a captura de teclas especiais, como as setas.

w.timeout(100): Define o tempo de espera em 100 ms para a próxima entrada do jogador, criando um delay para o movimento da cobra.



---

2. Inicialização da Cobrinha e da Comida

snk_x = sw // 4
snk_y = sh // 2
snake = [
    [snk_y, snk_x],
    [snk_y, snk_x - 1],
    [snk_y, snk_x - 2]
]
food = [sh // 2, sw // 2]
w.addch(int(food[0]), int(food[1]), curses.ACS_PI)

Posição inicial da cobra:

A cobra começa como uma lista de 3 blocos alinhados horizontalmente (snake).

snk_x e snk_y definem a posição inicial da cabeça da cobra.


Comida:

A comida começa no meio da tela.

curses.ACS_PI: Desenha o símbolo π como representação da comida.




---

3. Movimentação

next_key = w.getch()
key = key if next_key == -1 else next_key

w.getch(): Lê a tecla pressionada pelo jogador.

key: Define a direção atual da cobra com base na última tecla pressionada. Se nenhuma tecla for pressionada (-1), mantém a direção atual.



---

4. Atualização da Cabeça da Cobra

new_head = [snake[0][0], snake[0][1]]

if key == curses.KEY_DOWN:
    new_head[0] += 1
if key == curses.KEY_UP:
    new_head[0] -= 1
if key == curses.KEY_LEFT:
    new_head[1] -= 1
if key == curses.KEY_RIGHT:
    new_head[1] += 1

new_head: Calcula a nova posição da cabeça da cobra.

Dependendo da tecla pressionada, a posição (linha ou coluna) é alterada para cima, baixo, esquerda ou direita.



---

5. Colisão

if (new_head[0] in [0, sh] or
        new_head[1] in [0, sw] or
        new_head in snake):
    curses.endwin()
    quit()

Verifica colisões:

Bordas da tela: Se a cabeça da cobra ultrapassar os limites (0, sh, sw), o jogo termina.

Corpo da cobra: Se a nova cabeça da cobra for igual a qualquer parte do corpo, também termina o jogo.


curses.endwin(): Finaliza o modo curses e limpa a tela antes de sair.



---

6. Crescimento e Alimentação

if snake[0] == food:
    food = None
    while food is None:
        nf = [
            random.randint(1, sh-1),
            random.randint(1, sw-1)
        ]
        food = nf if nf not in snake else None
    w.addch(int(food[0]), int(food[1]), curses.ACS_PI)
else:
    tail = snake.pop()
    w.addch(int(tail[0]), int(tail[1]), ' ')

Comida ingerida:

Se a cabeça da cobra atinge a posição da comida, a cobra cresce (não remove o último bloco do corpo) e uma nova comida é gerada.

random.randint(1, sh-1): Gera uma posição aleatória para a comida dentro dos limites da tela, sem sobrepor o corpo da cobra.


Movimento normal:

Caso a comida não seja ingerida, o último bloco da cobra (tail) é removido para que o tamanho não aumente.




---

7. Renderização da Cobra

w.addch(int(snake[0][0]), int(snake[0][1]), curses.ACS_CKBOARD)

Desenha a nova cabeça da cobra na posição calculada.

curses.ACS_CKBOARD: Representa o corpo da cobra com um símbolo visual.



---

Funcionamento Geral

1. A cobra se move continuamente na direção da última tecla pressionada.


2. Se a cobra comer a comida, ela cresce e a comida aparece em outra posição aleatória.


3. O jogo termina se:

A cobra colidir com as bordas da tela.

A cobra colidir com ela mesma.




Como executar no Termux

1. Salve o código em um arquivo, por exemplo, snake.py.


2. Certifique-se de que o Python e o módulo curses estão instalados.


3. Execute com:

python3 snake.py



