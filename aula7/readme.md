# Introdução a Redes Neurais (Revisão)
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que resolve
- Modelos lineares clássicos são excelentes e rápidos, mas falham catastroficamente ao tentar separar dados que não são linearmente separáveis (o famoso "Problema do XOR").
- A necessidade de criar modelos capazes de aprender representações não-lineares complexas e hierárquicas diretamente dos dados.

### 1.2 Contexto histórico
- **1957:** A invenção do Perceptron por Frank Rosenblatt (inspirado em neurônios biológicos).
- **1969:** O livro "Perceptrons" (Minsky & Papert) prova as limitações do modelo de camada única, causando o primeiro "Inverno da IA".
- **Anos 80:** A popularização do algoritmo de *Backpropagation*, permitindo finalmente treinar redes com múltiplas camadas (Multi-Layer Perceptrons - MLPs) e revivendo o campo.

### 1.3 Impacto prático
- É a base matemática e arquitetural de absolutamente tudo o que chamamos de Deep Learning hoje (CNNs, Transformers, GANs, etc.).
- Permitiu que as máquinas parassem de depender da extração manual de características (abordada nas aulas anteriores) e começassem a aprender as melhores *features* sozinhas.

---

## 2️⃣ Como Funciona

### 2.1 O Perceptron (O Neurônio Artificial)
- A unidade fundamental. Recebe entradas $x_i$, multiplica por pesos sinápticos $w_i$, soma tudo e adiciona um viés (bias) $b$.
- A equação linear base: $z = \sum (w_i \cdot x_i) + b$.


### 2.2 Funções de Ativação (A Não-Linearidade)
- Aplicar transformações matemáticas após a soma ponderada para permitir que a rede aprenda padrões complexos. Sem elas, uma rede de 100 camadas seria matematicamente equivalente a uma única camada linear.
- **Sigmoid / Tanh:** Clássicas, transformam a saída para o intervalo [0, 1] ou [-1, 1]. Sofrem com o problema do Desaparecimento do Gradiente (*Vanishing Gradient*).
- **ReLU (Rectified Linear Unit):** A revolução moderna ($f(x) = \max(0, x)$). Simples, rápida e resolve grande parte do problema do desaparecimento do gradiente.


### 2.3 Backpropagation (Retropropagação)
- Como a rede "aprende". 
- **Forward Pass:** Os dados entram, passam pelas camadas e a rede faz uma previsão.
- **Função de Perda (Loss):** Calcula o quão errada foi a previsão em relação ao gabarito real (ex: Mean Squared Error ou Cross-Entropy).
- **Backward Pass:** Usando a Regra da Cadeia do cálculo diferencial, o algoritmo viaja de trás para frente na rede, calculando a derivada parcial (gradiente) da perda em relação a cada peso.

### 2.4 Otimizadores (Atualização dos Pesos)
- Uma vez que temos os gradientes, como ajustamos os pesos para minimizar o erro na próxima vez?
- **SGD (Stochastic Gradient Descent):** Dá um passo na direção oposta ao gradiente. Pode ser lento e ficar preso em mínimos locais.
- **Adam (Adaptive Moment Estimation):** O padrão-ouro da indústria hoje. Adiciona o conceito de "momento" (como uma bola rolando morro abaixo que ganha velocidade) e taxas de aprendizado adaptativas para cada peso, acelerando drasticamente a convergência.


---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Definir arquitetura (MLP) $\rightarrow$ Escolher Função de Perda e Otimizador $\rightarrow$ Compilar $\rightarrow$ Treinar (Épocas e Batches) $\rightarrow$ Avaliar.

### 3.2 Ferramentas e exemplos
- O **Google Colab** é o ambiente ideal para esta aula, aproveitando a aceleração gratuita de GPU.
- O **Keras** (junto com o TensorFlow) é perfeito para construir MLPs de forma limpa e intuitiva, abstraindo o cálculo pesado de tensores que ocorreria em código puro.

### 3.3 Exemplo mínimo prático
- Um notebook no Colab que:
  1. Cria um *toy dataset* não-linear (ex: o dataset "moons" do scikit-learn).
  2. Constrói um MLP simples usando a API Sequencial do Keras (uma camada oculta com ReLU e uma camada de saída com Sigmoid).
  3. Compila o modelo com o otimizador Adam.
  4. Plota a curva de *Loss* caindo ao longo das épocas de treinamento, mostrando a rede aprendendo ao vivo.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Deep Learning Book (Ian Goodfellow):**
  - Capítulo 6 (Deep Feedforward Networks) e Capítulo 8 (Optimization for Training Deep Models). O rigor matemático definitivo.
- **Documentação do Keras:**
  - Guia inicial (*Introduction to Keras for Engineers*), focando nas camadas `Dense` e nas classes de otimizadores.

### 🎥 Vídeos e Cursos
- **3Blue1Brown (YouTube):**
  - Playlist *Neural Networks*. Possivelmente a melhor explicação visual da internet sobre o que realmente significa o Backpropagation e a descida do gradiente. Obrigatório para intuição matemática.
- **Andrej Karpathy (YouTube):**
  - Aula *The spelled-out intro to neural networks and backpropagation: building micrograd*. Excelente para quem quer ver os cálculos sendo feitos linha a linha em Python do zero.

### 💻 Prática
- **TensorFlow Playground (playground.tensorflow.org):**
  - Ferramenta interativa no navegador onde o aluno pode adicionar camadas, mudar funções de ativação e ver em tempo real como o MLP tenta separar os pontos de dados. Perfeito para visualização prática.