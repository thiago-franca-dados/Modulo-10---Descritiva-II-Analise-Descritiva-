 # 💰 Análise Exploratória e Estabilidade Temporal — Previsão de Renda

Este projeto reúne uma Análise Exploratória de Dados (EDA) completa sobre o perfil socioeconômico de 15.000 clientes de uma instituição financeira (`previsao_de_renda.csv`). O objetivo é investigar variáveis quantitativas, qualitativas e a estabilidade temporal da base para embasar a construção de um modelo preditivo de renda (visando dimensionar limites de cartão de crédito sem atrito documental).

---

## 🛠️ Como o Projeto foi Elaborado

### 📌 Etapa 1: Variáveis Quantitativas, Outliers e Transformação Logarítmica
1. **Limpeza e Seleção:** Removi colunas identificadoras sem valor preditivo (`Unnamed: 0` e `index`) via `.drop()` e isolei as variáveis numéricas utilizando `select_dtypes(include=['number'])`.
2. **Análise de Correlação (`Pairplot` e `Heatmap`):** Plotei uma matriz de dispersão (`sns.pairplot`) e um mapa de calor (`.corr()` + `sns.heatmap`) para identificar padrões lineares e não lineares, destacando **`tempo_emprego`** e **`idade`** como os principais fatores quantitativos correlacionados com a `renda`.
3. **Investigação de Outliers (`Scatterplot` e `Boxplot`):** Mapeei a variável ordinal `educacao` em uma escala numérica (`1` a `5`) e construí um `sns.boxplot` ordenado por escolaridade, evidenciando forte assimetria à direita e concentração de **outliers superiores** na variável `renda`.
4. **Transformação Logarítmica (`np.log`):** Criei uma cópia da base (`df_log`) aplicando o logaritmo natural sobre a renda (`np.log(df['renda'])`) e repeti os gráficos de dispersão e boxplot, comprovando a redução do impacto visual dos outliers e a melhora na simetria da distribuição.

---

### 📌 Etapa 2: Variáveis Qualitativas e Significância Estatística
1. **Seleção de Atributos:** Filtrei o conjunto de dados mantendo a variável resposta (`renda`) e as variáveis booleanas **`posse_de_imovel`** e **`posse_de_veiculo`**.
2. **Avaliação Individual com Intervalos de Confiança (`sns.barplot`):** Plotei gráficos de barras individuais com intervalo de confiança de 90% (`errorbar=('ci', 90)`). A ausência de sobreposição entre as barras de erro de quem possui (`True`) e não possui (`False`) o bem permitiu verificar visualmente que ambas discriminam a renda de forma estatisticamente relevante.
3. **Análise Conjunta e Interação (`hue`):** Combinei `posse_de_veiculo` e `posse_de_imovel` em um único gráfico utilizando o parâmetro `hue` (com legenda externa via `bbox_to_anchor`), permitindo comparar o efeito combinado dos dois bens sobre a renda média dos clientes.

---

### 📌 Etapa 3: Análise de Estabilidade Temporal (*Data Drift*)
1. **Volume e Distribuição no Tempo (`sns.countplot`):** Selecionei a coluna temporal `data_ref` e criei um laço `for` para plotar automaticamente a contagem mensal de todas as variáveis qualitativas (`sexo`, `posse_de_veiculo`, `posse_de_imovel`, `tipo_renda`, `educacao`, `estado_civil` e `tipo_residencia`). Ao detectar uma quebra abrupta na variável **`educacao`**, isolei-a em um gráfico dedicado para investigar a anomalia mais de perto.
2. **Estabilidade das Médias de Renda (`sns.pointplot`):** Filtrei as colunas categóricas (`select_dtypes(include='object')`) e utilizei um segundo laço `for` gerando gráficos de pontos (`sns.pointplot`) com intervalo de confiança de 95% (`errorbar=('ci', 95)`), acompanhando mês a mês a evolução da renda média e a amplitude das margens de erro por categoria.

---

## 💡 Principais Insights Encontrados
* **Fatores Quantitativos:** O `tempo_emprego` é a variável numérica com maior correlação positiva com a renda do cliente.
* **Composição Familiar:** Há forte correlação positiva entre `qtd_filhos` e `qt_pessoas_residencia`, além de relação inversa com a `idade` (clientes mais velhos tendem a ter menos dependentes morando na mesma residência).
* **Efeito do Logaritmo:** A escala logarítmica suavizou a distorção causada por rendas muito elevadas, tornando as diferenças entre faixas de escolaridade muito mais nítidas para modelagem linear.
* **Poder de Discriminação Patrimonial:** Tanto a posse de veículo quanto a posse de imóvel apresentam diferenças estatisticamente significativas na média de renda, sendo a **posse de veículo** o fator que provoca maior salto na renda média quando analisadas em conjunto.
* **Quebra Estrutural em `educacao` (*Data Drift*):** Entre agosto (`2015-08-01`) e setembro (`2015-09-01`), a base sofreu uma inversão brusca no volume de clientes entre os níveis `Secundário` e `Superior completo`, indicando uma possível mudança no processo de coleta/cadastro que exige atenção antes de utilizar essa variável em produção.
* **Estabilidade das Médias:** As médias de renda por categoria mantêm uma ordenação consistente ao longo dos meses, embora grupos com menor representatividade amostral (como `Pós graduação` e `Estudante`) apresentem intervalos de confiança mais amplos.

---

## 💻 Tecnologias Utilizadas
* **Python 3**
* **Pandas & NumPy** (limpeza, filtragem, mapeamento ordinal e transformação logarítmica)
* **Seaborn & Matplotlib** (`pairplot`, `heatmap`, `scatterplot`, `boxplot`, `barplot`, `countplot` e `pointplot`)
