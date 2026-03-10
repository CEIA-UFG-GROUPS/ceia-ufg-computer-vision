# Structure from Motion (SfM) e SLAM
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que resolve
- Na visão estéreo (aula anterior), tínhamos duas câmeras fixas e conhecidas. Mas e se tivermos apenas *uma* câmera em movimento (como um vídeo de celular)? 
- Como extrair a estrutura 3D da cena (Structure) a partir do movimento da câmera (Motion)?
- O problema do "Ovo e da Galinha" na navegação autônoma: um robô precisa de um mapa para se localizar, mas precisa saber sua localização exata para conseguir desenhar o mapa.

### 1.2 Contexto histórico
- O desenvolvimento da fotogrametria digital nos anos 90 e 2000 viabilizou o SfM para reconstruir monumentos turísticos a partir de milhares de fotos aleatórias da internet (ex: o famoso projeto "Building Rome in a Day").
- A evolução do hardware permitiu que esses cálculos pesados fossem feitos em tempo real, dando origem ao SLAM visual moderno.

### 1.3 Impacto prático
- **Realidade Aumentada (AR):** ARKit (Apple) e ARCore (Google) rodam SLAM continuamente para "fixar" objetos virtuais no mundo real.
- **Navegação Autônoma:** Rovers espaciais, robôs aspiradores e carros autônomos usam SLAM para mapear ambientes desconhecidos e desviar de obstáculos.
- **Topografia e Agro:** Drones voando sobre plantações para gerar modelos 3D do terreno.

---

## 2️⃣ Como Funciona

### 2.1 O Pipeline de Structure from Motion (SfM)
- Diferente da visão estéreo clássica, aqui não temos a distância entre as câmeras. O SfM reconstrói simultaneamente a geometria 3D da cena e as poses (posição e rotação) da câmera em cada quadro do vídeo.
- **Passos:** Extração de Features (SIFT/ORB) $\rightarrow$ Matching entre quadros $\rightarrow$ Estimativa de Pose $\rightarrow$ Triangulação.


### 2.2 Bundle Adjustment (Otimização Global)
- Conforme a câmera se move e calcula posições, pequenos erros matemáticos se acumulam (o chamado *drift*).
- O **Bundle Adjustment** é um algoritmo de otimização não-linear gigantesco. Ele ajusta *todos* os parâmetros 3D dos pontos e *todas* as posições da câmera ao mesmo tempo para minimizar o "erro de reprojeção" (a diferença entre onde o ponto 3D foi calculado e onde o pixel realmente aparece na imagem).

### 2.3 SLAM (Simultaneous Localization and Mapping)
- O SLAM é, em essência, o SfM aplicado em **tempo real**. O sistema não pode esperar o vídeo acabar para processar os dados; ele precisa tomar decisões em frações de segundo.
- **Visual SLAM (vSLAM):** Utiliza apenas câmeras (RGB ou RGB-D).
- **Lidar SLAM:** Utiliza sensores a laser para gerar mapas de profundidade altamente precisos.

### 2.4 Loop Closure (Fechamento de Ciclo)
- O maior desafio do SLAM. O que acontece quando o robô dá a volta no quarteirão e chega ao ponto de partida? Devido ao *drift*, o mapa gerado não vai se alinhar perfeitamente.
- O *Loop Closure* usa reconhecimento de padrões visuais para identificar: "Eu já estive aqui!". Ao confirmar isso, o algoritmo deita mão a uma otimização em grafo para corrigir todo o mapa retroativamente.


---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Capturar vídeo $\rightarrow$ Rastrear pontos de interesse temporalmente $\rightarrow$ Estimar posições relativas da câmera $\rightarrow$ Gerar nuvem de pontos esparsa (Sparse Point Cloud) $\rightarrow$ Densificar para um modelo 3D sólido (Dense Point Cloud).

### 3.2 Ferramentas e exemplos
- **COLMAP:** O padrão-ouro acadêmico e de mercado para SfM. É uma ferramenta *state-of-the-art* que possui tanto interface gráfica quanto linha de comando.
- **ORB-SLAM3:** Uma das bibliotecas *open-source* mais famosas e robustas para SLAM visual em tempo real.

### 3.3 Exemplo mínimo prático
- Fazer uma demonstração ao vivo ou gravada usando o **COLMAP**:
  1. O apresentador grava um vídeo de 15 segundos circundando um objeto estático (ex: uma estátua ou um banco de praça).
  2. Extrai os frames do vídeo (ex: 2 frames por segundo).
  3. Importa no COLMAP, roda a extração de *features* e inicia o mapeamento automático.
  4. Exibe a interface do COLMAP mostrando a nuvem de pontos 3D gerada e as pirâmides vermelhas representando onde a câmera (celular) estava em cada momento da gravação.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Documentação do COLMAP:**
  - O tutorial inicial descreve muito bem a teoria por trás da reconstrução densa e esparsa.
- **Papers Históricos (Avançado):**
  - *ORB-SLAM: a Versatile and Accurate Monocular SLAM System* (Raúl Mur-Artal et al., 2015). Uma leitura fantástica para entender como um sistema de tempo real é arquitetado.

### 🎥 Vídeos e Cursos
- **Cyrill Stachniss (Universidade de Bonn):**
  - As aulas do Prof. Cyrill no YouTube sobre Fotogrametria e SLAM são lendárias na comunidade de robótica. Procurar especificamente pela aula de *Bundle Adjustment* e *Graph-based SLAM*.
- **First Principles of Computer Vision (Prof. Shree Nayar):**
  - Aula sobre *Structure from Motion* para a intuição visual da triangulação iterativa.

### 💻 Prática
- **Desafio pré-aula:** Instalar o COLMAP na máquina local (ou rodar via Docker). Gravar um conjunto de 30 a 50 fotos de um objeto na própria mesa e rodar o pipeline "Automatic Reconstruction" para ver a mágica do SfM acontecendo na prática.