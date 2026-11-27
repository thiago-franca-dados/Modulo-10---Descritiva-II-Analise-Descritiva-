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




# 💰 Análise Exploratória de Dados — Previsão de Renda

Este projeto apresenta uma Análise Exploratória de Dados (EDA) completa sobre o perfil socioeconômico de 15.000 clientes de uma instituição financeira (`previsao_de_renda.csv`). O objetivo é investigar como variáveis quantitativas e qualitativas se relacionam com a **renda**, gerando insumos para a construção de modelos preditivos de limite de crédito.

---

## 🛠️ Como o Projeto foi Elaborado

### 📌 Etapa 1: Variáveis Quantitativas, Outliers e Transformação Logarítmica
1. **Limpeza e Filtragem:** Removi colunas identificadoras sem valor preditivo (`Unnamed: 0` e `index`) via `.drop()` e isolei as variáveis numéricas com `select_dtypes(include=['number'])`.
2. **Análise de Correlação (`Pairplot` e `Heatmap`):** Gerei uma matriz de dispersão (`sns.pairplot`) e um mapa de calor (`.corr()` + `sns.heatmap`) para identificar padrões lineares e não lineares, destacando **`tempo_emprego`** e **`idade`** como os principais fatores quantitativos correlacionados à `renda`.
3. **Investigação de Outliers (`Scatterplot` e `Boxplot`):** Mapeei a variável ordinal `educacao` em escala numérica (`1` a `5`) e plotei um `sns.boxplot` ordenado por escolaridade, identificando forte assimetria à direita e concentração de **outliers superiores** na renda.
4. **Transformação Logarítmica (`np.log`):** Apliquei o logaritmo natural sobre a `renda` (`df_log['log_renda']`) e repeti as análises gráficas, comprovando a redução do impacto visual dos outliers e a melhora na simetria da distribuição.

---

### 📌 Etapa 2: Variáveis Qualitativas e Significância Estatística
1. **Seleção de Atributos:** Filtrei o conjunto de dados mantendo a variável resposta (`renda`) e as variáveis qualitativas booleanas **`posse_de_imovel`** e **`posse_de_veiculo`**.
2. **Avaliação Individual com Intervalos de Confiança (`sns.barplot`):** Plotei gráficos de barras individuais com intervalo de confiança de 90% (`errorbar=('ci', 90)`). A ausência de sobreposição entre as barras de erro de quem possui (`True`) e não possui (`False`) o bem permitiu verificar visualmente que ambas as variáveis discriminam a renda de forma estatisticamente relevante.
3. **Análise Conjunta e Interação (`hue`):** Combinei `posse_de_veiculo` e `posse_de_imovel` em um único gráfico utilizando o parâmetro `hue` (com ajuste externo da legenda via `bbox_to_anchor`), permitindo comparar o efeito combinado dos dois bens sobre a renda média dos clientes.

---

## 💡 Principais Insights Encontrados
* **Fatores Quantitativos:** O `tempo_emprego` apresenta a maior correlação positiva com a renda entre as variáveis numéricas analisadas.
* **Composição Familiar:** Há forte correlação positiva entre `qtd_filhos` e `qt_pessoas_residencia`, além de relação inversa com a `idade` (clientes mais velhos tendem a ter menos dependentes morando na mesma residência).
* **Efeito do Logaritmo:** A escala logarítmica suavizou a distorção causada por rendas muito elevadas, tornando as diferenças entre faixas de escolaridade muito mais nítidas.
* **Poder de Discriminação Patrimonial:** Tanto a posse de veículo quanto a posse de imóvel apresentam diferenças estatisticamente significativas na média de renda, sendo a **posse de veículo** o fator que provoca maior salto na renda média quando analisadas em conjunto.

---

## 💻 Tecnologias Utilizadas
* **Python 3**
* **Pandas & NumPy** (limpeza, filtragem, mapeamento ordinal e transformação logarítmica)
* **Seaborn & Matplotlib** (`pairplot`, `heatmap`, `scatterplot`, `boxplot` e `barplot` com intervalos de confiança)
