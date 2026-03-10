# Segmentação de Imagens Clássica
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que a segmentação resolve
- Caixas delimitadoras (*bounding boxes*) dizem *onde* o objeto está, mas não informam a sua *forma exata*.
- A necessidade de particionar uma imagem em múltiplas regiões ou conjuntos de pixels que compartilham características similares (cor, textura, intensidade), separando o objeto de interesse do fundo (Foreground vs. Background).

### 1.2 Contexto histórico
- Antes das arquiteturas modernas de Deep Learning (como U-Net ou Mask R-CNN), a segmentação dependia estritamente de heurísticas matemáticas, teoria dos grafos e algoritmos de clusterização.
- Muitas dessas técnicas clássicas ainda são usadas hoje como etapas de pré-processamento ou para gerar anotações automáticas e *pseudo-labels* para treinar modelos pesados.

### 1.3 Impacto prático
- **Medicina:** Isolar tumores ou vasos sanguíneos em exames de ressonância.
- **Agroindústria:** Separar folhas, frutos ou animais (como gado) do solo/pasto para análises de biometria ou saúde.
- **Edição de Imagens:** Ferramentas clássicas de "Varinha Mágica" e remoção de fundo.

---

## 2️⃣ Como Funciona

### 2.1 Thresholding (Limiarização)
- A forma mais simples de segmentação. Transforma uma imagem em tons de cinza em uma imagem binária (preto e branco).
- **Threshold Global:** Um único valor de corte para a imagem toda. 
- **Método de Otsu:** Um algoritmo inteligente que calcula automaticamente o limiar ideal, minimizando a variância dentro das classes (fundo e objeto).

- **Threshold Adaptativo:** Calcula limiares diferentes para pequenas regiões da imagem, ideal para fotos com iluminação irregular.

### 2.2 K-means Clustering na Imagem
- Tratar a imagem não como uma grade 2D, mas como um conjunto de pontos num espaço de cores (ex: RGB ou HSV).
- O algoritmo agrupa os pixels em $K$ clusters com base na similaridade de cor, substituindo a cor original do pixel pela cor do centroide do seu cluster.


### 2.3 Mean Shift
- Ao contrário do K-means, o Mean Shift não exige que você defina o número de clusters ($K$) antecipadamente.
- Funciona deslizando "janelas" pelo espaço de cores em direção às áreas de maior densidade (picos). É excelente para suavizar a imagem preservando as bordas estruturais.

### 2.4 Graph Cuts
- Modela a imagem como um grafo matemático, onde cada pixel é um "nó" e as "arestas" conectam pixels vizinhos (com pesos baseados na similaridade de cor ou intensidade).
- Adicionam-se dois nós especiais: Source (Objeto) e Sink (Fundo).
- O algoritmo encontra o "corte mínimo" (Min-Cut / Max-Flow) que separa o objeto do fundo rompendo as arestas de menor peso (onde há bordas evidentes).

### 2.5 Superpixels (SLIC)
- Agrupa pixels vizinhos que têm cores semelhantes em "blocos" maiores chamados de superpixels.
- Reduz drasticamente a complexidade da imagem (ex: de 1.000.000 de pixels para 500 superpixels), facilitando e acelerando o processamento por algoritmos subsequentes, como o Graph Cuts.


---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Carregar imagem $\rightarrow$ Escolher espaço de cores (ex: converter para HSV para isolar tons de verde) $\rightarrow$ Aplicar técnica de agrupamento ou corte $\rightarrow$ Gerar a Máscara Binária final.

### 3.2 Ferramentas e exemplos
- **Google Colab:** Perfeito para rodar os algoritmos de clusterização e plotar os resultados rapidamente com Matplotlib.
- **Bibliotecas Core:** - `cv2.threshold` e `cv2.kmeans` (OpenCV).
  - `skimage.segmentation` (Scikit-Image) é a melhor biblioteca em Python para algoritmos de segmentação mais refinados, como SLIC (Superpixels).

### 3.3 Exemplo mínimo prático
- Um script simples que:
  1. Carrega uma imagem de um objeto em um fundo razoavelmente contrastante.
  2. Aplica o K-means com $K=3$ (para separar fundo, sombra e objeto principal).
  3. Plota a imagem segmentada com as cores dos centroides.
  4. Aplica um threshold de Otsu em um dos canais resultantes para extrair a máscara binária (preto/branco) perfeita do objeto.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Documentação do OpenCV:**
  - *Image Thresholding* e *Otsu's Binarization*.
  - *K-Means Clustering in OpenCV* (Excelente tutorial aplicando K-means diretamente em pixels).
  - *Interactive Foreground Extraction using GrabCut* (Uma implementação clássica baseada em Graph Cuts presente no OpenCV).
- **Documentação do Scikit-Image:**
  - Guia de Segmentação (`skimage.segmentation`), especialmente a documentação sobre SLIC.

### 🎥 Vídeos e Cursos
- **First Principles of Computer Vision (Prof. Shree Nayar):**
  - Playlist sobre *Image Segmentation*. Os vídeos sobre K-means e Mean Shift são altamente intuitivos e explicam a densidade no espaço de cores de forma brilhante.
- **Computer Vision - University of Central Florida (Mubarak Shah):**
  - Aulas teóricas sobre Graph Cuts e Min-Cut/Max-Flow (para quem quiser entender a teoria dos grafos por trás do método).

### 💻 Prática
- **Desafio pré-aula:** Usar o algoritmo **GrabCut** do OpenCV. O desafio é carregar uma imagem, desenhar um retângulo envolta do objeto principal e deixar o algoritmo separar o fundo quase perfeitamente usando a lógica de Graph Cuts por debaixo dos panos.