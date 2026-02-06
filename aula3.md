# 📊 Aula 3 — Visualização de Dados

## 🎯 Objetivo da Aula
Nesta aula, aprendemos a **visualizar dados de forma gráfica** para explorar padrões, comparar grupos e comunicar informações de forma clara e intuitiva.

Serão utilizados diferentes tipos de gráficos para:
- Entender distribuições
- Comparar categorias
- Identificar outliers
- Comunicar resultados de forma visual

## 👀 Revisão da Base de Dados
Utilizamos a base já **limpa e preparada** nas aulas anteriores.

```python
df_limpo.head()
```

## 📊 Gráficos de Barras — Frequência de Categorias
Gráficos de barras são ideais para comparar quantidades entre categorias.

Conceitos:
Distribuição de variáveis categóricas

Comparação visual de frequências

Leitura rápida de padrões dominantes

df_limpo['senioridade'].value_counts().plot(
    kind='bar',
    title="Distribuição de senioridade"
)
##🎨 Visualização com Seaborn
O **Seaborn** é uma biblioteca baseada no Matplotlib que facilita a criação de gráficos estatísticos com melhor estética.

```python
import seaborn as sns
```
**Gráfico de barras com média salarial**
```python
sns.barplot(data=df_limpo, x='senioridade', y='usd')
```
Conceitos:
Agregação automática (média por padrão)

Comparação entre categorias

Relação entre variável categórica e numérica

## 🖼️ Personalização com Matplotlib
O Matplotlib permite controle fino sobre títulos, rótulos e tamanho dos gráficos.
```
import matplotlib.pyplot as plt
plt.figure(figsize=(8,5))
sns.barplot(data=df_limpo, x='senioridade', y='usd')
plt.title("Salário médio por senioridade")
plt.xlabel("Senioridade")
plt.ylabel("Salário médio anual (USD)")
plt.show()
```

## 📐 Agregações com GroupBy
Antes de visualizar, é comum calcular métricas agregadas.

Conceitos:
Agrupamento de dados

Cálculo de médias

Ordenação de resultados
```
df_limpo.groupby('senioridade')['usd'].mean().sort_values(ascending=False)
```

## 🔀 Ordenação Personalizada de Categorias
Controlar a ordem das categorias melhora a interpretação visual.

```
ordem = (
    df_limpo
    .groupby('senioridade')['usd']
    .mean()
    .sort_values(ascending=True)
    .index
)
plt.figure(figsize=(8,5))
sns.barplot(data=df_limpo, x='senioridade', y='usd', order=ordem)
plt.title("Salário médio por senioridade")
plt.xlabel("Senioridade")
plt.ylabel("Salário médio anual (USD)")
plt.show()
```


##📈 Histogramas — Distribuição de Valores
Histogramas mostram como os valores estão distribuídos ao longo de intervalos.

Conceitos:
Distribuição de dados

Assimetria

Concentração de valores

Densidade (KDE)

```
plt.figure(figsize=(10,5))
sns.histplot(df_limpo['usd'], bins=50, kde=True)
plt.title("Distribuição dos salários anuais")
plt.xlabel("Salário em USD")
plt.ylabel("Frequência")
plt.show()
```

## 📦 Boxplot — Identificação de Outliers
Boxplots são ideais para analisar dispersão e valores extremos.

Conceitos:
Mediana

Quartis

Amplitude interquartil

Outliers

```
plt.figure(figsize=(8,5))
sns.boxplot(x=df_limpo['usd'])
plt.title("Boxplot Salário")
plt.xlabel("Salário em USD")
plt.show()
```

## 📊 Boxplot por Categoria
Permite comparar a distribuição de uma variável numérica entre categorias.

```
ordem_senioridade = ['junior', 'pleno', 'senior', 'executivo']

plt.figure(figsize=(8,5))
sns.boxplot(
    x='senioridade',
    y='usd',
    data=df_limpo,
    order=ordem_senioridade
)
plt.title("Boxplot da distribuição por senioridade")
plt.xlabel("Salário em USD")
plt.show()
```
Boxplot com cores

```
plt.figure(figsize=(8,5))
sns.boxplot(
    x='senioridade',
    y='usd',
    data=df_limpo,
    order=ordem_senioridade,
    palette='Set2',
    hue='senioridade'
)
plt.title("Boxplot da distribuição por senioridade")
plt.xlabel("Salário em USD")
plt.show()
```

## 🌐 Visualizações Interativas com Plotly
O Plotly permite criar gráficos interativos, ideais para dashboards e apresentações.

```
import plotly.express as px
Gráfico de barras interativo
senioridade_media_salario = (
    df_limpo
    .groupby('senioridade')['usd']
    .mean()
    .sort_values(ascending=False)
    .reset_index()
)

fig = px.bar(
    senioridade_media_salario,
    x='senioridade',
    y='usd',
    title='Média Salarial por Senioridade',
    labels={
        'senioridade': 'Nível de Senioridade',
        'usd': 'Média Salarial Anual (USD)'
    }
)

fig.show()
```

##🥧 Gráficos de Pizza — Proporções
Gráficos de pizza mostram a participação relativa de cada categoria.

```
remoto_contagem = df_limpo['remoto'].value_counts().reset_index()
remoto_contagem.columns = ['tipo_trabalho', 'quantidade']
Pizza simples
fig = px.pie(
    remoto_contagem,
    names='tipo_trabalho',
    values='quantidade',
    title='Proporção dos tipos de trabalho'
)
fig.show()
Donut chart
fig = px.pie(
    remoto_contagem,
    names='tipo_trabalho',
    values='quantidade',
    title='Proporção dos tipos de trabalho',
    hole=0.5
)
fig.show()
Exibindo percentuais
fig.update_traces(textinfo='percent+label')
fig.show()
```


## 🗺️ Mapas — Visualização Geográfica
Mapas permitem analisar dados por localização geográfica.

Conceitos:
Dados espaciais

Códigos de países (ISO)

Visualização por intensidade de cor

```
pip install pycountry
import pycountry
Conversão de código ISO-2 para ISO-3
def iso2_to_iso3(code):
    try:
        return pycountry.countries.get(alpha_2=code).alpha_3
    except:
        return None

df_limpo['residencia_iso3'] = df_limpo['residencia'].apply(iso2_to_iso3)
Mapa coroplético
df_ds = df_limpo[df_limpo['cargo'] == 'Data Scientist']
media_ds_pais = (
    df_ds
    .groupby('residencia_iso3')['usd']
    .mean()
    .reset_index()
)

fig = px.choropleth(
    media_ds_pais,
    locations='residencia_iso3',
    color='usd',
    color_continuous_scale='rdylgn',
    title='Salário médio de Cientista de Dados por país',
    labels={
        'usd': 'Salário médio (USD)',
        'residencia_iso3': 'País'
    }
)

fig.show()
```

## 💾 Salvando o Resultado Final
Após limpeza, análise e visualização, é comum salvar a base final.

```
df_limpo.to_csv('dados-imersao-final.csv', index=False)
```

## ✅ Conclusão da Aula
Nesta aula aprendemos a:

Criar diferentes tipos de gráficos

Escolher visualizações adequadas para cada objetivo

Comparar categorias e distribuições

Identificar padrões e outliers

Criar gráficos interativos e mapas

A visualização de dados é uma das etapas mais importantes da análise, pois transforma números em insights compreensíveis e acionáveis.

