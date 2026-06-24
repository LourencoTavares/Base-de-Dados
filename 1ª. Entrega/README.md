# Projeto BD — Parte 1: Jardim Zoológico

**Projeto desenvolvido no âmbito da UC de Base de Dados (2025/2026).**

---

## Descrição

Este projeto corresponde à **1.ª entrega do projeto de Base de Dados**, tendo como objetivo modelar o domínio de um **jardim zoológico**.

O sistema permite representar informação sobre:

- animais e respetivas espécies;
- compatibilidade entre espécies;
- recintos do zoo;
- bilhetes e acessos a recintos especiais;
- faturação;
- funcionários, tratadores e funcionários de limpeza;
- alocações temporais de funcionários a recintos.

O foco desta entrega foi a **modelação conceptual e relacional da base de dados**, garantindo que o modelo respeita as regras do domínio descritas no enunciado. :contentReference[oaicite:0]{index=0}

---

## Objetivo

Desenvolver uma solução de modelação de dados que inclua:

- um **Modelo Entidade-Associação (E-A)**;
- um conjunto de **Restrições de Integridade (RIs)**;
- a respetiva **conversão para Modelo Relacional**;
- normalização das relações, procurando garantir **FNBC** sempre que possível;
- resolução de interrogações em **Álgebra Relacional**.

O projeto seguiu uma abordagem de:

> análise do domínio -> modelação E-A -> conversão relacional -> validação com RIs -> álgebra relacional

---

## Conteúdo Implementado

O relatório está dividido em três partes principais. :contentReference[oaicite:1]{index=1}

### 1. Modelação Entidade-Associação

Foi construído um modelo E-A para representar o domínio do jardim zoológico.

O modelo inclui entidades como:

- **Animal**
- **Espécie**
- **Recinto**
- **Bilhete**
- **Fatura**
- **Funcionário**
- **Tratador**
- **Funcionário de Limpeza**
- **Intervalo Temporal**

Foram também modeladas especializações de recintos:

- **Recinto Individual**
- **Recinto Específico**
- **Recinto Partilhado**
- **Recinto Especial**

Além do diagrama, foram definidas várias **Restrições de Integridade**, como:

- uma espécie não pode ser compatível consigo própria;
- um recinto individual contém exatamente um animal;
- um recinto específico contém todos os animais de uma só espécie;
- um recinto partilhado contém espécies compatíveis entre si;
- um funcionário não pode estar alocado a horários sobrepostos.

---

### 2. Conversão E-A para Modelo Relacional

O modelo E-A foi convertido para o modelo relacional, resultando em relações como:

- `Recinto`
- `RecintoEspecial`
- `RecintoNãoEspecial`
- `Animal`
- `Espécie`
- `Compatível`
- `Bilhete`
- `Fatura`
- `Funcionário`
- `Tratador`
- `FuncionárioLimpeza`
- `Alocado_a`
- `SupervisionadoPor`

Foram identificadas:

- **chaves primárias**;
- **chaves estrangeiras**;
- **restrições UNIQUE**;
- **restrições de integridade adicionais**.

Também foram consideradas regras de normalização, com o objetivo de manter as relações em **Forma Normal de Boyce-Codd (FNBC)** sempre que possível.

---

### 3. Álgebra Relacional

Foram resolvidas interrogações sobre o esquema relacional fornecido no enunciado, incluindo:

1. identificar o(s) recinto(s) especiais com menos bilhetes vendidos;
2. contar bilhetes com acesso a todos os recintos especiais;
3. contar bilhetes sem acesso a recintos especiais;
4. interpretar uma expressão de álgebra relacional dada.

---

## Estrutura do Projeto

- `E1 - Enunciado.pdf` -> enunciado da 1.ª entrega do projeto
- `entrega-bd-01-06.pdf` -> relatório final da entrega
- diagrama E-A incluído no relatório
- modelo relacional incluído no relatório
- respostas de álgebra relacional incluídas no relatório
  
---

## Autores

Projeto realizado pelo **Grupo 06**, no âmbito da UC de **Base de Dados**.

Elementos do grupo:

- Afonso Lúcio e Costa
- Lourenço Guerreiro Tavares
- Rafael Rosas Caratão
