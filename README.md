# Lab 01 — SARIMA no varejo brasileiro (PMC/IBGE)

Esse é o lab da disciplina de **Financial Analytics** (Insper).

Acessível em: https://joaofelipefarias.github.io/financial-analytics/

A ideia do exercício foi pegar a série da **PMC** (Pesquisa Mensal do Comércio do IBGE). índice de volume de vendas no varejo ampliado, mensal, base 2022 = 100, e ajustar um SARIMA pra uma UF específica.

A UF que escolhi foi **Pernambuco (PE)**.

## O que tem aqui

- `index.ipynb` — o notebook com tudo: análise descritiva, testes, modelo, previsão e discussão.
- `index.html` — o mesmo notebook renderizado em HTML (é o que vai pro GitHub Pages).
- `requirements.txt` — as bibliotecas que usei.
- `.gitignore` — gitignore padrão;

## Como rodar

Eu usei `uv` pra gerenciar o ambiente. Se quiser reproduzir:

```bash
uv venv --python 3.11
uv pip install -r requirements.txt
.venv/bin/jupyter nbconvert --to notebook --execute index.ipynb --output index.ipynb
.venv/bin/jupyter nbconvert --to html index.ipynb --output index.html
```

Pra abrir no Jupyter normal:

```bash
.venv/bin/jupyter lab index.ipynb
```

## Resumo dos main takeaways

- A série de PE não é estacionária (ADF p ≈ 0,23, KPSS p ≤ 0,01). Tem tendência clara e sazonalidade anual bem visível (picos em nov/dez, vales no começo do ano).
- Depois de uma diferença regular já fica estacionária. O `ndiffs` confirma d = 1.
- O `nsdiffs` (CH/OCSB) deu D = 0, mas o **AutoARIMA** decidiu usar D = 1 mesmo, porque o AICc fica melhor.
- Modelo escolhido: **SARIMA(1,1,1)(0,1,2)[12]**.
- Os resíduos passaram no Ljung-Box em todos os lags (12, 24 e 36) → ok, parecem white noise.
- O Jarque-Bera rejeitou normalidade, mas isso é por causa do choque da pandemia em 2020, alguns outliers.
- Previsão de 24 meses captura bem a sazonalidade e os intervalos abrem de forma controlada.

A discussão completa (com as 5 perguntas respondidas) tá no final do notebook.

## Stack

- Python 3.11
- `pandas`, `numpy`, `matplotlib`
- `statsmodels` (ADF, KPSS, STL, ACF/PACF, Ljung-Box)
- `pmdarima` (`ndiffs`, `nsdiffs`)
- `statsforecast` (`AutoARIMA`)

## Link do relatório

Configurei o GitHub Pages pra servir o `index.html` direto da raiz.
