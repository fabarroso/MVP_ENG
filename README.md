# MVP_ENG
**MVP -  Engenharia de Dados**

**Nome:** Fabio de Andrade Barroso

**Matricula:** 4052025000158

**Dataset:** Dados e estatísticas do Campeonato Brasileiro de Futebol

**Cobertura temporal:** 2003 - 2023

**Visão Geral do Projeto**

Este projeto implementa um pipeline analítico completo para dados históricos do Campeonato Brasileiro (2003–2023), utilizando uma arquitetura em camadas (Medallion Architecture) composta por Bronze, Silver e Gold, com foco em qualidade dos dados, governança e suporte à análise exploratória e histórica.

Os dados foram obtidos a partir de datasets públicos do Kaggle, em formato CSV, sem restrições de licença, e abrangem partidas, gols, cartões e estatísticas de jogo.

Os arquivos CSV foram carregados diretamente no catálogo do Databricks, integrando o metastore como fontes da camada Bronze. Esse procedimento garantiu execução centralizada, controle de schema e disponibilidade imediata dos dados para processamento distribuído e para as etapas subsequentes do pipeline (Silver e Gold).

**Arquitetura e Pipeline de Dados**

**Camada Bronze — Ingestão**

Os arquivos CSV brutos são carregados e persistidos como tabelas Delta, mantendo os dados em seu estado original. Nesta etapa são adicionados metadados técnicos, como data de ingestão e origem do arquivo, além da padronização dos nomes das colunas para snake_case. O objetivo é garantir rastreabilidade e preservar a integridade da fonte.

**Camada Silver — Padronização e Qualidade**

A camada Silver consolida os dados da Bronze aplicando correções, normalizações e conversões de tipos. Entre os principais tratamentos realizados estão:

- Conversão segura de datas e timestamps.
- Normalização textual de clubes, arenas e categorias.
- Correção de valores especiais (ex.: “-” → “Empate”).
- Conversão de métricas para tipos numéricos consistentes.
- Criação de atributos derivados, como minuto_int para gols e cartões.
- Correções semânticas pontuais, como padronização de posições de jogadores.

Ao final, são geradas quatro tabelas Silver consistentes e prontas para análise.

**Análise de Qualidade dos Dados**

Após a Silver, é executada uma análise de qualidade que valida:

- Valores nulos e sua aderência à realidade da fonte.
- Tipos de dados e schemas.
- Domínios categóricos.
- Intervalos mínimos e máximos de variáveis numéricas.
- Distribuições estatísticas (histogramas).
- Unicidade das chaves nativas.

Os resultados confirmam a confiabilidade da camada Silver para uso analítico.

**Catalogo de Dados**

Foi construído um catálogo técnico da camada Silver, documentando colunas, tipos, exemplos de domínio, valores mínimos e máximos e descrições funcionais. Esse catálogo apoia governança, entendimento do modelo e manutenção do pipeline.

**Camada Gold — Modelagem Dimensional**

Na camada Gold, os dados são organizados em um modelo dimensional Snowflake, composto por:

- Dimensões: dim_tempo, dim_clube, dim_arena
- Tabelas fato: fato_partida, fato_gol, fato_cartao, fato_estatistica_time_partida

O modelo garante integridade referencial e suporta análises históricas, comparativas e temporais sobre desempenho, comportamento disciplinar e estatísticas de jogo.

**Análises**

Com a camada Gold, foram realizadas análises que respondem a perguntas de negócio como:

- Quais clubes tiveram o melhor desempenho geral ao longo dos 20 anos?
- Em quais períodos da partida ocorrem mais gols?
- Times com maior posse de bola vencem mais?
- Quais clubes receberam mais cartões?
- Existe vantagem significativa em jogar como mandante?
- Quais arenas receberam mais partidas e como os clubes performam nelas?
- Como o desempenho dos clubes evoluiu ao longo dos anos?

**Resultado Final**

O projeto entrega um Data Warehouse confiável, documentado e analiticamente consistente, permitindo consultas SQL, visualizações e geração de insights sobre 20 anos do Campeonato Brasileiro.

