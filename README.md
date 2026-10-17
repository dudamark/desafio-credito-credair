# Análise de Crédito: CredAIR

Análise de dados e modelo de classificação para prever a inadimplência de clientes da CredAIR, uma financeira fictícia. Projeto desenvolvido como Desafio 1 da trilha de análise de dados.

## Objetivo

Identificar os fatores associados à inadimplência e construir um modelo que apoie a decisão de concessão de crédito.

## Base de dados

O arquivo `data/credito.csv` contém 5.000 propostas de crédito com 11 colunas:

| Coluna | Descrição |
|---|---|
| idade | Idade do cliente |
| estado | UF do cliente |
| escolaridade | Nível de escolaridade |
| renda_mensal | Renda mensal, em reais |
| tempo_emprego_anos | Tempo no emprego atual, em anos |
| score_credito | Score de crédito (335 a 850) |
| valor_solicitado | Valor do empréstimo, em reais |
| prazo_meses | Prazo do empréstimo (6 a 60 meses) |
| finalidade | Finalidade do empréstimo |
| dividas_ativas | Número de dívidas ativas |
| inadimplente | Alvo: 1 para inadimplente, 0 para adimplente |

## Estrutura do projeto

```
desafio-credito-credair/
├── data/
│   └── credito.csv
├── notebooks/
│   └── analise_credito.ipynb
├── reports/
├── requirements.txt
├── .gitignore
└── README.md
```

## Como executar

Requisitos: Python 3.14.

```powershell
git clone https://github.com/dudamark/dessfio-credito-credair.git
cd dessfio-credito-credair
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
jupyter lab
```

No JupyterLab, abra `notebooks/analise_credito.ipynb` e execute todas as células em ordem (Run > Run All Cells).

## Etapas da análise

1. **Entendimento do problema**: definição do objetivo e da variável alvo.
2. **Análise exploratória**: distribuição das variáveis e relação de cada uma com a inadimplência.
3. **Limpeza e preparação**: padronização do estado, remoção de 19 duplicadas e de 10 rendas abaixo do piso de 900 reais, correção de 15 idades e limitação do tempo de emprego à idade menos 14 anos. Base final com 4.971 registros.
4. **Engenharia de atributos**: criação do comprometimento de renda (parcela estimada sem juros dividida pela renda) e da renda em escala logarítmica; seleção de atributos com base em correlação e testes estatísticos.
5. **Preparação para modelagem**: divisão estratificada em treino (80%) e teste (20%).
6. **Modelagem**: Regressão Logística em pipeline com padronização dos atributos.
7. **Avaliação**: comparação com um modelo de referência, ajuste para o desbalanceamento das classes e matriz de confusão.
8. **Conclusão**: fatores de risco, desempenho, recomendações e limitações.

## Principais resultados

- O **comprometimento de renda** é o fator mais associado à inadimplência. Quando a parcela supera a renda, 94,5% dos clientes ficam inadimplentes.
- **Score de crédito** e **dívidas ativas** completam os três principais fatores de risco.
- O risco dos prazos curtos vem do peso da parcela na renda, não do prazo em si.
- Estado, escolaridade, finalidade e idade não mostraram relação relevante com a inadimplência.

Modelo final: Regressão Logística com pesos balanceados e 6 atributos (score, comprometimento de renda, renda em log, dívidas ativas, prazo e tempo de emprego).

| Métrica (teste) | Referência | Modelo final |
|---|---|---|
| Acurácia | 0,656 | 0,733 |
| Precisão | 0,000 | 0,596 |
| Recall | 0,000 | 0,687 |
| F1 | 0,000 | 0,639 |
| AUC | 0,500 | 0,805 |

Entre as propostas que o modelo aprovaria no teste, a taxa de inadimplência é de 17,8%, contra 34,4% da base.

## Limitações

- A base não informa a taxa de juros; a parcela foi estimada sem juros.
- Os dados são fictícios, e a relação muito forte entre comprometimento de renda e inadimplência provavelmente reflete a forma como foram gerados.
- O ponto de corte de aprovação não foi calibrado com os custos reais de cada tipo de erro.

## Tecnologias

Python, pandas, NumPy, matplotlib, seaborn, SciPy, scikit-learn e JupyterLab.

## Autor

Carlos Marques
