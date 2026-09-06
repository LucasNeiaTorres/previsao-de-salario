# previsao-de-salario

Regressão linear que estima o salário de jogadores do FIFA a partir de atributos do
jogo — idade, posição, média, potencial e pontuação total. Notebook único, do
carregamento cru dos dados até a leitura dos coeficientes do modelo.

## Contexto

Projeto feito durante uma Semana de Data Science, como exercício de ponta a ponta de
um pipeline de regressão: limpar os dados, codificar o que é categórico, treinar,
medir e **interpretar** — não só prever.

## O que o notebook faz

1. **Limpa o alvo.** O salário vem como texto com símbolo e sufixo (`€110K`); o
   notebook remove o `€`, troca `K` por `000` e converte para inteiro.
2. **Escolhe as features.** Fica com `Idade`, `Posicao`, `Media`, `Potencial` e
   `Total_Pontos`, e descarta o que não ajuda o modelo (nome e time).
3. **Codifica a posição.** `Posicao` é categórica, então passa por `OneHotEncoder`
   via `ColumnTransformer` — vira várias colunas binárias, uma por posição.
4. **Treina e mede.** `train_test_split` com `test_size=0.2` e `random_state=0`,
   `LinearRegression`, e o R² no conjunto de teste.
5. **Interpreta.** Imprime os coeficientes (`regressor.coef_`) e desenha os gráficos
   de dispersão de `Potencial` e `Media` contra o salário.

## O achado interessante: o modelo é legível

A parte que passa de "treina e mostra o score" é a leitura dos coeficientes. Depois
do one-hot a regressão tem 17 features, e o sinal e a magnitude de cada coeficiente
dizem em que direção aquele atributo empurra o salário. O maior peso positivo salvo
no notebook é `Feature: 6, Score: 33022.85982`; o maior negativo,
`Feature: 10, Score: -62080.98557`. Um modelo linear paga o preço de ser simples com
a vantagem de ser inspecionável — dá para apontar o que ele acha que vale dinheiro.

## Resultado registrado

Transcrito da saída salva no notebook, sem recalcular:

- **R² no conjunto de teste: `0.7089710245098851`** (`regressor.score(X_test, y_test)`,
  `test_size=0.2`, `random_state=0`).

## Como rodar

1. Abra `previsao_salario_FIFA.ipynb` no Google Colab ou em um Jupyter local.
2. Tenha instalados `pandas`, `numpy`, `matplotlib`, `seaborn` e `scikit-learn`.
3. Execute as células em ordem.

## Limitações conhecidas

- **O dataset não está versionado.** O notebook lê `sds_fifa.csv`, que não vem no
  repositório — sem ele, nada roda. Quem clonar precisa desse arquivo.
- **Sem validação além de uma divisão.** Um único `train_test_split`, sem validação
  cruzada nem repetição; o R² é de uma partição só.
- **Sem escalonamento das features numéricas** e sem tratamento de outliers — o
  modelo é a regressão linear direta sobre os dados codificados.

## Estrutura

```
.
├── previsao_salario_FIFA.ipynb   # pipeline completo, do CSV à interpretação
└── README.md
```
