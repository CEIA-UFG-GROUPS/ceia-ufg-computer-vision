# Geometria de Múltiplas Vistas (Stereo Vision)
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que a Visão Estéreo resolve
- Quando uma câmera captura uma foto, o mundo tridimensional (3D) é projetado em um plano bidimensional (2D). Toda a informação de profundidade (o eixo Z) é perdida nessa projeção matemática.
- Como podemos recuperar a distância exata de um objeto até a câmera usando apenas imagens? A solução é a triangulação a partir de dois ou mais pontos de vista diferentes.

### 1.2 Contexto histórico
- Inspirado na visão binocular humana: a ligeira diferença (disparidade) entre o que o olho esquerdo e o direito veem é o que o nosso cérebro usa para perceber a profundidade.
- A fotogrametria clássica já usava fotografias aéreas sobrepostas para criar mapas topográficos muito antes do advento dos computadores modernos.

### 1.3 Impacto prático
- **Veículos Autônomos e Robótica:** Calcular a distância exata de pedestres e obstáculos em tempo real para evitar colisões.
- **Realidade Aumentada (AR):** Mapeamento do ambiente para posicionar objetos virtuais de forma realista.
- **Reconstrução 3D:** Criar modelos tridimensionais a partir de fotos (base para tecnologias futuras do curso, como NeRFs).

---

## 2️⃣ Como Funciona

### 2.1 O Princípio da Triangulação
- Se conhecemos a distância entre duas câmeras (a linha de base ou *baseline*) e conseguimos encontrar o exato mesmo ponto na imagem da câmera esquerda e da câmera direita, podemos usar trigonometria simples para calcular a profundidade desse ponto.

### 2.2 Geometria Epipolar
- O problema do "Feature Matching": buscar um pixel da imagem esquerda em *toda* a imagem direita é extremamente custoso e sujeito a erros.
- A **Geometria Epipolar** resolve isso criando uma restrição geométrica. Ela prova matematicamente que um ponto na imagem esquerda só pode existir ao longo de uma linha específica (a *linha epipolar*) na imagem direita, reduzindo a busca de 2D para 1D.


### 2.3 Matriz Fundamental e Matriz Essencial
- A matemática que descreve essa relação entre as duas câmeras.
- **Matriz Essencial (E):** Relaciona os pontos das duas câmeras assumindo que elas estão perfeitamente calibradas (conhecemos a distância focal e o centro óptico).
- **Matriz Fundamental (F):** Uma generalização que relaciona os pixels das duas câmeras mesmo quando elas **não** estão calibradas. É calculada encontrando correspondências de pontos (ex: usando ORB ou SIFT da aula anterior) entre as duas imagens.

### 2.4 Retificação de Imagens
- Linhas epipolares podem ser diagonais, o que dificulta o processamento em matrizes.
- A **Retificação** aplica uma transformação de perspectiva nas duas imagens para que as linhas epipolares fiquem perfeitamente horizontais e alinhadas.
- Após a retificação, o pixel $(x, y)$ na imagem esquerda só precisará ser buscado na mesma linha $y$ da imagem direita, variando apenas o $x$.


### 2.5 Mapas de Disparidade (Disparity Maps)
- A **Disparidade** é a diferença na coordenada horizontal ($x_{esquerda} - x_{direita}$) do mesmo ponto nas duas imagens retificadas.
- **Regra de Ouro:** A profundidade é *inversamente proporcional* à disparidade. Objetos próximos têm grande disparidade (mudam muito de lugar entre as fotos); objetos muito distantes (como o céu) têm disparidade quase zero.
- Algoritmos como *Block Matching* (BM) ou *Semi-Global Block Matching* (SGBM) calculam essa diferença para todos os pixels, gerando uma imagem (mapa de disparidade) onde a intensidade da cor representa a distância.


---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Capturar par estéreo $\rightarrow$ Encontrar correspondências e calcular a Matriz Fundamental $\rightarrow$ Retificar as imagens $\rightarrow$ Calcular o Mapa de Disparidade $\rightarrow$ Converter Disparidade para Profundidade (Nuvem de Pontos 3D).

### 3.2 Ferramentas e exemplos
- `OpenCV` possui módulos completos e altamente otimizados para isso: `cv2.findFundamentalMat()`, `cv2.stereoRectifyUncalibrated()`, e as classes `cv2.StereoBM_create()` e `cv2.StereoSGBM_create()`.

### 3.3 Exemplo mínimo prático
- Um notebook iterativo onde:
  1. Carregamos um par de imagens estéreo famosas já retificadas (ex: o dataset "Tsukuba").
  2. Instanciamos o objeto `cv2.StereoSGBM_create()` ajustando parâmetros como `minDisparity` e `numDisparities`.
  3. Computamos o mapa de disparidade e o exibimos com o Matplotlib usando um mapa de cores térmico (colormap) para evidenciar os objetos mais próximos brilhando "quentes" e o fundo "frio".

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Documentação do OpenCV (OpenCV-Python Tutorials):**
  - *Epipolar Geometry* (Entendimento profundo de epipolos e cálculo da Matriz Fundamental).
  - *Depth Map from Stereo Images* (Aplicações práticas com Block Matching).
- **Livro-texto:** - *Computer Vision: Algorithms and Applications* (Richard Szeliski) - Capítulo 11 (Stereo Correspondence) e Capítulo 12 (3D Reconstruction).

### 🎥 Vídeos e Cursos
- **First Principles of Computer Vision (Prof. Shree Nayar):**
  - Playlist inteira sobre *Stereo*. As ilustrações matemáticas sobre a restrição epipolar são excepcionais para a intuição visual.
- **Cyrill Stachniss (YouTube):**
  - Aulas sobre fotogrametria e geometria de múltiplas vistas (excelente para a parte de álgebra linear e matrizes Essencial/Fundamental).

### 💻 Prática
- **Mini-projeto:** Pegar o celular, tirar duas fotos de um objeto estático (movendo a câmera apenas horizontalmente uns 5 cm entre as fotos). Carregar essas fotos em um script, usar ORB para encontrar correspondências, calcular a Matriz Fundamental e desenhar as linhas epipolares nas imagens usando o OpenCV.