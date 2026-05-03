# NPS Preditivo — Tech Challenge Fase 1
## Pós-Graduação em IA Science | FIAP Pos Tech

---

## Objetivo

Analisar dados operacionais de um e-commerce para identificar os fatores que influenciam a satisfação do cliente (NPS) e construir um modelo preditivo capaz de antecipar se um cliente será **Satisfeito** ou **Insatisfeito** antes da aplicação da pesquisa de NPS.

---

## Estrutura do Repositório

```
├── data/
│   └── desafio_nps_fase_1.csv       # Base de dados original
├── notebooks/
│   └── tech_challenge_nps.ipynb     # Notebook principal com toda a análise
├── models/
│   ├── predicoes_nps.csv            # Predições no conjunto de teste (500 registros)
│   └── simulacao_acoes_nps.csv      # Simulação de ações para toda a base (2500 registros)
├── reports/
│   └── figures/                     # Gráficos gerados pela EDA e comparação de modelos
├── requirements.txt                 # Dependências do projeto
└── README.md
```

---

## Descrição da Base de Dados

Arquivo: `data/desafio_nps_fase_1.csv`

| Variável | Descrição |
|---|---|
| `customer_id` | Identificador único do cliente |
| `order_id` | Identificador único do pedido |
| `customer_age` | Idade do cliente |
| `customer_region` | Região geográfica |
| `customer_tenure_months` | Tempo de relacionamento com a empresa (meses) |
| `order_value` | Valor total do pedido |
| `items_quantity` | Quantidade de itens no pedido |
| `discount_value` | Valor de desconto aplicado |
| `payment_installments` | Número de parcelas |
| `delivery_time_days` | Tempo total de entrega (dias) |
| `delivery_delay_days` | Dias de atraso na entrega |
| `freight_value` | Valor do frete |
| `delivery_attempts` | Número de tentativas de entrega |
| `customer_service_contacts` | Contatos do cliente com atendimento |
| `resolution_time_days` | Tempo para resolução de problemas (dias) |
| `complaints_count` | Número de reclamações registradas |
| `repeat_purchase_30d` | Recompra em 30 dias (0=não, 1=sim) |
| `csat_internal_score` | Score interno de satisfação |
| `nps_score` | **Variável alvo** — nota NPS de 0 a 10 |

**Total:** 2.500 registros, sem valores nulos.

---

## Metodologia

### Requisito 1 — Entendimento do Negócio
Análise qualitativa do problema: por que o NPS importa para e-commerce, quais áreas se beneficiam dos insights e como o NPS impacta recompra, boca a boca e market share.

### Requisito 2 — Definição da Target
Identificação e justificativa da variável `nps_score` como alvo do problema, com discussão de riscos como viés de resposta e data leakage.

### Requisito 3 — Análise Exploratória (EDA)
EDA orientada a negócio com foco em:
- Distribuição do NPS e proporção de Promotores/Neutros/Detratores
- Correlação das variáveis operacionais com o NPS
- Impacto do atraso na entrega, reclamações e contatos com atendimento
- Segmentação por perfil de cliente (região, idade, tempo de relacionamento)

### Requisito 4 — Modelo Preditivo
Classificação binária: **Satisfeito (NPS ≥ 7)** vs **Insatisfeito (NPS < 7)**

| Modelo | AUC-ROC |
|---|---|
| Random Forest | 0.8771 |
| **Regressão Logística** | **0.8640** |
| Gradient Boosting | 0.8630 |

**Modelo escolhido: Regressão Logística** — melhor equilíbrio entre classes (recall 0.74 para satisfeitos vs. 0.26 do Random Forest), mais interpretável e adequado para análise de fatores de insatisfação.

Features utilizadas (14 variáveis operacionais — sem data leakage):
- `delivery_delay_days`, `complaints_count`, `customer_service_contacts`, `resolution_time_days`, `delivery_time_days`, `delivery_attempts`, `order_value`, `freight_value`, `items_quantity`, `discount_value`, `payment_installments`, `customer_age`, `customer_tenure_months`, `region_encoded`

> Variáveis `csat_internal_score` e `repeat_purchase_30d` excluídas por risco de data leakage.

---

## Como Reproduzir os Resultados

### 1. Pré-requisitos

```bash
pip install -r requirements.txt
```

### 2. Executar o notebook

```bash
jupyter notebook notebooks/tech_challenge_nps.ipynb
```

Ou via Anaconda Navigator, abrir o arquivo `notebooks/tech_challenge_nps.ipynb`.

### 3. Ordem de execução

Execute todas as células em sequência (Cell > Run All). O notebook irá:
1. Carregar os dados de `data/desafio_nps_fase_1.csv`
2. Realizar a EDA e gerar os gráficos em `reports/figures/`
3. Treinar e comparar os 3 modelos de classificação e 3 de regressão
4. Gerar `models/predicoes_nps.csv` e `models/simulacao_acoes_nps.csv`

### Ambiente testado
- Python 3.13 (Anaconda)
- pandas, numpy, scikit-learn, matplotlib, seaborn, joblib

---

## Principais Resultados

- **74% dos clientes são detratores** — NPS calculado da empresa ≈ -66
- **Atraso na entrega** é o principal destruidor de satisfação (correlação -0.60 com NPS)
- Com apenas 1 dia de atraso, o NPS médio cai de 6.9 para 4.6
- Clientes sem reclamações têm NPS médio de 8.5 (promotores)
- Perfil demográfico (região, idade) **não** diferencia NPS — o problema é operacional

---

## Grupo

Pós-Graduação em IA Science — FIAP Pos Tech  

