# 💰 Análise Exploratória de Dados — Previsão de Renda

Este projeto realiza uma Análise Exploratória de Dados (EDA) sobre o perfil socioeconômico de 15.000 clientes de uma instituição financeira (`previsao_de_renda.csv`). O objetivo é entender o comportamento da variável **renda** e identificar padrões estatísticos que auxiliem na construção de modelos preditivos para definição de limite de crédito.

---

## 🛠️ Como o Projeto foi Elaborado

### 1. Limpeza e Seleção de Variáveis
* **Remoção de Identificadores:** Eliminei colunas sem valor preditivo (`Unnamed: 0` e `index`) via `.drop()` para evitar ruídos nas métricas estatísticas.
* **Filtro Automático:** Utilizei `select_dtypes(include=['number'])` para isolar as variáveis quantitativas antes de gerar as matrizes de análise.

### 2. Análise de Correlação (`Pairplot` e `Heatmap`)
* **Visão Geral:** Plotei uma matriz de dispersão (`sns.pairplot`) para observar visualmente a distribuição conjunta e a linearidade entre as variáveis numéricas.
* **Quantificação:** Construí um mapa de calor (`sns.heatmap`) a partir de `.corr()` para medir a intensidade das relações, identificando **`tempo_emprego`** e **`idade`** como as variáveis numéricas mais correlacionadas com a `renda`.

### 3. Análise Bivariada, Outliers e Engenharia de Atributos
* **Gráficos de Dispersão:** Investiguei detalhadamente a relação de `renda` com `tempo_emprego` e `idade` por meio de `sns.scatterplot`.
* **Mapeamento Ordinal e Distribuição:** Converti a variável categórica `educacao` em uma escala numérica ordinal (`1` a `5`) usando `.map()` para calcular sua correlação e gerei um `sns.boxplot` ordenado por nível de escolaridade, evidenciando forte assimetria à direita e presença de **outliers superiores** na renda.

### 4. Transformação Logarítmica (`np.log`)
* **Tratamento de Assimetria:** Criei uma cópia segura da base (`df_log`) e apliquei o logaritmo natural (`np.log(df['renda'])`) para comprimir a escala dos valores extremos.
* **Reavaliação Visual:** Repeti os gráficos de dispersão e o boxplot por escolaridade utilizando `log_renda`, comprovando a melhoria na simetria e na legibilidade dos dados para futuras etapas de modelagem preditiva.

---

## 💡 Principais Insights Encontrados
* **Fator Principal:** O `tempo_emprego` é a variável quantitativa que apresenta maior correlação positiva com a renda do cliente.
* **Composição Familiar:** Existe forte correlação positiva entre `qtd_filhos` e `qt_pessoas_residencia`, além de relação inversa com a `idade` (clientes mais velhos tendem a ter menos dependentes morando na mesma residência).
* **Impacto do Logaritmo:** A transformação logarítmica suavizou significativamente o efeito visual dos outliers na renda — especialmente visível na comparação por faixas de `educacao` — tornando a distribuição mais adequada para algoritmos lineares.

---

## 💻 Tecnologias Utilizadas
* **Python 3**
* **Pandas & NumPy** (manipulação de dados, mapeamento e transformação logarítmica)
* **Seaborn & Matplotlib** (pairplot, heatmap, scatterplot e boxplot)
