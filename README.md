# PROJETO 25 - Dashboard Financeiro (Xperiun)

> Dashboard financeiro para análise de receitas, custos e despesas por cliente, permitindo identificar gargalos operacionais e mensurar o custo por cliente em uma visão consolidada.

---

## Contexto

- **Cliente/Stakeholder:** Xperiun (projeto de portfólio)
- **Setor:** Financeiro
- **Stack:** Power BI + DAX + Power Query (M)
- **Status:** Em andamento
- **Início:** 2024-01-01
- **Última atualização:** 2026-10-14

---

## Problema

A empresa não possui uma visão consolidada do desempenho financeiro por cliente. As análises são feitas manualmente em planilhas, sem rastreabilidade de entradas e saídas por natureza contábil, classe operacional ou tipo de custo (fixo/variável). Isso dificulta a identificação de clientes deficitários e gargalos de custo no setor financeiro.

---

## Solução

Dashboard interativo em Power BI que centraliza as movimentações financeiras (entradas e saídas) cruzadas com a base de clientes por estado. O modelo permite:

- Análise de receita vs. custo vs. despesa por cliente e por período
- Classificação de saídas em custos fixos e variáveis
- Segmentação por classe operacional (operacional vs. não operacional)
- Visão geográfica da base de clientes por UF
- Drill-down temporal de janeiro/2020 a janeiro/2026

---

## Mapeamento de Requisitos

### Fontes de Dados

| Fonte | Arquivo | Registros | Descrição |
|-------|---------|-----------|-----------|
| Movimentação Financeira | `Base de Dados/Base de Dados.xlsx` | 24.425 | Movimentações de entrada e saída com classificação contábil |
| Cadastro de Clientes | `Base de Dados/Clientes.xlsx` | 1.878 | Clientes com respectivo estado (UF) |

### Dicionário de Dados — Movimentação

| Coluna | Tipo | Descrição | Valores |
|--------|------|-----------|---------|
| Cliente | Texto | Identificador do cliente | Cliente 1, Cliente 2, ... |
| Valor | Numérico | Valor da movimentação em R$ | — |
| Data da Movimentação | Data | Data do registro | 2020-01-01 a 2026-01-30 |
| Conta Contábil | Inteiro | Código da conta | 11, 12, 21, 22, 31, 32, 33 |
| Tipo de Movimento | Texto | Direção do fluxo | Entrada, Saída |
| Natureza da Conta | Texto | Classificação contábil | Receita, Custo, Despesa |
| Classe Operacional | Texto | Classificação operacional | Operacional, Não Operacional |
| Tipo de Saída | Texto | Comportamento do custo | Fixo, Variável, - (para entradas) |

### Dicionário de Dados — Clientes

| Coluna | Tipo | Descrição | Valores |
|--------|------|-----------|---------|
| Nome Cliente | Texto | Nome do cliente | Cliente 1, Cliente 2, ... |
| Estado do Cliente | Texto | UF do cliente | 26 estados brasileiros |

### Relacionamento entre Tabelas

- **Movimentação[Cliente]** → **Clientes[Nome Cliente]** (N:1)

### Requisitos Funcionais

| ID | Requisito | Prioridade |
|----|-----------|------------|
| RF01 | Visualizar total de receitas, custos e despesas por período | Alta |
| RF02 | Identificar resultado financeiro (receita - custos - despesas) | Alta |
| RF03 | Analisar custo por cliente individual | Alta |
| RF04 | Segmentar saídas por tipo (fixo vs. variável) | Média |
| RF05 | Classificar movimentações por classe operacional | Média |
| RF06 | Filtrar por conta contábil | Média |
| RF07 | Distribuição geográfica de clientes por UF | Baixa |
| RF08 | Análise temporal com drill-down (ano/mês/dia) | Alta |
| RF09 | Comparativo de períodos (YoY, MoM) | Média |

### Requisitos Não Funcionais

| ID | Requisito |
|----|-----------|
| RNF01 | Atualização dos dados via refresh manual ou gateway |
| RNF02 | Performance aceitável com 24k+ registros |
| RNF03 | Compatibilidade com Power BI Desktop e Service |

### Métricas e Indicadores (DAX)

| Métrica | Descrição |
|---------|-----------|
| Total Receita | Soma de valores onde Tipo de Movimento = "Entrada" |
| Total Custos | Soma de valores onde Natureza da Conta = "Custo" |
| Total Despesas | Soma de valores onde Natureza da Conta = "Despesa" |
| Resultado Líquido | Total Receita - Total Custos - Total Despesas |
| Margem Operacional | Resultado Operacional / Total Receita |
| Custo por Cliente | Total Custos / Contagem de Clientes Ativos |
| % Custo Fixo | Total Custo Fixo / Total Saídas |
| % Custo Variável | Total Custo Variável / Total Saídas |

---

## Estrutura de Arquivos

```
PROJETO 25 - Financeiro(Xperiun)/
├── README.md                              # Este arquivo
├── PROJETO 25 - Financeiro(Xperiun).pbix  # Dashboard principal (em desenvolvimento)
├── Dashboard Financeiro-Gabarito.pbix     # Dashboard gabarito/referência
└── Base de Dados/
    ├── Base de Dados.xlsx                 # Movimentações financeiras (24.425 registros)
    └── Clientes.xlsx                      # Cadastro de clientes (1.878 registros)
```

---

## Como Rodar / Usar

1. Abrir o arquivo `PROJETO 25 - Financeiro(Xperiun).pbix` no Power BI Desktop
2. Em **Transformar Dados**, atualizar o caminho da fonte para apontar para a pasta `Base de Dados/` local
3. Clicar em **Atualizar** para carregar os dados
4. Para referência, consultar o `Dashboard Financeiro-Gabarito.pbix` como modelo de visualização

---

## Decisões Técnicas Relevantes

| Decisão | Motivo | Data |
|---------|--------|------|
| Duas fontes Excel separadas (movimentação + clientes) | Separação de responsabilidade: fato vs. dimensão | 2024-01-01 |
| Classificação por Natureza da Conta (Receita/Custo/Despesa) | Padrão contábil brasileiro para DRE | 2024-01-01 |
| Tipo de Saída (Fixo/Variável) apenas para saídas | Análise de ponto de equilíbrio e alavancagem operacional | 2024-01-01 |
| Conta Contábil como código numérico | Simplificação para agrupamento hierárquico | 2024-01-01 |

---

## Volumetria e Estatísticas

| Indicador | Valor |
|-----------|-------|
| Total de registros | 24.425 |
| Período coberto | Jan/2020 a Jan/2026 (6 anos) |
| Total de clientes | 1.878 |
| Estados representados | 26 UFs |
| Total de entradas | R$ 87.169.071,50 |
| Total de saídas | R$ 89.440.278,08 |
| Resultado acumulado | R$ -2.271.206,58 |
| Contas contábeis | 7 |

---

## Riscos e Limitações

- Dados são de exercício/portfólio — não representam uma empresa real
- Nomes de clientes anonimizados (Cliente 1, Cliente 2, ...)
- Não há tabela de plano de contas detalhado (apenas códigos numéricos)
- Não há tratamento de duplicatas validado na origem
- Resultado acumulado negativo pode indicar necessidade de validação nos dados de saída
- Sem informação de centro de custo ou departamento

---

## Próximos Passos

- [ ] Definir layout final das páginas do dashboard
- [ ] Implementar medidas DAX para todos os KPIs mapeados
- [ ] Criar tabela calendário (dCalendario) para análise temporal
- [ ] Criar tabela de plano de contas para enriquecer a dimensão contábil
- [ ] Adicionar página de análise geográfica por UF
- [ ] Implementar comparativo YoY e MoM
- [ ] Validar resultado negativo acumulado com regras de negócio
- [ ] Documentar regras de negócio específicas no CLAUDE.md

---

## Contato

**Nilvan Filipe**
[LinkedIn](https://www.linkedin.com/in/nilvanfelipe) · [GitHub](https://github.com/nilvanfelipe)
