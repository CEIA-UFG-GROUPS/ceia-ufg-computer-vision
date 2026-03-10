# Ajuste de Modelos e Otimização
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que a otimização resolve
- Modelos modernos de Visão Computacional (como arquiteturas gigantes de YOLO ou Vision Transformers) são incrivelmente precisos, mas são "pesados". Eles exigem muita memória RAM/VRAM e poder computacional.
- Na prática, não podemos rodar um modelo de 500MB que exige uma GPU de última geração em uma câmera de segurança no pasto, em um drone, ou em um dispositivo móvel na borda (Edge AI).

### 1.2 Contexto histórico
- Durante muito tempo, a comunidade focou apenas em quebrar recordes de acurácia em benchmarks (como o ImageNet), o que gerou modelos cada vez mais profundos e lentos.
- Com a necessidade de democratizar a IA e rodá-la em tempo real em hardwares limitados, surgiu o foco na eficiência, culminando em técnicas para "espremer" redes neurais sem perder (muita) precisão.

### 1.3 Impacto prático
- **Redução de Custos na Nuvem:** Modelos menores e mais rápidos exigem instâncias de servidores muito mais baratas.
- **Latência:** Respostas em tempo real para sistemas críticos (ex: inferência em frações de segundo para controle de qualidade em tempo real ou biometria de animais em movimento rápido).
- **Sustentabilidade (Green AI):** Redução do consumo de energia durante a inferência.

---

## 2️⃣ Como Funciona

### 2.1 O Paradigma do Trade-off
- Toda técnica de otimização lida com o equilíbrio (trade-off) entre três pilares: **Acurácia** (quão bem o modelo prevê), **Latência** (quão rápido ele prevê) e **Tamanho/Memória** (quanto espaço ele ocupa).

### 2.2 Quantização (Quantization)
- Consiste em reduzir a precisão matemática dos pesos da rede. Em vez de armazenar números em formato de ponto flutuante de 32 bits (FP32), nós os convertemos para 16 bits (FP16) ou inteiros de 8 bits (INT8).
- **Vantagem:** Reduz o tamanho do modelo em até 4x (de FP32 para INT8) e acelera muito a inferência em hardwares otimizados.
- Tipos: *Post-Training Quantization* (PTQ - feito após o treino) e *Quantization-Aware Training* (QAT - o modelo é treinado já "sabendo" que será reduzido, mitigando a perda de acurácia).


### 2.3 Poda (Pruning)
- As redes neurais são densas, mas muitos pesos (conexões sinápticas) acabam tendo valores próximos a zero, contribuindo pouco para a decisão final.
- O *Pruning* "poda" essas conexões irrelevantes, transformando matrizes densas em matrizes esparsas.
- **Resultado:** Modelos com menos parâmetros literais. Pode ser não-estruturado (remover pesos individuais) ou estruturado (remover filtros convolucionais inteiros).


### 2.4 Destilação de Conhecimento (Knowledge Distillation)
- Usamos um modelo gigantesco e altamente preciso (o **Professor** / *Teacher*) para treinar um modelo menor e mais rápido (o **Aluno** / *Student*).
- O Aluno não aprende apenas com os gabaritos rígidos (0 ou 1) dos dados originais (Hard Labels), mas também com as probabilidades que o Professor gerou (Soft Labels) — ex: "Isso é 90% um bezerro, mas tem 10% de chance de ser uma vaca adulta". Isso transfere uma "nuvem" de conhecimento riquíssima.


### 2.5 Formatos de Exportação e Runtimes
- Como preparar o modelo para o *deploy*: a importância de sair do ecossistema de pesquisa (PyTorch `.pt` ou Keras `.h5`) e ir para padrões de interoperabilidade de mercado.
- **ONNX (Open Neural Network Exchange):** O formato universal.
- **TensorRT (NVIDIA) e OpenVINO (Intel):** Compiladores que pegam o modelo e o reescrevem da forma mais otimizada possível para a arquitetura do chip de destino.

---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Treinar modelo base (FP32) $\rightarrow$ Exportar $\rightarrow$ Aplicar técnica de otimização (ex: Quantização INT8) $\rightarrow$ Validar a queda de acurácia vs. ganho de velocidade $\rightarrow$ Empacotar para produção.

### 3.2 Ferramentas e exemplos
- O uso de containeres **Docker** torna-se essencial aqui. Compilar modelos com TensorRT ou OpenVINO exige bibliotecas de sistema C++ muito específicas, e isolar isso no Docker evita "quebrar" o ambiente local do desenvolvedor.
- O **Google Colab** continua sendo excelente para testar quantização dinâmica nativa do PyTorch/TensorFlow rapidamente.

### 3.3 Exemplo mínimo prático
- Um script comparativo no Colab:
  1. Carregar uma CNN pré-treinada (ex: MobileNet ou um modelo criado nas aulas anteriores).
  2. Medir o tamanho do arquivo original e o tempo de inferência para 1000 imagens.
  3. Aplicar *Post-Training Dynamic Quantization* com 3 linhas de código usando PyTorch.
  4. Medir o novo tamanho (que deve ser significativamente menor) e o novo tempo de inferência, comparando o F1-Score antes e depois.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Documentação do PyTorch (ou TensorFlow):**
  - Guias oficiais sobre *Quantization* e *Pruning*.
- **Paper Histórico e Acessível:**
  - *Distilling the Knowledge in a Neural Network* (Geoffrey Hinton et al., 2015). O artigo que popularizou o termo "Knowledge Distillation".

### 🎥 Vídeos e Cursos
- **TinyML and Efficient Deep Learning Computing (MIT 6.5940):**
  - As aulas do Prof. Song Han (MIT) disponíveis no YouTube são as melhores da internet sobre este assunto. Focar nos vídeos sobre *Pruning and Sparsity* e *Quantization*.
- **NVIDIA Developer (YouTube):**
  - Vídeos introdutórios sobre como o **TensorRT** funciona por baixo dos panos para fundir camadas (*Layer Fusion*) e otimizar a memória.

### 💻 Prática
- **Desafio pré-aula:** Converter um modelo salvo em `.pt` (PyTorch) ou `.keras` (TensorFlow) para o formato **ONNX**. Usar uma ferramenta visual gratuita e online chamada **Netron** (`netron.app`) para abrir o arquivo ONNX e inspecionar visualmente a estrutura e os pesos de cada camada da rede.