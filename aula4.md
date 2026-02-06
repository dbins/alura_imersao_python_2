# 📊 Aula 4 — Construindo um Dashboard com Streamlit

## 🎯 Objetivo da Aula
Nesta aula, o objetivo é aprender a **construir e disponibilizar um dashboard interativo** utilizando a biblioteca **Streamlit**, permitindo:
- Visualização dinâmica de dados
- Aplicação de filtros interativos
- Exibição de métricas (KPIs)
- Geração de gráficos interativos
- Publicação do projeto na web

---

## 🚀 O que é o Streamlit?
O **Streamlit** é uma biblioteca Python que permite criar **aplicações web interativas para análise de dados** de forma simples, rápida e com pouco código.

### Conceitos:
- Transformar scripts Python em apps web
- Criação de dashboards sem frontend tradicional
- Atualização automática baseada em interação do usuário

---

## 🌐 Exemplo de Dashboard Publicado
Dashboard final publicado no Streamlit Cloud:  
🔗 https://dashboard-salarios-dados.streamlit.app/

---

## 🧪 Criação do Ambiente Virtual
O uso de ambientes virtuais garante **isolamento de dependências** do projeto.

### Criar o ambiente virtual
```bash
python3 -m venv .venv
```
Ativar o ambiente virtual
Windows
```
.venv\Scripts\Activate
```
Mac / Linux
```
source .venv/bin/activate
```
## 📦 Gerenciamento de Dependências
Para garantir reprodutibilidade, utilizamos um arquivo requirements.txt.
```
pandas==2.2.3
streamlit==1.44.1
plotly==5.24.1
```

Instalação das bibliotecas
```
pip install -r requirements.txt
```

## ⚙️ Estrutura Básica do App Streamlit
Um app Streamlit é executado como um script Python comum, mas com comandos específicos para interface.
```
import streamlit as st
import pandas as pd
import plotly.express as px
```

## 🧭 Configuração da Página
Define informações gerais da aplicação.

Conceitos:
Título do app

Ícone

Layout responsivo
```
st.set_page_config(
    page_title="Dashboard de Salários na Área de Dados",
    page_icon="📊",
    layout="wide",
)
```

## 📥 Carregamento dos Dados
O dashboard consome a base de dados já limpa e preparada.

```
df = pd.read_csv(
    "https://raw.githubusercontent.com/vqrca/dashboard_salarios_dados/refs/heads/main/dados-imersao-final.csv"
)```

## 🧩 Barra Lateral (Sidebar)
A sidebar é usada para controles de filtro.

Conceitos:
Interação do usuário

Filtragem dinâmica

Atualização automática do dashboard
```
st.sidebar.header("🔍 Filtros")
```

## 🎛️ Filtros Interativos
Filtros permitem que o usuário explore subconjuntos dos dados.

Conceitos:
Multiseleção

Filtros categóricos

Filtros temporais
```
anos_disponiveis = sorted(df['ano'].unique())
anos_selecionados = st.sidebar.multiselect(
    "Ano", anos_disponiveis, default=anos_disponiveis
)
senioridades_disponiveis = sorted(df['senioridade'].unique())
senioridades_selecionadas = st.sidebar.multiselect(
    "Senioridade", senioridades_disponiveis, default=senioridades_disponiveis
)
contratos_disponiveis = sorted(df['contrato'].unique())
contratos_selecionados = st.sidebar.multiselect(
    "Tipo de Contrato", contratos_disponiveis, default=contratos_disponiveis
)
tamanhos_disponiveis = sorted(df['tamanho_empresa'].unique())
tamanhos_selecionados = st.sidebar.multiselect(
    "Tamanho da Empresa", tamanhos_disponiveis, default=tamanhos_disponiveis
)
```

## 🔎 Filtragem do DataFrame
Os filtros da interface são aplicados diretamente ao DataFrame.

Conceitos:
Máscaras booleanas

Encadeamento de condições

Atualização reativa

```
df_filtrado = df[
    (df['ano'].isin(anos_selecionados)) &
    (df['senioridade'].isin(senioridades_selecionadas)) &
    (df['contrato'].isin(contratos_selecionados)) &
    (df['tamanho_empresa'].isin(tamanhos_selecionados))
]
```

## 🧾 Conteúdo Principal do Dashboard
Textos e títulos ajudam a contextualizar a análise.
```
st.title("🎲 Dashboard de Análise de Salários na Área de Dados")
st.markdown(
    "Explore os dados salariais na área de dados nos últimos anos. "
    "Utilize os filtros à esquerda para refinar sua análise."
)```

## 📌 Métricas Principais (KPIs)
KPIs fornecem resumo rápido dos dados filtrados.

Conceitos:
Indicadores-chave

Agregações estatísticas

Comunicação executiva
```
if not df_filtrado.empty:
    salario_medio = df_filtrado['usd'].mean()
    salario_maximo = df_filtrado['usd'].max()
    total_registros = df_filtrado.shape[0]
    cargo_mais_frequente = df_filtrado["cargo"].mode()[0]
col1, col2, col3, col4 = st.columns(4)

col1.metric("Salário médio", f"${salario_medio:,.0f}")
col2.metric("Salário máximo", f"${salario_maximo:,.0f}")
col3.metric("Total de registros", f"{total_registros:,}")
col4.metric("Cargo mais frequente", cargo_mais_frequente)
```

## 📊 Gráficos Interativos com Plotly
Os gráficos são organizados em colunas para melhor layout visual.
```
col_graf1, col_graf2 = st.columns(2)
```

## 📈 Top 10 Cargos por Salário Médio
```
top_cargos = (
    df_filtrado
    .groupby('cargo')['usd']
    .mean()
    .nlargest(10)
    .sort_values(ascending=True)
    .reset_index()
)
grafico_cargos = px.bar(
    top_cargos,
    x='usd',
    y='cargo',
    orientation='h',
    title="Top 10 cargos por salário médio",
    labels={'usd': 'Média salarial anual (USD)', 'cargo': ''}
)
st.plotly_chart(grafico_cargos, use_container_width=True)
```

## 📉 Distribuição Salarial
```
grafico_hist = px.histogram(
    df_filtrado,
    x='usd',
    nbins=30,
    title="Distribuição de salários anuais",
    labels={'usd': 'Faixa salarial (USD)', 'count': ''}
)
st.plotly_chart(grafico_hist, use_container_width=True)
```

## 🥧 Proporção dos Tipos de Trabalho
```
remoto_contagem = df_filtrado['remoto'].value_counts().reset_index()
remoto_contagem.columns = ['tipo_trabalho', 'quantidade']
grafico_remoto = px.pie(
    remoto_contagem,
    names='tipo_trabalho',
    values='quantidade',
    title='Proporção dos tipos de trabalho',
    hole=0.5
)
grafico_remoto.update_traces(textinfo='percent+label')
st.plotly_chart(grafico_remoto, use_container_width=True)
🗺️ Mapa de Salários por País
df_ds = df_filtrado[df_filtrado['cargo'] == 'Data Scientist']
media_ds_pais = df_ds.groupby('residencia_iso3')['usd'].mean().reset_index()
grafico_paises = px.choropleth(
    media_ds_pais,
    locations='residencia_iso3',
    color='usd',
    color_continuous_scale='rdylgn',
    title='Salário médio de Cientista de Dados por país',
    labels={'usd': 'Salário médio (USD)', 'residencia_iso3': 'País'}
)
st.plotly_chart(grafico_paises, use_container_width=True)
```

## 📋 Tabela de Dados Detalhados
Permite visualizar os dados filtrados em formato tabular.
```
st.subheader("Dados Detalhados")
st.dataframe(df_filtrado)
```
## ☁️ Deploy com Streamlit Cloud
O Streamlit Cloud permite publicar o dashboard gratuitamente.

Conceitos:
Deploy contínuo via GitHub

Execução em nuvem

Compartilhamento por URL

🔗 https://streamlit.io/cloud

## ✅ Conclusão da Aula
Nesta aula aprendemos a:

Criar dashboards interativos com Streamlit

Aplicar filtros dinâmicos

Exibir métricas e gráficos

Integrar Plotly com Streamlit

Publicar aplicações de dados na web

Essa etapa fecha o ciclo da análise, transformando dados e gráficos em uma ferramenta interativa de tomada de decisão.