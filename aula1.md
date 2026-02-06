# 📊 Aula 1 — Análise de Dados com Pandas

## 🎯 Objetivo da Aula
Apresentar o fluxo inicial de **análise exploratória de dados** com Pandas, abordando:
- Importação de dados
- Estrutura do DataFrame
- Estatísticas descritivas
- Análise de variáveis categóricas
- Limpeza e padronização de dados

---

## 🐼 Introdução ao Pandas
O Pandas é uma biblioteca do Python voltada para **manipulação e análise de dados estruturados**.  
Seu principal objeto é o **DataFrame**, uma estrutura bidimensional composta por linhas e colunas.

```python
import pandas as pd
```
## 📥 Carregamento da Base de Dados
Os dados são carregados a partir de um arquivo CSV hospedado online.

Conceitos:
Leitura de dados tabulares

Criação de um DataFrame

Uso de fontes externas (URLs)

```
df = pd.read_csv(
    "https://raw.githubusercontent.com/guilhermeonrails/data-jobs/refs/heads/main/salaries.csv"
)```

## 👀 Visualização Inicial dos Dados
Antes de qualquer análise, é importante observar uma amostra dos dados.

Conceitos:
Inspeção visual

Identificação inicial das variáveis

Compreensão do conteúdo das colunas
```
df.head()
```
## 🧱 Estrutura do DataFrame
Aqui analisamos como o DataFrame está organizado internamente.

Conceitos:
Tipos de dados (numéricos e categóricos)

Quantidade de valores não nulos

Uso de memória

Qualidade geral da base
```
df.info()
```
## 🔢 Estatísticas Descritivas (Variáveis Numéricas)
A estatística descritiva resume o comportamento das variáveis numéricas.

Conceitos:
Média e mediana

Dispersão (desvio padrão)

Valores mínimos e máximos

Quartis

```
df.describe()
```
## 📐 Dimensão da Base de Dados
Saber o tamanho da base é essencial para planejar análises futuras.

Conceitos:
Número de observações (linhas)

Número de atributos (colunas)
```
df.shape
linhas, colunas = df.shape[0], df.shape[1]
print('Linhas:', linhas)
print('Colunas:', colunas)
```

## 🏷️ Nomes das Colunas
Colunas com nomes claros facilitam a leitura, análise e comunicação dos resultados.

Conceitos:
Padronização de nomenclatura

Clareza semântica

Adequação ao idioma do projeto
```
df.columns
Renomeando colunas
novos_nomes = {
    'work_year': 'ano',
    'experience_level': 'senioridade',
    'employment_type': 'contrato',
    'job_title': 'cargo',
    'salary': 'salario',
    'salary_currency': 'moeda',
    'salary_in_usd': 'usd',
    'employee_residence': 'residencia',
    'remote_ratio': 'remoto',
    'company_location': 'empresa',
    'company_size': 'tamanho_empresa'
}

df.rename(columns=novos_nomes, inplace=True)
df.head()
```

## 🧩 Variáveis Categóricas
Variáveis categóricas representam classificações ou grupos, e não valores numéricos contínuos.

Conceitos:
Frequência de categorias

Distribuição dos dados

Identificação de padrões dominantes

Nível de senioridade
```
df['senioridade'].value_counts()
```
Tipo de contrato
```
df['contrato'].value_counts()
```
Regime de trabalho
```
df['remoto'].value_counts()
```
Tamanho da empresa
```
df['tamanho_empresa'].value_counts()
```

## 🧼 Padronização de Categorias
Siglas dificultam a interpretação dos dados. Substituí-las melhora a legibilidade e a análise.

Conceitos:
Limpeza de dados categóricos

Padronização textual

Preparação para visualização e relatórios

Senioridade
```
senioridade = {
    'SE': 'senior',
    'MI': 'pleno',
    'EN': 'junior',
    'EX': 'executivo'
}
df['senioridade'] = df['senioridade'].replace(senioridade)
```
Tipo de contrato
```
contrato = {
    'FT': 'integral',
    'PT': 'parcial',
    'CT': 'contrato',
    'FL': 'freelancer'
}
df['contrato'] = df['contrato'].replace(contrato)
```
Tamanho da empresa
```
tamanho_empresa = {
    'L': 'grande',
    'M': 'media',
    'S': 'pequena'
}
df['tamanho_empresa'] = df['tamanho_empresa'].replace(tamanho_empresa)
```
Regime de trabalho
```
mapa_trabalho = {
    0: 'presencial',
    100: 'remoto',
    50: 'hibrido'
}
df['remoto'] = df['remoto'].replace(mapa_trabalho)
df.head()
```

## 🧾 Resumo das Variáveis Categóricas
Também é possível gerar estatísticas descritivas específicas para colunas categóricas.

Conceitos:
Número de categorias únicas

Categoria mais frequente

Frequência da moda
```
df.describe(include='object')
```

##✅ Conclusão da Aula
Nesta aula, percorremos as etapas iniciais da análise de dados:

Importação e inspeção da base

Entendimento da estrutura

Estatísticas descritivas

Análise e padronização de variáveis categóricas

Esses passos são fundamentais para garantir consistência, clareza e confiabilidade nas análises que serão feitas nas próximas aulas.