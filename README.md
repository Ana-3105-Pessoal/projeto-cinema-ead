# 🎬 Análise da Indústria Cinematográfica

> **Status:** Em desenvolvimento — projeto final da disciplina de Extração e Análise de Dados

Pipeline completo de dados sobre a indústria do cinema, desde a extração de múltiplas fontes até a análise exploratória e visualização em dashboard.

---

## 📌 Objetivo

Investigar padrões de sucesso na indústria cinematográfica a partir de dados reais, respondendo perguntas como:
- Filmes de franquias faturam mais do que filmes independentes?
- Quais estúdios apresentam maior taxa de sucesso?
- A língua original e o país de produção influenciam o desempenho?

---

## 🗂️ Fontes de Dados

| Fonte | Descrição |
|-------|-----------|
| [IMDb Datasets](https://datasets.imdbws.com/) | Metadados e avaliações de filmes |
| [Kaggle — The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?resource=download&select=movies_metadata.csv) | Dataset `movies_metadata.csv` com informações financeiras e de produção |
| [TMDB API](https://www.themoviedb.org/) | Dados complementares: franquias, estúdios, idioma original e países de produção |

Os datasets de IMDb Dataset pegos foram: 'title.basics.tsv.gz' e 'title.rating.tsv.gz'

---

## ⚙️ Pipeline

```
IMDb (title.basics + title.ratings)
          +
    movies_metadata.csv (Kaggle)
          +
       TMDB API
          ↓
     Merge e limpeza
          ↓
  Análise exploratória
          ↓
      Dashboard
```

**Decisões técnicas relevantes:**
- Join entre fontes feito via `imdb_id` para evitar ambiguidades por título
- Cache da extração via API salvo em `tmdbAPI_dados.csv` para evitar requisições repetidas
- Colunas binárias criadas (`idioma_ingles`, `produzido_eua`, `is_franchise`) para facilitar análises comparativas
- Nulos em `estudio_principal` e `production_countries` preenchidos com `"Não informado"` para preservar dados das demais colunas

---

## 📁 Estrutura do Repositório

```
ProjetoFinal_EAD/
│
├── projeto_final.ipynb     # Notebook principal com todo o pipeline
├── tmdbAPI_dados.csv       # Resultado da extração via TMDB API
├── .gitignore
└── README.md
```

> Os datasets brutos (IMDb e Kaggle) não estão incluídos por serem grandes demais. Baixe os links acima e coloque na mesma pasta antes de rodar o notebook.

---

## 🔑 Configuração da API

Para rodar a extração via TMDB, crie um arquivo `tmdb.env` na raiz do projeto:

```
TMDB_API_KEY=sua_chave_aqui
```

Gere sua chave gratuita em [themoviedb.org](https://www.themoviedb.org/).

---

## 🛠️ Tecnologias

- Python 3
- Pandas
- Requests
- Jupyter Notebook
- Power BI / Looker Studio *(dashboard em desenvolvimento)*
