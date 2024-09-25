# DefenseTowerPy

### Índice

1. [Introdução](#introdução)
2. [Como Rodar](#como-rodar)
3. [Estrutura do Projeto](#estrutura-do-projeto)
   - [main.py](#1-mainpy)
   - [button.py](#2-buttonpy)
   - [enemy.py](#3-enemypy)
   - [enemy_data.py](#4-enemy_datapy)
   - [turret.py](#5-turretpy)
   - [turret_data.py](#6-turret_datapy)
   - [world.py](#7-worldpy)
   - [constants.py](#8-constantspy)
4. [Contribuidores](#contribuidores)
5. [Licença](#licença)


---

### Introdução

Este projeto é um jogo de *Tower Defense* desenvolvido em Python utilizando a biblioteca Pygame. O objetivo do jogador é defender seu território, posicionando torres para eliminar inimigos que seguem um caminho predefinido. O jogo possui vários tipos de inimigos e torres, com diferentes níveis de dificuldade conforme o progresso.

---

### Como Rodar

1. Clone o repositório do projeto:
    ```bash
    git clone (https://github.com/EuFontoura/TetrisPy.git)
    ```
2. Dê um duplo clique no arquivo executável, localizado no diretório `dist/main.exe`, para iniciar o jogo.
   
Alternativamente, se preferir rodar via código fonte:

1. Certifique-se de ter o Python instalado em sua máquina, assim como a biblioteca Pygame (`pip install pygame`).
2. Clone o repositório do projeto:
    ```bash
    git clone (https://github.com/EuFontoura/TetrisPy.git)
    ```
3. Execute o arquivo `main.py` usando o seguinte comando no terminal:

```bash
python main.py
```

---

## **Overview**

Este jogo é um Tower Defense onde o jogador constrói torres para defender um caminho contra inimigos que tentam atravessá-lo. O jogo foi desenvolvido usando a biblioteca Pygame, com diferentes classes e módulos para organizar as funcionalidades, como gerenciamento de inimigos, torres, interface de usuário (botões) e mundo (mapa).

### Estrutura do Projeto

### **1. `main.py`**

O módulo `main.py` é o ponto de entrada do jogo. Ele inicializa o Pygame, configura a janela, gerencia o loop principal do jogo e lida com eventos de interação do usuário, como cliques em botões e movimentação de inimigos. Além disso, é responsável por integrar todos os outros módulos, como inimigos, torres, mundo e interface de usuário.

#### **Fluxo Geral do `main.py`**

1. **Inicialização do Pygame**: O jogo é iniciado com a inicialização do Pygame e a configuração da janela de jogo.
2. **Carregamento de Recursos**: São carregadas as imagens, sons e outros recursos necessários, como sprites de inimigos e torres, mapas e botões.
3. **Instanciação de Objetos**: Os objetos principais do jogo são criados, como as torres, inimigos, o mapa (`World`) e os botões.
4. **Loop Principal do Jogo**: O jogo roda dentro de um loop contínuo (game loop), onde o estado é atualizado a cada frame (FPS) e a interface é desenhada na tela.
5. **Eventos e Interações**: O jogo responde a eventos como cliques em botões, construção de torres e movimentação de inimigos.
6. **Condições de Vitória ou Derrota**: O loop verifica constantemente se o jogador venceu ou perdeu o nível, atualizando o estado do jogo conforme necessário.

#### **Funções e Lógica do `main.py`**

- **Inicialização e Configurações**:
  - `pygame.init()`: Inicializa os módulos necessários do Pygame.
  - `screen`: A janela principal onde o jogo é renderizado.
  - `clock`: Controla a taxa de quadros por segundo (FPS).

- **Carregamento de Imagens e Sons**:
  - Imagens de botões, torres e inimigos são carregadas usando `pygame.image.load()` e convertidas para otimizar a renderização.
  - Sons de efeitos e música de fundo são carregados e tocados com `pygame.mixer`.

- **Objetos Principais**:
  - **Botões**: São instanciados botões para ações como iniciar o jogo, comprar torres, entre outros.
  - **Mundo**: O mapa do jogo é carregado e processado usando a classe `World` a partir de um arquivo de dados.
  - **Inimigos**: Inimigos são gerados e adicionados à lista de inimigos controlados pelo `World`.

- **Loop Principal do Jogo**:
  - No loop, são realizadas as seguintes ações:
    - **Controle de FPS**: `clock.tick(FPS)` garante que o jogo rode na velocidade definida pelo FPS.
    - **Eventos do Usuário**: `pygame.event.get()` captura eventos, como cliques do mouse e teclas pressionadas.
      - **Construção de Torres**: Quando o jogador clica em uma posição válida, uma torre é criada e posicionada no mapa.
      - **Interação com Botões**: Os botões são verificados para ver se foram clicados e realizam suas ações correspondentes.
    - **Atualização do Jogo**: A lógica do jogo é atualizada, como o movimento dos inimigos, a rotação e disparo das torres, e a verificação de vitória/derrota.
    - **Renderização do Jogo**: O mapa, inimigos, torres e interface são desenhados a cada frame.

- **Encerramento do Jogo**:
  - O jogo termina quando o jogador fecha a janela ou quando as condições de vitória ou derrota são atendidas. Isso é capturado com um evento `pygame.QUIT`, que finaliza o loop e encerra o Pygame com `pygame.quit()`.

---

### **Estrutura do Loop Principal**

Aqui está uma visão geral simplificada do loop principal no `main.py`:

```python
# Loop principal do jogo
running = True
while running:
    # Controle do FPS
    clock.tick(FPS)

    # Processa eventos
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        # Detecta cliques nos botões e torres
        if event.type == pygame.MOUSEBUTTONDOWN:
            pos = pygame.mouse.get_pos()
            # Lógica para construção de torres e clique nos botões

    # Atualiza o estado do jogo (inimigos, torres, etc.)
    world.update()

    # Verifica se o nível foi concluído ou se o jogador perdeu
    if world.check_level_complete():
        world.reset_level()

    # Desenha tudo na tela
    screen.fill((0, 0, 0))  # Limpa a tela
    world.draw(screen)
    # Desenha outros elementos como botões e torres

    # Atualiza a tela
    pygame.display.update()

# Encerramento do Pygame
pygame.quit()
```

### **2. `button.py`**

Este módulo é responsável por criar e gerenciar botões interativos na interface do jogo, que são usados para diversas interações, como construir torres ou iniciar o jogo.

#### **Classe `Button`**
Representa um botão clicável no jogo.

- **Atributos:**
  - `image`: A imagem que representa o botão.
  - `rect`: O retângulo que delimita a área clicável do botão, com base na imagem.
  - `clicked`: Booleano que verifica se o botão foi clicado.
  - `single_click`: Booleano que indica se o botão deve ser clicado apenas uma vez.

- **Métodos:**
  - `__init__(self, x, y, image, single_click)`: Inicializa o botão com uma imagem, posição (x, y) e define se é um botão de clique único.
  - `draw(self, surface)`: Desenha o botão na tela e verifica se ele foi clicado. Retorna `True` se houver uma ação de clique.

---

### **3. `enemy.py`**

Este módulo gerencia os inimigos que seguem o caminho do mapa, controlando suas posições, movimentação e status.

#### **Classe `Enemy`**
Representa um inimigo no jogo.

- **Atributos:**
  - `waypoints`: Lista de pontos que o inimigo seguirá no mapa.
  - `pos`: A posição atual do inimigo.
  - `target_waypoint`: O próximo ponto de destino do inimigo.
  - `health`: A quantidade de vida do inimigo.
  - `speed`: A velocidade do inimigo.
  - `angle`: O ângulo de rotação do inimigo.
  - `image`: A imagem atual do inimigo (após possíveis rotações).
  - `rect`: A área de colisão do inimigo.

- **Métodos:**
  - `__init__(self, enemy_type, waypoints, images)`: Inicializa o inimigo com o tipo, waypoints e imagens.
  - `update(self, world)`: Atualiza a posição e rotação do inimigo, verifica se está vivo.
  - `move(self, world)`: Move o inimigo de acordo com seus waypoints e calcula a distância.
  - `rotate(self)`: Ajusta a rotação do inimigo com base na direção do movimento.
  - `check_alive(self, world)`: Verifica se o inimigo ainda está vivo. Se não, incrementa o número de inimigos mortos e adiciona dinheiro ao jogador.

---

### **4. `enemy_data.py`**

Este módulo contém os dados e configurações dos inimigos, incluindo suas quantidades por nível e atributos como vida e velocidade.

- **Dicionário `ENEMY_SPAWN_DATA`**: Define a quantidade de inimigos de cada tipo (weak, medium, strong, elite) que aparecerá em cada nível.

- **Dicionário `ENEMY_DATA`**: Contém as configurações de cada tipo de inimigo:
  - `weak`: Vida = 10, Velocidade = 2
  - `medium`: Vida = 15, Velocidade = 3
  - `strong`: Vida = 20, Velocidade = 4
  - `elite`: Vida = 30, Velocidade = 6

---

### **5. `turret.py`**

Este módulo gerencia as torres que o jogador pode construir para defender o caminho. As torres podem atacar os inimigos, têm um alcance definido e podem ser atualizadas.

#### **Classe `Turret`**
Representa uma torre defensiva no jogo.

- **Atributos:**
  - `upgrade_level`: O nível de atualização da torre.
  - `range`: O alcance da torre.
  - `cooldown`: Tempo de espera entre os disparos.
  - `last_shot`: Tempo desde o último disparo.
  - `target`: O inimigo atualmente sendo atacado pela torre.
  - `x`, `y`: Posições da torre no mapa.
  - `animation_list`: Lista de animações para a torre em diferentes níveis.
  - `rect`: A área de colisão da torre.

- **Métodos:**
  - `__init__(self, sprite_sheets, tile_x, tile_y)`: Inicializa a torre com os sprites e a posição no mapa.
  - `load_images(self, sprite_sheet)`: Carrega as imagens da torre a partir do spritesheet.
  - `update(self, enemy_group)`: Atualiza o estado da torre e procura novos alvos.
  - `pick_target(self, enemy_group)`: Seleciona um inimigo para ser atacado, baseado na distância.
  - `play_animation(self)`: Executa a animação de disparo da torre.
  - `upgrade(self)`: Aumenta o nível da torre, melhorando o alcance e reduzindo o cooldown.
  - `draw(self, surface)`: Desenha a torre na tela e mostra o alcance quando selecionada.

---

### **6. `turret_data.py`**

Contém os dados de configuração das torres, como alcance e cooldown para cada nível de atualização.

- **Lista `TURRET_DATA`**: Define as propriedades das torres em diferentes níveis:
  - **Nível 1**: Alcance = 90, Cooldown = 1500 ms
  - **Nível 2**: Alcance = 110, Cooldown = 1200 ms
  - **Nível 3**: Alcance = 125, Cooldown = 1000 ms
  - **Nível 4**: Alcance = 150, Cooldown = 900 ms

---

### **7. `world.py`**

Este módulo gerencia o mapa do jogo, os waypoints (pontos de controle no caminho dos inimigos), os níveis, e a lógica de progressão do jogo.

#### **Classe `World`**
Controla o mapa, os inimigos e a progressão dos níveis.

- **Atributos:**
  - `level`: O nível atual do jogo.
  - `health`: A quantidade de vida restante do jogador.
  - `money`: Dinheiro do jogador para comprar torres e upgrades.
  - `tile_map`: O mapa de tiles (quadrados) do jogo.
  - `waypoints`: Lista de pontos pelos quais os inimigos se movem.
  - `enemy_list`: Lista de inimigos que serão gerados.
  - `spawned_enemies`: Contador de inimigos gerados.
  - `killed_enemies`: Contador de inimigos derrotados.
  - `missed_enemies`: Contador de inimigos que chegaram ao final do caminho.

- **Métodos:**
  - `__init__(self, data, map_image)`: Inicializa o mundo com os dados do mapa e imagem.
  - `process_data(self)`: Processa os dados do mapa e extrai os waypoints e layout.
  - `process_waypoints(self, data)`: Processa a lista de waypoints do mapa.
  - `process_enemies(self)`: Gera a lista de inimigos com base no nível atual.
  - `check_level_complete(self)`: Verifica se o nível foi concluído (todos os inimigos foram derrotados ou chegaram ao final).
  - `reset_level(self)`: Reseta o estado do nível (inimigos, contadores).
  - `draw(self, surface)`: Desenha o mapa na tela.

---

### **8. `constants.py`**

Contém as constantes globais usadas no jogo.

- `ROWS`: Número de linhas do mapa.
- `COLS`: Número de colunas do mapa.
- `TILE_SIZE`: Tamanho de cada tile.
- `SIDE_PANEL`: Largura da área lateral da interface.
- `SCREEN_WIDTH`: Largura total da tela.
- `SCREEN_HEIGHT`: Altura total da tela.
- `FPS`: Taxa de quadros por segundo.
- `HEALTH`: Vida inicial do jogador.
- `MONEY`: Dinheiro inicial do jogador.
- `SPAWN_COOLDOWN`: Tempo de espera entre o surgimento de inimigos.
- `TURRET_LEVELS`: Número máximo de níveis de atualização das torres.
- `BUY_COST`: Custo de compra de uma torre.
- `UPGRADE_COST`: Custo para atualizar uma torre.
- `KILL_REWARD`: Recompensa por matar um inimigo.
- `LEVEL_COMPLETE_REWARD`: Recompensa por completar um nível.

---

### Contribuidores

- Gabriel Fontoura

---

### Licença

Este projeto não possui uma licença formal.

--- 
