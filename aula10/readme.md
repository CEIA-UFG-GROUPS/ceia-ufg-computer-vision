
# Anotação de Dados e Servidores Locais
## README do Apresentador

Este documento organiza a apresentação da aula e serve como **guia conceitual** para o expositor.
A estrutura abaixo deve ser seguida para garantir clareza, progressão lógica e alinhamento com o grupo.

---

## 1️⃣ Motivação

### 1.1 O problema que a anotação resolve
- O princípio fundamental de *Data-centric AI*: um modelo de rede neural, por mais avançado que seja (como um YOLOv8 ou Mask R-CNN), é cego sem exemplos bem definidos.
- A necessidade de traduzir o conhecimento humano (ex: "isto é um bezerro", "isto é uma lesão na folha") para coordenadas matemáticas que o computador consiga processar durante o treinamento.

### 1.2 O desafio logístico
- Projetos reais de Visão Computacional frequentemente exigem a anotação de milhares de objetos (às vezes mais de 5.000 *bounding boxes* em centenas de imagens) para garantir a generalização do modelo.
- Fazer isso individualmente em ferramentas offline gera gargalos. Como uma equipe colabora na mesma base de dados sem sobrescrever o trabalho do outro? 
- Como manter a privacidade dos dados (ex: imagens médicas ou dados proprietários da indústria) sem enviá-los para plataformas na nuvem?

### 1.3 Impacto prático
- Subir um servidor próprio de anotação permite que vários anotadores trabalhem em paralelo, via navegador, diretamente na mesma base de dados.
- Permite a integração de IA assistida (Auto-Annotation), onde um modelo pré-treinado faz uma marcação inicial e o humano apenas corrige, acelerando o processo em até 10x.

---

## 2️⃣ Como Funciona

### 2.1 Tipos de Anotação e Casos de Uso
- **Bounding Boxes (Caixas Delimitadoras):** Rápidas de desenhar, padrão para detecção de objetos.
- **Polígonos e Máscaras:** Necessários para segmentação de instâncias (quando o formato exato do objeto importa).

- **Keypoints (Pontos-chave):** Usados para estimativa de pose (ex: rastrear as articulações de um animal ou humano).

### 2.2 Formatos de Exportação
- Ferramentas diferentes exigem formatos diferentes. É crucial entender a estrutura dos arquivos exportados:
  - **YOLO (.txt):** Um arquivo de texto por imagem, normalizado de 0 a 1 `(classe x_centro y_centro largura altura)`.
  - **COCO (.json):** Um único arquivo JSON gigante detalhando todas as imagens, categorias e anotações do dataset.
  - **Pascal VOC (.xml):** Formato clássico baseado em tags XML.

### 2.3 Ferramentas Locais vs. Servidores
- **LabelImg / Labelme:** Excelentes ferramentas em Python para rodar localmente e anotar pequenos lotes, mas sem recursos de colaboração.
- **CVAT (Computer Vision Annotation Tool):** Criado pela Intel, é o padrão-ouro de código aberto para servidores de anotação. Roda via navegador, suporta equipes, revisão de qualidade e auto-anotação.


### 2.4 A Infraestrutura por Trás (Docker)
- Subir um servidor robusto como o CVAT exige banco de dados (PostgreSQL), cache (Redis) e a aplicação web. 
- A forma de viabilizar isso em qualquer distribuição Linux sem conflitos de dependência é através da conteinerização com **Docker** e a orquestração com **Docker Compose**.

---

## 3️⃣ Quickstart 

### 3.1 Visão geral do fluxo
- Preparar o ambiente (Instalar Docker) $\rightarrow$ Clonar o repositório da ferramenta de anotação $\rightarrow$ Subir os containeres $\rightarrow$ Criar um projeto via interface web $\rightarrow$ Distribuir tarefas para a equipe.

### 3.2 Ferramentas e exemplos
- **Docker e Docker Compose:** Essenciais para a infraestrutura.
- **CVAT:** Foco principal da demonstração prática.

### 3.3 Exemplo mínimo prático
- Uma demonstração ao vivo ou roteiro no terminal para subir o CVAT:
  1. No terminal do Linux: `git clone https://github.com/cvat-ai/cvat`
  2. Acessar a pasta e rodar: `docker compose up -d`
  3. Criar o superusuário via linha de comando (`docker exec -it cvat_server ...`).
  4. Abrir o `localhost:8080` no navegador, fazer login, criar uma nova "Task", fazer o upload de 5 imagens de exemplo e mostrar como desenhar e exportar os dados no formato YOLO.

---

## 4️⃣ Materiais de Estudo

Para que os participantes se preparem **antes da aula** ou aprofundem os conhecimentos posteriormente, recomenda-se:

### 📖 Leituras Essenciais
- **Documentação do CVAT:**
  - Guia de Instalação Rápida (Installation Guide - Docker).
  - Guia do Usuário (Como criar projetos, tarefas e exportar datasets).
- **Especificações de Formato:**
  - O repositório oficial do formato COCO para entender a estrutura dos arquivos JSON gerados na anotação.

### 🎥 Vídeos e Cursos
- **Roboflow (YouTube):**
  - Embora o Roboflow seja uma plataforma na nuvem, o canal deles no YouTube possui os melhores tutoriais curtos sobre "Best Practices for Computer Vision Annotation" (como evitar caixas frouxas ou justas demais).
- **Tutoriais de CVAT:**
  - Buscar no YouTube por "CVAT Auto Annotation with YOLOv8" para entender como plugar um modelo dentro da ferramenta para fazer o trabalho pesado.

### 💻 Prática
- **Desafio pré-aula:** Instalar o `LabelImg` (via `pip install labelImg`) na própria máquina e anotar 10 imagens usando Bounding Boxes, exportando no formato YOLO. Em seguida, abrir os arquivos `.txt` gerados para entender como o computador lê aquelas coordenadas.