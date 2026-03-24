# Pipeline de Dados – Censo Escolar (INEP)

Pipeline de dados desenvolvido em PySpark no Databricks para ingestão, tratamento e análise dos microdados do Censo Escolar.

---

## Objetivo

Construir um pipeline escalável para transformar dados brutos em indicadores de infraestrutura escolar por estado, utilizando arquitetura em camadas.

---

## Arquitetura

Modelo Medallion:

- **Bronze**: ingestão dos dados brutos
- **Silver**: tratamento, limpeza e padronização
- **Gold**: agregações e indicadores analíticos

Fluxo:

1. Download dos dados via HTTP
2. Extração e leitura dos arquivos CSV
3. Tratamento de encoding (`latin1`)
4. Limpeza e filtro de dados
5. Agregação por estado (`NO_UF`)
6. Cálculo de indicadores percentuais

---

## Fonte de Dados

- Origem: INEP - Instituto Nacional de Estudos e Pesquisas Educacionais (Censo Escolar)
- Formato: CSV (compactado em .zip)
- Enconding: latin1
- Frequência: Anual
- Tabelas:
    - Tabela Escola
    - Tabela Turma

https://download.inep.gov.br/dados_abertos/microdados_censo_escolar_{ano}_.zip

---

## Tecnologias

* Databricks
* PySpark
* Delta Lake
* Unity Catalog
* Python (requests, zipfile)

---

## Modelagem

### Bronze

* Dados brutos sem transformação

### Silver

* Remoção de nulos
* Padronização de colunas
* Ajuste de encoding

### Gold

* Agrupamento por estado
* Métricas derivadas

---

## Indicadores

MÉTRICAS:

* % escolas com água
* % escolas com energia elétrica
* % escolas com internet
* % escolas com banda larga
* % escolas com laboratório (ciências e informática)
* % escolas com biblioteca
* % escolas com quadra esportiva
* % escolas com refeitório

PESOS:

Saneamento > Infraestrutura > Lazer

* PERC_POSSUI_AGUA: 2
* PERC_POSSUI_ESGOTO: 2
* PERC_POSSUI_ENERGIA: 2
* PERC_POSSUI_BANHEIRO: 2
* PERC_POSSUI_INTERNET: 1.5
* PERC_POSSUI_BANDA_LARGA: 1.5
* PERC_POSSUI_LAB_INFORMATICA: 1
* PERC_POSSUI_LAB_CIENCIAS: 1
* PERC_POSSUI_BIBLIOTECA: 1
* PERC_POSSUI_QUADRA_ESPORTES: 0.5
* PERC_POSSUI_REFEITORIO: 1

CLASSIFICAÇÃO:

* SCORE_INFRA >= 85 --> A
* SCORE_INFRA >= 80 --> B
* SCORE_INFRA >= 75 --> C
* SCORE_INFRA >= 70 --> D
* SCORE_INFRA >  70 --> E

---

## Lógica de Cálculo

Percentual = (Quantidade com característica / Total de escolas) * 100

Para colunas binárias (0 e 1), o percentual pode ser obtido via média.

---

## Parametrização

O pipeline é controlado por:

```python
year_ref
```

Permite reprocessamento por ano.

---

## Execução

1. Definir o ano (Por padrão, é obtido o ano atual -1):

```python
year_ref = 2026
```

2. Executar o notebook completo no Databricks

---

## Observações

* Uso de encoding `latin1` devido ao padrão dos dados
* Dependência da disponibilidade da fonte oficial
* Pipeline preparado para reprocessamento

---

## Autores

* Alef Anderson Fernandes Clarindo da Silva
* Gabriel de Almeida Gava
* Geovanna dos Santos Medeiros
* Guilherme José Pereira Ratini
* Riuler Machado Mendes

