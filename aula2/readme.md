# Processamento de Imagem e Filtragem
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que a filtragem resolve
- Imagens do mundo real raramente são perfeitas; elas sofrem com variações de iluminação, ruído de sensores e desfoque.
- Antes de tentar extrair inteligência ou treinar modelos complexos, muitas vezes é necessário "limpar" os dados e realçar as estruturas que realmente importam (como as bordas de um objeto).

### 1.2 Contexto histórico
- O desenvolvimento das técnicas de filtragem espacial em 2D baseou-se fortemente no processamento clássico de sinais unidimensionais.
- Durante décadas, a "engenharia de características" (feature engineering) manual através de filtros era a única forma de fazer um computador reconhecer padrões antes da popularização das redes neurais.

### 1.3 Impacto prático
- Melhoria drástica na qualidade de dados de entrada em sistemas de produção que operam em ambientes não controlados (ex: câmeras de monitoramento em campo aberto, que sofrem com poeira ou baixa luminosidade).
- Redução da carga computacional ao simplificar a imagem (ex: transformar uma foto complexa em um mapa de linhas e contornos).

---

## 2️⃣ Como Funciona

### 2.1 A Operação de Convolução
- O que é um *Kernel* (ou Máscara): uma pequena matriz de pesos (ex: 3x3 ou 5x5).
- A mecânica do deslizamento: como o kernel percorre a matriz da imagem original, multiplicando os valores dos pixels e somando os resultados para gerar um novo pixel na imagem de saída.


### 2.2 Filtros Lineares (Suavização)
- **Filtro da Média (Mean Filter):** Substitui cada pixel pela média da sua vizinhança. É simples, mas costuma borrar excessivamente os detalhes.
- **Filtro Gaussiano (Gaussian Blur):** Utiliza uma distribuição normal para dar mais peso aos pixels centrais do kernel. É o padrão-ouro para remover ruídos de alta frequência preservando melhor as estruturas maiores da imagem.

### 2.3 Filtros Não-Lineares
- **Filtro da Mediana (Median Blur):** Em vez de calcular uma média, ordena os valores da vizinhança e pega o valor central. 
- Extremamente eficaz contra o "Ruído Sal e Pimenta" (salt-and-pepper noise), que consiste em pixels brancos e pretos aleatórios, sem borrar as bordas reais dos objetos.

### 2.4 Detecção de Bordas (Algoritmo de Canny)
- Uma borda representa uma mudança abrupta (alta frequência) na intensidade dos pixels.
- **O pipeline do Canny Edge Detector:**
  1. Aplicação do Filtro Gaussiano (para remover ruídos que causariam falsas bordas).
  2. Cálculo do Gradiente (intensidade e direção da borda usando operadores como o Sobel).
  3. Supressão de Não-Máximos (afinar as linhas das bordas detectadas).
  4. Thresholding com Histerese (conectar bordas fortes e descartar bordas fracas/isoladas).


---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Carregar uma imagem ruidosa $\rightarrow$ Aplicar diferentes filtros para observar o impacto $\rightarrow$ Extrair as bordas estruturais do objeto focado.

### 3.2 Ferramentas e exemplos
- **Bibliotecas Core:** `OpenCV` para as operações de convolução otimizadas e `Matplotlib` para plotar os resultados lado a lado.
- As funções essenciais: `cv2.GaussianBlur()`, `cv2.medianBlur()`, `cv2.filter2D()` (para kernels customizados) e `cv2.Canny()`.

### 3.3 Exemplo mínimo prático
- Um script simples no Colab que:
  1. Carrega uma imagem e injeta ruído artificial (opcional, para fins didáticos).
  2. Aplica um Filtro da Média e um Filtro da Mediana, exibindo como o Mediana limpa certos ruídos de forma superior.
  3. Aplica o `cv2.Canny` na imagem limpa para revelar apenas o contorno dos objetos principais.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Documentação Oficial do OpenCV:**
  - *Image Smoothing* (Aprofundamento em filtros lineares e não-lineares).
  - *Canny Edge Detection* (Explicação passo a passo de como o limiar de histerese funciona).
- **Livro-texto:** - *Digital Image Processing* (Gonzalez & Woods) - Capítulo sobre Filtragem Espacial.

### 🎥 Vídeos e Cursos
- **First Principles of Computer Vision (Prof. Shree Nayar - Columbia University):**
  - Playlist sobre *Image Filtering* e *Edge Detection* no YouTube. Excelente para quem quer entender a matemática por trás dos gradientes de imagem.
- **Stanford CS231n:**
  - Revisão de operações matemáticas espaciais nas aulas introdutórias.

### 💻 Prática
- **Desafio pré-aula:** Tentar implementar uma operação de convolução simples "na mão" usando apenas loops em Python e matrizes do NumPy, antes de usar a função pronta `cv2.filter2D()`. Isso sedimenta o entendimento de como os pesos do kernel interagem com os pixels.