# 📊 Previsão de Consumo de Produtos em uma Cafeteria usando Amazon SageMaker Canvas  
Projeto Desafio DIO — Machine Learning no-code com AWS SageMaker Canvas

---

## 🎯 1. Objetivo do Projeto

O objetivo deste projeto foi construir, treinar, analisar e realizar previsões de consumo utilizando Amazon SageMaker Canvas, explorando Machine Learning sem código (no-code).  
O cenário escolhido foi uma cafeteria, com foco em prever a coluna **saida**, que representa o consumo ou venda de cada produto em um determinado dia.

O fluxo do projeto seguiu as etapas sugeridas pela DIO:

1. Criar dataset sintético (1.000 registros)
2. Importar os dados no Canvas e treinar o modelo
3. Analisar métricas e impacto das variáveis
4. Fazer previsões utilizando Single Prediction
5. Registrar insights e conclusões

---

## 📁 2. Dataset Utilizado

Encontra-se na subpasta datasets, com o nome cafeteria_treinamento_simpleprediction_1000.csv

O dataset sintético inclui as seguintes colunas:

- `data_movimento` — Data do registro (não utilizada pelo modelo)
- `produto_id` — Código identificador do produto
- `produto_nome` — Nome do produto (ex.: Café Expresso, Cappuccino)
- `categoria` — Classificação (bebida, insumo, embalagem)
- `estoque_inicial` — Quantidade de estoque no início do dia
- `entrada` — Reposição do dia
- `temperatura` — Temperatura média do dia
- `eventos_regiao` — Indica eventos na região (0/1)
- `feriado` — Indica feriado (0/1)
- `promocao` — Indica promoção (0/1)
- `saida` — Variável-alvo (consumo/vendas)

---

## ⚙️ 3. Construção e Treinamento do Modelo

- O dataset foi carregado no SageMaker Canvas.
- A coluna alvo selecionada foi **saida**.
- Embora o Canvas inicialmente sugerisse Time Series Forecasting, foi configurado como **Numeric Model Type** (Regressão Tabular), o que permite o uso do recurso Single Prediction.
- O treinamento foi executado no modo **Quick Build**.

---

## 📈 4. Métricas do Modelo

Após o treinamento, as métricas apresentadas foram:

- **RMSE:** 9.599  
- **MSE:** 92.147  

Segundo o Canvas:

> “O modelo frequentemente prevê valores dentro de +/- 9.599 da saída real.”

Dentro do contexto de um dataset sintético, com ampla variabilidade, esse desempenho é considerado satisfatório e funcional para fins educacionais.

---

## 🔍 5. Impacto Global das Variáveis (Feature Importance)

| Ordem | Feature          | Impacto | Interpretação |
|------|------------------|---------|---------------|
| **1** | **temperatura**      | **30.429%** | Afeta diretamente o consumo de bebidas quentes. |
| **2** | **estoque_inicial**  | **20.921%** | Limita ou viabiliza o consumo. |
| **3** | **produto_id**       | **19.211%** | Cada produto tem seu padrão próprio. |
| **4** | **entrada**          | **14.093%** | Influencia a reposição e disponibilidade. |
| **5** | **produto_nome**     | **11.132%** | Complementa a identificação do produto. |
| **6** | categoria        | 2.081% | Baixo impacto devido à similaridade. |
| **7** | eventos_regiao   | 1.056% | Pequena influência no conjunto sintético. |
| **8** | promocao         | 0.889% | Pouco impactante no histórico. |

---

## 🔮 6. Previsões (Single Prediction Scenarios)

Foram criados cenários específicos para avaliar como o modelo reage a mudanças nas variáveis.

---

### 🟣 Cenário 1 — Café Expresso em dia frio (18°C)

**Valores utilizados:**
- Produto: PRD001 — Café Expresso  
- Estoque: 111  
- Temperatura: 18°C  
- Entrada: 0  

**Previsão:**  
### ➜ **30.006 unidades**

**Feature Importance específica:**
- temperatura: 86.29%
- produto_nome: 5.15%
- produto_id: 3.88%
- estoque_inicial: 3.18%

**Interpretação:**  
Temperaturas baixas elevam a demanda por bebidas quentes — comportamento captado perfeitamente pelo modelo.

---

### 🔵 Cenário 2 — Café Expresso em dia quente (30°C+)

**Valores utilizados:**
- Produto: PRD001 — Café Expresso  
- Estoque: 111  
- Temperatura: >30°C  

**Previsão:**  
### ➜ **10.572 unidades**

O consumo caiu cerca de 65% comparado ao cenário frio — totalmente coerente com o mercado de cafeterias.

---

### 🟢 Cenário 3 — Cappuccino com temperatura neutra (22°C)

**Valores utilizados:**
- Produto: PRD003 — Cappuccino  
- Estoque: 75  
- Temperatura: 22°C  

**Previsão:**  
### ➜ **17.935 unidades**

**Feature Importance:**
- produto_id: 29.95%
- estoque_inicial: 27.55%
- temperatura: 17.14%

**Interpretação:**  
Um consumo mais moderado e estável, como esperado para esse produto.

---

### 🔴 Cenário 4 — Estoque Baixo (15 unidades)

**Valores utilizados:**
- Produto: PRD003 — Cappuccino  
- Estoque: 15  
- Temperatura: 22°C  

**Previsão:**  
### ➜ **20.817 unidades**

**Feature Importance:**
- produto_id: 62.17%
- estoque_inicial: 30.50%

**Interpretação:**  
Mesmo com estoque baixo, o padrão histórico de demanda do produto dominou.  
Esse tipo de comportamento é comum em datasets sintéticos, onde certas correlações surgem mais fortes.

---

## 🧩 7. Conclusões

- O modelo aprendeu padrões realistas, especialmente a relação entre temperatura e consumo.  
- Produtos diferentes apresentaram demandas distintas, mostrando boa generalização.  
- Com Single Prediction foi possível simular cenários práticos e interpretar o modelo de forma clara.  
- Mesmo sendo um dataset sintético, as previsões foram coerentes e úteis.  

**Projeto concluído com sucesso.**

---

## 🙏 Agradecimentos e Notas sobre a Construção do Projeto

É importante registrar que o dataset utilizado neste projeto foi inteiramente concebido com o apoio do ChatGPT, a partir das orientações fornecidas para atender às exigências do desafio da DIO.  
Diante da proximidade do prazo final de entrega e do tempo reduzido para esclarecimento de dúvidas diretamente com a equipe da DIO — que sempre se mostra extremamente prestativa — utilizei o ChatGPT para auxiliar na criação da base de dados, na evolução da análise e na condução dos cenários, garantindo que o desafio fosse concluído dentro do prazo.

Ressalto que essa colaboração não substitui o aprendizado; ao contrário, permitiu que eu me dedicasse mais profundamente ao entendimento do SageMaker Canvas e à análise dos resultados.  

Aproveito para agradecer à equipe da DIO pelos diversos treinamentos de excelente qualidade que venho realizando, os quais têm contribuído significativamente para o meu desenvolvimento técnico e profissional.

---
