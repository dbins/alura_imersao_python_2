# 🧹 Aula 2 — Preparação e Limpeza de Dados

## 🎯 Objetivo da Aula
Nesta aula, o foco é aprender como **identificar, analisar e tratar problemas comuns em bases de dados**, especialmente:
- Valores ausentes (nulos)
- Estratégias de preenchimento
- Remoção de dados incompletos
- Ajuste de tipos de dados

Essas etapas são fundamentais para garantir **qualidade e confiabilidade** nas análises.

---

## ❓ Valores Nulos (Missing Values)
Valores nulos representam **informações ausentes** e são comuns em bases reais.  
Eles podem surgir por falhas de coleta, integração de sistemas ou ausência de resposta.

### Conceitos:
- Identificação de dados faltantes
- Representação de valores nulos (`NaN`)
- Impacto dos nulos nas análises estatísticas

```python
df.isnull()
```

### 🔍 Inspeção Visual da Base
Após identificar valores nulos, é importante revisar a estrutura geral do DataFrame.

```
df.head()
```

### 🔢 Quantidade de Valores Nulos por Coluna
Nem todo valor ausente tem o mesmo impacto. Saber onde e quanto está faltando é essencial.

Conceitos:
Contagem de valores ausentes

Identificação de colunas problemáticas

Decisão de estratégia de tratamento

```
df.isnull().sum()
```

### 🗓️ Análise de Valores Únicos
Verificar os valores únicos ajuda a identificar inconsistências e entender melhor os dados.

Conceitos:
Domínio dos dados

Presença de valores inesperados

Identificação indireta de problemas

```
df['ano'].unique()
```

### 🧾 Identificação de Registros com Dados Ausentes
Em alguns casos, é útil visualizar quais linhas contêm valores nulos.

Conceitos:
Filtragem condicional

Análise de registros incompletos

Avaliação do impacto da remoção ou correção

```
df[df.isnull().any(axis=1)]
```

### 🧮 Estratégias de Preenchimento de Valores Nulos (Imputação)
Nem sempre remover dados é a melhor opção. Muitas vezes, é possível substituir valores ausentes por estimativas.

Conceitos:
Imputação por média

Imputação por mediana

Diferença entre média e mediana em dados com outliers

```
import numpy as np

df_salarios = pd.DataFrame({
    'nome': ["Ana", "Bruno", "Carlos", "Daniele", "Val"],
    'salario': [4000, np.nan, 5000, np.nan, 100000]
})
```
Preenchimento pela média
```
df_salarios['salario_media'] = df_salarios['salario'].fillna(
    df_salarios['salario'].mean().round(2)
)
```
Preenchimento pela mediana
```
df_salarios['salario_mediana'] = df_salarios['salario'].fillna(
    df_salarios['salario'].median()
)
df_salarios
```

### ⏭️ Preenchimento por Propagação de Valores
Em dados sequenciais (como séries temporais), faz sentido usar valores vizinhos.

Forward Fill (propaga o valor anterior)
```
df_temperaturas = pd.DataFrame({
    "Dia": ["Segunda", "Terça", "Quarta", "Quinta", "Sexta"],
    "Temperatura": [30, np.nan, np.nan, 28, 27]
})

df_temperaturas["preenchido_ffill"] = df_temperaturas["Temperatura"].ffill()
df_temperaturas
```

Backward Fill (usa o próximo valor válido)
```
df_temperaturas["preenchido_bfill"] = df_temperaturas["Temperatura"].bfill()
df_temperaturas
```
Conceitos:
Preenchimento contextual

Uso em séries temporais

Dependência da ordem dos dados

🏷️ Preenchimento de Valores Categóricos
Para variáveis categóricas, é comum substituir valores ausentes por rótulos explícitos.

Conceitos:
Evitar perda de registros

Manter coerência semântica

Facilitar análises futuras
```
df_cidades = pd.DataFrame({
    'nome': ["Ana", "Bruno", "Carlos", "Daniele", "Val"],
    'cidade': ["São Paulo", np.nan, "Curitiba", np.nan, "Belém"]
})

df_cidades['cidade_preenchida'] = df_cidades["cidade"].fillna("Não informado")
df_cidades
```

### 🗑️ Remoção de Registros com Valores Nulos
Quando a quantidade de dados ausentes é pequena, uma abordagem simples é removê-los.

Conceitos:
Eliminação de registros incompletos

Avaliação do impacto no volume de dados

Trade-off entre quantidade e qualidade

```
df_limpo = df.dropna()
df_limpo.isnull().sum()
```


### 🔍 Verificação da Base Após Limpeza
Após a limpeza, é importante confirmar se a base está consistente.
```
df_limpo.head()
df_limpo.info()
```

### 🔄 Ajuste de Tipos de Dados
Dados podem ser carregados com tipos inadequados. Ajustá-los melhora desempenho e clareza.

Conceitos:
Conversão de tipos

Diferença entre tipos numéricos

Preparação para análises estatísticas e visuais

```
df_limpo = df_limpo.assign(
    ano=df_limpo['ano'].astype('int64')
)
```

### ✅ Conclusão da Aula
Nesta aula aprendemos a:

Identificar valores ausentes

Avaliar seu impacto na base

Aplicar diferentes estratégias de tratamento

Remover registros incompletos quando necessário

Ajustar tipos de dados

A limpeza e preparação dos dados é uma das etapas mais importantes da análise, pois garante que as conclusões obtidas nas próximas fases sejam confiáveis e representativas.

