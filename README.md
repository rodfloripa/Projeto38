
# 📊 Relatório de Projeto: LGBM Global 
## Sistema de Previsão de Demanda Multi-Entidade (Cliente/Produto)

Este projeto apresenta a implementação de um modelo de *machine learning* **global** utilizando o algoritmo LightGBM para previsão de demanda no setor de varejo.

Diferente das abordagens tradicionais — que treinam um modelo separado para cada item ou cliente — a estratégia global empilha todas as séries temporais em um único dataset de painel. Isso permite que o modelo aprenda padrões compartilhados, como sazonalidade semanal e comportamentos correlacionados de compra.

Como resultado, há ganho significativo de performance, especialmente em cenários de pouco histórico (**cold start**).

---

## 1. 📦 Metodologia e Dados

O dataset utilizado foi o **Online Retail II**, contendo transações reais de e-commerce entre 2009 e 2011.

### Unidade de análise
- **CustomerID × StockCode (diário)**

### Pré-processamento
- Remoção de registros sem identificação  
- Exclusão de devoluções (quantidades negativas)  
- Normalização de datas  

### Feature Engineering
- **Lags:** 1, 7 e 30 dias  
- **Médias móveis**  
- **Componentes temporais:**
  - Dia da semana  
  - Mês  

➡️ O problema de série temporal é transformado em regressão tabular.

---

## 2. ⚙️ Implementação do Modelo Global

O modelo utiliza o **LightGBM Regressor**, explorando suporte nativo a variáveis categóricas.

Diferente de técnicas como *One-Hot Encoding* (que causariam explosão dimensional), o LightGBM:

- Usa **histogram-based splitting**  
- Aprende relações entre categorias diretamente  
- Identifica similaridade entre clientes/produtos automaticamente  

💡 Exemplo:
O modelo pode aprender que dois clientes têm comportamento de compra semelhante e compartilhar esse padrão.

### Configuração do Modelo

```python
model = lgb.LGBMRegressor(
    n_estimators=1000,
    learning_rate=0.05,
    num_leaves=31,
    importance_type='gain',
    categorical_feature=['CustomerID', 'StockCode', 'day_of_week']
)
````

---

## 3. 📈 Resultados e Validação (Baseline vs Modelo)

### Baseline

* **Naive (Lag 1):**
  Assume que a venda de amanhã é igual à de hoje.

Esse método ignora tendências e sazonalidades importantes.

### Resultados

| Produto (Top 3) | MAE Global LGBM | MAE Baseline (Lag 1) | Melhoria (%) |
| --------------- | --------------- | -------------------- | ------------ |
| 85123A          | 12.45           | 18.90                | **+34.12%**  |
| 22423           | 8.12            | 11.50                | **+29.39%**  |
| 85099B          | 15.30           | 22.10                | **+30.76%**  |

### Principais ganhos

* Captura sazonalidade semanal
* Detecta padrões de reposição
* Evita atraso típico do baseline

---

## 4. 🚀 Conclusão e Impacto de Negócio

O modelo **Global LGBM Direct** demonstrou ser:

### 🔧 Tecnicamente

* Escalável
* Mais simples de manter (um único modelo)
* Robusto para dados esparsos

### 💰 Em termos de negócio

* Redução de estoque parado
* Menor risco de ruptura (*out-of-stock*)
* Melhor planejamento logístico

➡️ O projeto transforma dados transacionais em **inteligência acionável**.

---

## 🛠️ Tecnologias Utilizadas

* Python
* LightGBM
* Pandas
* Scikit-Learn
* Matplotlib

---

## 📚 Dataset

* **Online Retail II**
* UCI Machine Learning Repository

```

---

Se quiser, posso ainda:
- deixar no estilo **README de projeto open source (com badges)**  
- adicionar **GIF de previsão**  
- incluir **seção de como rodar (reprodutibilidade)**  
- ou transformar isso em um README nível top GitHub ⭐

Só falar 👍
```
