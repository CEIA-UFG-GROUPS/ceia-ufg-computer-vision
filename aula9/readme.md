# CNNs: Arquiteturas Clássicas
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que a CNN resolve
- Redes Densas (MLPs) exigem o achatamento (*flatten*) da imagem em um vetor 1D, o que destrói completamente a estrutura espacial e a relação de vizinhança entre os pixels.
- Para uma imagem de alta resolução, um MLP exigiria bilhões de parâmetros logo na primeira camada, tornando o treinamento impossível e altamente suscetível ao *overfitting*.
- A necessidade de um modelo que aprenda a extrair características locais (como bordas e texturas) e mantenha a invariância à translação espacial.

### 1.2 Contexto histórico
- **Anos 90:** A criação da LeNet-5 por Yann LeCun, usada com sucesso para reconhecer dígitos em cheques bancários, mas limitada pelo hardware da época.
- **2012 (O Ponto de Virada):** O lançamento da **AlexNet**, que utilizou GPUs para treinar uma CNN profunda e esmagou todas as técnicas clássicas (SVM, SIFT) na competição ImageNet, iniciando a era de ouro do Deep Learning na visão computacional.

### 1.3 Impacto prático
- As CNNs dominaram todas as tarefas de processamento visual (classificação, detecção, segmentação) na última década.
- Compreender estas arquiteturas clássicas é essencial, pois elas ainda são usadas como os "backbones" (extratores de características) para modelos mais complexos aplicados em biometria, monitoramento agropecuário e análise médica.

---

## 2️⃣ Como Funciona

### 2.1 Convoluções e Compartilhamento de Pesos
- Diferente de um filtro clássico (como o Sobel da Aula 2) cujos pesos são fixos, na CNN os valores da matriz do *Kernel* são os pesos que a rede **aprende** durante o *Backpropagation*.
- **Compartilhamento de pesos:** O mesmo kernel desliza por toda a imagem, procurando o mesmo padrão em diferentes lugares, reduzindo drasticamente o número de parâmetros da rede.


### 2.2 Camadas de Pooling
- A operação de Subamostragem. O **Max Pooling** é o mais comum, onde a rede seleciona apenas o maior valor de uma pequena janela (ex: 2x2).
- **Função:** Reduz a dimensionalidade espacial, economiza processamento e confere invariância a pequenas translações na imagem (se o objeto mover um pixel para o lado, o valor máximo capturado continuará o mesmo).

### 2.3 A Evolução da Profundidade: AlexNet e VGG
- **AlexNet:** Introduziu o uso massivo de ativações ReLU e Dropout.
- **VGG (Visual Geometry Group):** Sistematizou o design de redes. Provou que empilhar várias camadas convolucionais com filtros pequenos (3x3) é matematicamente mais eficiente e expressivo do que usar uma única camada com um filtro gigante (como os 11x11 usados na AlexNet).


### 2.4 Inception (GoogLeNet): Crescendo na Largura
- Por que escolher entre um filtro 1x1, 3x3 ou 5x5 se a rede pode usar todos ao mesmo tempo?
- O Bloco Inception processa a imagem em caminhos paralelos com diferentes tamanhos de filtros e depois concatena os resultados.
- Introduziu o uso de convoluções 1x1 como uma forma inteligente de "comprimir" a profundidade dos canais, poupando custo computacional.


### 2.5 ResNet (Redes Residuais): Vencendo a Profundidade
- O grande problema das redes profundas (como a VGG de 19 camadas): adicionar mais camadas começava a piorar o resultado devido ao desaparecimento do gradiente (*Vanishing Gradient*).
- A genialidade da ResNet: As conexões residuais (*Skip Connections*). A informação pula algumas camadas e é somada à saída mais à frente. Isso cria um "caminho expresso" para os gradientes fluírem livres de volta ao início da rede, permitindo treinar modelos com 50, 100 ou até 152 camadas.


---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Carregar uma imagem $\rightarrow$ Inicializar um modelo clássico pré-treinado na ImageNet $\rightarrow$ Fazer a inferência (Classificação) ou visualizar os mapas de características (*Feature Maps*).

### 3.2 Ferramentas e exemplos
- O ecossistema **Keras** (`tf.keras.applications`) fornece todas essas arquiteturas prontas com os pesos já treinados, ideais para experimentação rápida no **Google Colab**.

### 3.3 Exemplo mínimo prático
- Um script comparativo no Colab onde:
  1. Carregamos um modelo `VGG16` e um `ResNet50` pré-treinados usando o Keras.
  2. Passamos a imagem de um objeto comum.
  3. Comparamos não apenas as predições de ambos, mas usamos um código simples para "abrir a caixa preta" e plotar com Matplotlib como são as ativações (Feature Maps) da primeira camada convolucional da VGG.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Artigos Originais (Papers Históricos):**
  - *Deep Residual Learning for Image Recognition* (He et al., 2015). A leitura do paper da ResNet é surpreendentemente acessível e obrigatória.
  - *Very Deep Convolutional Networks for Large-Scale Image Recognition* (Simonyan & Zisserman, 2014) - Paper da VGG.
- **Documentação do Keras:**
  - Guia `Keras Applications` para entender como importar os modelos e usar as funções de pré-processamento nativas de cada um (`preprocess_input`).

### 🎥 Vídeos e Cursos
- **Stanford CS231n:**
  - Aula sobre *CNN Architectures*. Excelente explicação visual da evolução dos parâmetros da AlexNet até a ResNet, e como as conexões de atalho (*skip connections*) da ResNet realmente alteram a topologia da perda (*loss landscape*).
- **DeepLearning.AI (Andrew Ng - Coursera):**
  - Curso de *Convolutional Neural Networks*. O módulo que detalha passo a passo o bloco Inception e o bloco Residual é um dos mais didáticos disponíveis.

### 💻 Prática
- **Mini-projeto pré-aula:** Usar o Keras para construir um modelo VGG "em miniatura" (do zero, sem baixar pesos prontos) para classificar o dataset CIFAR-10. Focar em reproduzir o padrão de blocos: Convolução 3x3 -> Convolução 3x3 -> Max Pooling 2x2.