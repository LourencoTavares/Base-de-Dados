# Projeto BD — Parte 2: Implementação do Sistema Zoo

**Projeto desenvolvido no âmbito da UC de Base de Dados (2025/2026).**

---

## Descrição

Este projeto corresponde à **2.ª entrega do projeto de Base de Dados**, dando continuidade à modelação realizada na Parte 1.

Nesta fase, o objetivo foi implementar a base de dados do **sistema de informação de um jardim zoológico**, permitindo gerir e analisar:

- zonas do zoo;
- recintos;
- espécies e animais;
- vendas de bilhetes;
- acessos dos bilhetes a zonas;
- votação nos recintos;
- receitas e rentabilidade.

Além da base de dados, foi também desenvolvido um **protótipo de aplicação RESTful**, que permite interagir com a base de dados através de endpoints em JSON.

---

## Objetivo

Implementar uma solução completa que inclua:

- criação do esquema SQL da base de dados;
- implementação de Restrições de Integridade;
- preenchimento consistente da base de dados;
- desenvolvimento de uma API REST;
- análise de dados com SQL;
- criação de uma vista materializada;
- cálculo de rentabilidade dos recintos;
- otimização de consultas com índices.

O projeto seguiu uma abordagem de:

> criação da BD -> implementação de RIs -> povoamento da BD -> desenvolvimento da API -> análise de dados -> otimização com índices

---

## Funcionalidades Implementadas

### 1. Criação da Base de Dados

Foi implementada a base de dados `Zoo` em PostgreSQL.

O esquema inclui as seguintes tabelas principais:

- `zona`
- `recinto`
- `especie`
- `animal`
- `venda`
- `bilhete`
- `acesso`

Foram também criados tipos enumerados para representar:

- categorias de animais;
- continentes.

Exemplos de categorias:

- Aves
- Carnívoros
- Herbívoros
- Mamíferos Marinhos
- Primatas
- Répteis

---

### 2. Restrições de Integridade

Foram implementadas Restrições de Integridade para garantir a consistência dos dados.

Entre as principais RIs estão:

- uma zona não pode ter simultaneamente `categoria` e `continente` a `NULL`;
- um animal tem de estar alojado num recinto pertencente a uma zona compatível com a sua espécie;
- todos os animais da mesma espécie têm de estar alojados em recintos da mesma zona;
- uma venda tem de incluir pelo menos um bilhete com acesso a pelo menos uma zona.

Sempre que possível foram usadas restrições simples, como `CHECK`, `PRIMARY KEY`, `FOREIGN KEY` e `UNIQUE`.

Foram usados `TRIGGERs` apenas quando a restrição envolvia várias tabelas ou validações que não podiam ser expressas diretamente com restrições simples.

---

### 3. Preenchimento da Base de Dados

A base de dados foi preenchida com dados consistentes e suficientes para permitir testar as consultas e funcionalidades.

O preenchimento inclui:

- pelo menos 7 zonas;
- zonas dedicadas a categorias;
- zonas dedicadas a continentes;
- zonas com categoria e continente;
- recintos associados a zonas;
- espécies reais;
- animais associados a espécies e recintos;
- vendas de bilhetes;
- acessos dos bilhetes a zonas;
- votos nos recintos.

As vendas foram geradas para o período de:

> 2026-01-01 -> 2026-06-11

Foram ainda considerados requisitos como:

- mais bilhetes vendidos ao fim de semana;
- existência de bilhetes com desconto;
- bilhetes com acesso total;
- pelo menos 75% dos bilhetes com voto efetuado.

---

### 4. Desenvolvimento da Aplicação RESTful

Foi desenvolvido um protótipo de **web service RESTful** para permitir acesso programático à base de dados.

A API devolve respostas em JSON e implementa os seguintes endpoints:

#### `GET /zona/<zona>/`

Devolve a lista de recintos de uma determinada zona.

Para cada recinto, apresenta:

- número do recinto;
- espécies presentes;
- nome científico;
- nome comum;
- número de animais dessa espécie no recinto.

---

#### `POST /recinto/<recinto>/voto/<bilhete>/`

Regista o voto de um bilhete num recinto.

A operação:

- verifica se o bilhete existe;
- verifica se o bilhete ainda não votou;
- verifica se o recinto existe;
- confirma se o bilhete tem acesso à zona do recinto;
- atualiza `bilhete.votou`;
- incrementa `recinto.votos`.

Também devolve mensagens de erro adequadas quando:

- o bilhete não existe;
- o recinto não existe;
- o bilhete já votou;
- o bilhete não tem acesso à zona do recinto.

---

#### `POST /venda/`

Executa uma venda de um ou mais bilhetes.

O endpoint recebe:

- NIF do cliente, opcional;
- lista de bilhetes;
- zonas de acesso de cada bilhete;
- desconto opcional.

A operação insere dados nas tabelas:

- `venda`
- `bilhete`
- `acesso`

E devolve:

- preço total da venda;
- lista dos bilhetes vendidos;
- número de cada bilhete;
- preço de cada bilhete.

---

## Segurança e Transações

Foram aplicadas estratégias para garantir segurança e consistência.

### Prevenção de SQL Injection

As queries foram escritas com parâmetros do Psycopg, usando placeholders, em vez de concatenação direta de strings.

Exemplo conceptual:

```python
cur.execute(
    """
    SELECT *
    FROM bilhete
    WHERE bid = %(bilhete)s;
    """,
    {"bilhete": bilhete}
)
