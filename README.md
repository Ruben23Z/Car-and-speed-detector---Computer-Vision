# Contagem e Estimação de Velocidade de Veículos em Vídeo

> Sistema de visão por computador que deteta, rastreia e estima automaticamente a velocidade de veículos em sequências de vídeo de trânsito rodoviário, com precisão sub-frame.

---

## Contexto e Motivação

Este projeto foi desenvolvido no âmbito da unidade curricular de **Processamento de Imagem e Visão (PIV)**, do curso de Engenharia Informática no **ISEL — Instituto Superior de Engenharia de Lisboa**, no semestre de Inverno 2025/26.

O problema de monitorização de velocidade em contexto rodoviário é classicamente resolvido com radares ou sensores físicos — equipamentos dispendiosos e de instalação complexa. Este projeto explora uma abordagem alternativa e mais acessível: a análise automática de vídeo de vigilância já existente para extrair informação de velocidade, sem necessidade de qualquer hardware adicional.

A principal motivação foi demonstrar que técnicas clássicas de processamento de imagem — como subtração de fundo, operações morfológicas e rastreamento por correspondência de centroides — são suficientes para produzir estimativas de velocidade com precisão aceitável para fins de análise de tráfego.

---

## Principais Funcionalidades

- **Estimação automática do fundo estático** da cena, a partir de múltiplos fotogramas espaçados do vídeo, eliminando o ruído ambiental antes de qualquer detecção.

- **Detecção de objetos em movimento** através da diferença entre cada fotograma e o fundo estimado, seguida de limiarização adaptativa (Otsu).

- **Limpeza morfológica** da máscara binária, reduzindo falsos positivos e consolidando regiões que correspondem a veículos individuais.

- **Rastreamento multi-veículo** com atribuição de identidade persistente ao longo dos fotogramas, usando distância euclidiana entre centroides e tolerância a ausências temporárias.

- **Medição de velocidade com interpolação sub-frame** — em vez de contar apenas fotogramas inteiros, o sistema calcula a fracção exata do intervalo entre fotogramas em que o veículo cruzou cada linha de detecção, resultando em estimativas temporais de maior resolução.

- **Relatório estatístico final** com velocidade média, mínima, máxima e desvio padrão da amostra de veículos detetados.

- **Exportação de vídeo anotado**, com sobreposição visual das linhas de detecção, caixas delimitadoras, identificadores e velocidade estimada por veículo.

---

## Tecnologias Empregues

- **Python 3** — Linguagem principal de desenvolvimento
- **OpenCV** — Processamento de imagem, captura de vídeo e exportação de resultados visuais
- **NumPy** — Operações matriciais e cálculos estatísticos
- **Matplotlib** — Visualização interativa de fotogramas e histogramas durante o desenvolvimento
- **Jupyter Notebook** — Ambiente de desenvolvimento e apresentação iterativa do trabalho

---

## Abordagem Técnica (Visão Geral)

O pipeline de processamento segue as seguintes etapas: 

A medição de velocidade baseia-se em duas **linhas virtuais horizontais** definidas na cena. Quando um veículo rastreado cruza a primeira linha (entrada) e depois a segunda (saída), o sistema regista os instantes exatos — com precisão sub-frame via interpolação linear — e calcula a velocidade com base na distância real conhecida entre as linhas.

---

## Resultados Obtidos

Com base no vídeo de teste fornecido (`AutoEstrada.wm`), o sistema processou **12 veículos detetados**, dos quais **11 completaram o percurso** entre as duas linhas de medição, produzindo os seguintes resultados:

| Métrica              | Valor         |
|----------------------|---------------|
| Velocidade Média     | **73,55 km/h** |
| Velocidade Mínima    | 54,57 km/h    |
| Velocidade Máxima    | 99,03 km/h    |
| Desvio Padrão        | 15,23 km/h    |
| Tempo Médio de Percurso | 0,3565 s  |

---

## Impato e Aprendizagem

Este projeto consolidou competências fundamentais em visão por computador aplicada a cenários reais, nomeadamente:

- **Raciocínio sobre precisão de medição** — a introdução da interpolação sub-frame não foi apenas um refinamento técnico, mas uma decisão de engenharia consciente: com 24 fps, cada fotograma representa ~41 ms de incerteza. Ignorar essa granularidade introduziria erros sistemáticos nas estimativas de velocidade.

- **Pipeline de processamento de imagem** — a cadeia completa, desde a aquisição até ao relatório final, obrigou a tomar decisões de compromisso em cada etapa (threshold, kernels morfológicos, limiar de distância para rastreamento) com base em observação empírica do comportamento do sistema.

- **Comunicação de resultados** — a produção de um vídeo anotado e de um relatório estatístico estruturado demonstra que um sistema técnico só é completo quando os seus resultados são interpretáveis por terceiros.

Este trabalho serve igualmente como prova de conceito de que soluções de monitorização de tráfego de baixo custo são viáveis com ferramentas de código aberto e vídeo de câmara convencional.

---

## Autores

| Nome            | Número |
|-----------------|--------|
| **Ruben Zhang** | A51388 |
| **Rodrigo Neves** | A51452 |

**Turma 51D — Docente: Pedro Jorge**
ISEL — Processamento de Imagem e Visão, Inverno 2025/26

---

*Projeto de natureza académica, desenvolvido para fins de aprendizagem e avaliação.*
