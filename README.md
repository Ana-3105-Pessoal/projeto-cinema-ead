# 🎬 Análise da Indústria Cinematográfica

> **Status:** Projeto concluído — pipeline, EDA, visualizações e dashboard finalizados

Pipeline completo de dados sobre a indústria do cinema, desde a extração de múltiplas fontes até a análise exploratória e visualização em dashboard.

---

## 📌 Objetivo

Investigar padrões de sucesso na indústria cinematográfica a partir de dados reais, respondendo perguntas como:
- Quais filmes tiveram os maiores lucros absolutos e maior retorno sobre investimento (ROI)?
- Filmes de franquias faturam mais do que filmes independentes?
- Quais estúdios apresentam maior lucro total?
- A língua original e o país de produção influenciam o desempenho financeiro?
- Qual gênero é mais lucrativo e popular?
- A quantidade de votos influencia a confiabilidade da nota?
- Qual período histórico produziu mais filmes de sucesso?
- Popularidade tem relação com lucro ou avaliação?

---

## 🗂️ Fontes de Dados

| Fonte | Descrição |
|-------|-----------|
| [IMDb Datasets](https://datasets.imdbws.com/) | Metadados e avaliações de filmes (`title.basics.tsv.gz` e `title.ratings.tsv.gz`) |
| [Kaggle — The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?resource=download&select=movies_metadata.csv) | Dataset `movies_metadata.csv` com informações financeiras e de produção |
| [TMDB API](https://www.themoviedb.org/) | Dados complementares: franquias, estúdios, idioma original e países de produção |

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
     Transformação
          ↓
  Análise exploratória (EDA)
          ↓
     Visualizações
          ↓
      Dashboard
```

**Decisões técnicas relevantes:**
- Join entre fontes feito via `imdb_id` para evitar ambiguidades por título
- Cache da extração via API salvo em `tmdbAPI_dados.csv` para evitar requisições repetidas
- Colunas binárias criadas (`idioma_ingles`, `produzido_eua`, `voto_confiavel`) para facilitar análises comparativas
- Nulos em `estudio` e `paises_producao` preenchidos com `"Não informado"` para preservar dados das demais colunas
- Filmes com orçamento abaixo de R$1.000 removidos por serem dados placeholder sem valor analítico

---

## 🔍 Colunas Criadas na Transformação

| Coluna | Origem | Descrição |
|--------|--------|-----------|
| `lucro` | Calculada | `receita - orcamento` |
| `roi` | Calculada | `(lucro / orcamento) * 100` — retorno sobre investimento em % |
| `ano` | `data_lancamento` | Ano de lançamento extraído da data |
| `decada` | `ano` | Agrupamento por década (ex: "2000s") |
| `idioma_ingles` | `idioma_original` | True se idioma original for inglês |
| `produzido_eua` | `paises_producao` | True se produzido nos EUA |
| `genero_principal` | `generos` | Primeiro gênero listado |
| `voto_confiavel` | `num_votos` | True se acima da mediana de votos do dataset |

---

## 📊 Principais Insights

- **Lucro vs ROI:** os filmes com maior lucro absoluto (Avatar, Avengers) são diferentes dos com maior ROI — filmes de baixo orçamento como Paranormal Activity dominam o retorno proporcional
- **Franquias:** franquias lideram o topo do lucro absoluto; Avatar Collection tem média de 2,5 bilhões por filme
- **Gênero:** Adventure é o gênero mais lucrativo em média, superando Action apesar de Action ter quase 3x mais filmes
- **Idioma e país:** filmes em inglês faturam mais em média, mas filmes em outros idiomas têm nota média maior — possível viés de seleção na base
- **Votos:** filmes com muitos votos têm nota média mais alta (6,86 vs 6,14) e menor desvio padrão (0,87 vs 1,00), indicando avaliações mais estáveis

---

## 📁 Estrutura do Repositório

```
ProjetoFinal_EAD/
│
├── projeto_final.ipynb     # Notebook principal com todo o pipeline
├── fontes_dados/           # Datasets brutos (IMDb, Kaggle) — ignorada no .gitignore
├── tmdbAPI_dados.csv       # Cache da extração via TMDB API
├── df_final_limpo.csv      # Dataset final tratado e transformado
├── tmdb.env                # Chave da API (não versionado)
├── .gitignore
└── README.md
```

> Os datasets brutos (IMDb e Kaggle) não estão incluídos por serem grandes demais. Baixe pelos links acima e coloque na pasta `fontes_dados/` antes de rodar o notebook.

---

## 🔑 Configuração da API

Para rodar a extração via TMDB, crie um arquivo `tmdb.env` na raiz do projeto:

```
TMDB_API_KEY=sua_chave_aqui
```

Gere sua chave gratuita em [themoviedb.org](https://www.themoviedb.org/).

> A extração completa (~5.300 filmes) leva entre 5 e 15 minutos. Após a primeira execução, os dados ficam em cache em `tmdbAPI_dados.csv` e não é necessário rodar novamente.

---

## 🛠️ Tecnologias

- Python 3
- Pandas
- Requests + python-dotenv
- Matplotlib + Seaborn
- Jupyter Notebook
- Looker Studio

---

## 📚 Disciplina

**Extração e Análise de Dados** — 3º Semestre
Centro Universitário SENAC (UNISENAC) — 2026