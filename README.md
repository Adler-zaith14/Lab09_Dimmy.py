# Laboratório 09 — Pipeline de RAG Avançado (HyDE e Re-ranking)

**Instituição:** ICEV — Instituto de Ensino Superior  
**Disciplina:** Tópicos em Inteligência Artificial  
**Professor:** Dimmy Magalhães  
**Autor:** Adler Castro Alves

---

## Nota de Integridade

Este laboratório foi desenvolvido de forma individual, com base nos materiais disponibilizados em aula e nas documentações oficiais das bibliotecas utilizadas. Os erros encontrados durante a execução foram identificados e corrigidos manualmente. Durante o desenvolvimento, identifiquei que o parâmetro max_prompt_length não era suportado na versão instalada do TRL e precisei removê-lo. Também corrigi o parâmetro tokenizer para processing_class, conforme exigido nas versões mais recentes da biblioteca. Além disso, ajustei a precisão de fp16 para garantir compatibilidade com o ambiente de execução utilizado. Todo esse processo de depuração e correção foi realizado por mim ao longo da execução do laboratório.

---

## Por que o Colab foi usado

Este laboratório foi desenvolvido de forma individual, focado na implementação de um sistema de busca semântica para o nicho de Direito Trabalhista. O projeto utiliza técnicas avançadas para mitigar a distância semântica entre a linguagem coloquial do usuário e o jargão jurídico.
Declaração: Partes deste laboratório foram geradas/complementadas com IA, revisadas e validadas por [Formação de estrutura textual do README, além de apoio no versionamento, geramento da chave do token no código como passo a passo pra colocar e rodar, fora o uso de pesquisas de frases relacionada ao nicho que escolhi]

---

## Análise Técnica: Por que usar HNSW?

**Conforme solicitado na tarefa analítica, avaliei o impacto dos hiperparâmetros do índice HNSW (hnsw:M e hnsw:construction_ef)
:
Eficiência vs. Memória: O HNSW consome mais memória RAM que uma busca KNN exata porque armazena uma estrutura de grafo hierárquico adicional

Parâmetros: No código, configurei M=32 e ef_construction=200.
 O aumento de M melhora a precisão ao criar mais conexões entre os vetores, mas exige mais memória. Já o ef_construction garante um grafo mais robusto durante a criação, impactando o tempo inicial de indexação


---

## Erros Encontrados e Avisos do Terminal

## Durante a execução no ambiente Colab, identifiquei e tratei os seguintes pontos observados nas saídas do terminal:
Conflitos de Dependência: O instalador do pip reportou conflitos com as bibliotecas opentelemetry. No entanto, como o core do pipeline (ChromaDB e Groq) funcionou corretamente, esses erros de versão foram ignorados sem prejuízo à execução

Aviso de Autenticação (HF_TOKEN): O terminal exibiu um aviso sobre a ausência do HF_TOKEN. Como o download dos modelos do Hugging Face para este laboratório é público, o pipeline seguiu normalmente de forma não autenticada

Mensagens de "UNEXPECTED KEY": Ao carregar os modelos all-MiniLM-L6-v2 e o Cross-Encoder, surgiram avisos de chaves inesperadas nos tensores. Conforme documentado nas notas do terminal, essas mensagens podem ser ignoradas, pois não afetam a arquitetura de inferência do modelo


---

## Demonstração de Resultados (Pipeline em Ação)
O sistema foi testado com uma query real e obteve os seguintes resultados
:
Pergunta do Usuário: "me botaram pra fora e não pagaram meus direitos"

 1. Documento Hipotético (HyDE)
O LLM gerou um texto técnico focado em Rescisão Contratual e Violação de Direitos, detalhando multas de 40% do FGTS, saldo de salário e férias vencidas para servir de base para a busca

 2. Recuperação e Re-ranking
O pipeline recuperou 10 documentos via Similaridade de Cosseno (Bi-Encoder) e refinou para os 3 melhores via Cross-Encoder

 Top 3 Finais Gerados:
Extrapolação habitual da jornada de trabalho... (Score: -10.6447)

Diferença salarial indevida... (Score: -10.8577)

Fraude na relação empregatícia mediante pejotização... (Score: -10.9050)

---

## Estrutura do Repositório

 1.)Instalação: Setup do chromadb, sentence-transformers e groq

2.)Indexação: Criação da coleção juris_rag_trabalhista com 20 documentos indexados

3.)Funções Core: Implementação do gerar_documento_tecnico (HyDE) e do loop de re-ranking

 4.)Relatório: Script final que imprime o comparativo entre Bi-Encoder e Cross-Encoder


---

## Como Executar

Abra o notebook no Google Colab.

Configure sua GROQ_API_KEY nos Secrets do ambiente

Execute todas as células. O pipeline imprimirá o relatório completo conforme mostrado na seção de resultados deste README


## Anexo 

**Google Colab:**
[https://colab.research.google.com/drive/10ZvxEcY2FcNKcEvSCzUa-kNkLHOjzCu2?usp=sharing]


