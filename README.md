# Limpeza e Enriquecimento de Dados — IMDb

Projeto de tratamento de dados em Python/Pandas usando um dataset real e "sujo" do IMDb, com enriquecimento automático de valores faltantes via API externa (OMDb).

Desenvolvido como exercício prático de Data Cleaning, com foco em lidar com problemas reais de qualidade de dado, como valores ausentes, inconsistências, e complementação de informação faltante usando fonte externa.

## Objetivo do projeto:

Tratar um dataset de filmes do IMDb com sujeira real (não simulada), aplicando técnicas de limpeza em Pandas e resolvendo um problema comum em dados reais: **valores faltantes que podem ser recuperados de uma fonte externa confiável**, em vez de simplesmente descartados ou imputados genericamente.

## O que foi feito

- Diagnóstico de qualidade dos dados (valores nulos, duplicatas, tipos inconsistentes)
- Tratamento de dados ausentes e padronização de colunas
- **Enriquecimento de dados via API:** para registros com data de lançamento ausente, o pipeline consulta a [OMDb API](https://www.omdbapi.com/) usando o ID do IMDb (`imdbID`) de cada filme, e preenche automaticamente apenas os valores faltantes, sem alterar os registros que já estavam completos
- Conversão e padronização de tipos de dado (datas, texto)

## Tecnologias utilizadas

- **Python 3**
- **Pandas** — limpeza, transformação e tratamento dos dados
- **Requests** — consumo da API OMDb
- **python-dotenv** — gerenciamento seguro da chave de API

## Estrutura do projeto

```
Limpeza_dados-Pandas/
├── Messy_data/           # Dataset original, com os dados sujos
├── Cleaned_data/         # Dataset final, após limpeza e enriquecimento
├── cleaning_imdb.ipynb   # Notebook com todo o processo de limpeza
├── requirements.txt      # Dependências do projeto
├── .env                  # Chave da API (não versionado)
└── .gitignore
```

## Como executar

1. Clone o repositório:
```bash
git clone https://github.com/yagosilvax/ETL-IMDB_data.git
cd ETL-IMDB_data
```

2. Crie e ative um ambiente virtual:
```bash
python -m venv venv
venv\Scripts\activate      # Windows
source venv/bin/activate   # Linux/Mac
```

3. Instale as dependências:
```bash
pip install -r requirements.txt
```

4. Crie um arquivo `.env` na raiz do projeto com sua chave da OMDb API (gratuita, obtida em [omdbapi.com](https://www.omdbapi.com/apikey.aspx)):
```env
api_key=sua_chave_aqui
```

5. Abra e execute o notebook `cleaning_imdb.ipynb`.

## Desafios técnicos resolvidos

- **Identificação de nomes de coluna inconsistentes:** validação cuidadosa dos nomes reais das colunas do dataset antes de referenciá-las no código, evitando erros de acesso a colunas inexistentes.
- **Preenchimento seletivo de dados faltantes:** uso de máscara booleana (`df.loc[mask, ...]`) para aplicar a chamada à API apenas nas linhas com dado ausente, sem reprocessar registros já completos.
- **Tratamento de erro em chamadas de API em lote:** função de busca construída para retornar `None` em caso de falha (filme não encontrado, erro de conexão), evitando que uma única falha interrompesse o processamento do dataset inteiro.
- **Gerenciamento seguro de credenciais:** uso de variável de ambiente para a chave da API, mantida fora do controle de versão.


