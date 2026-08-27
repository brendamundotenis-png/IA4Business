# Biblioteca de Prompts

Prompts reutilizáveis, testados e validados nesta conversa. Cada um segue a estrutura TAREFA / FORMATO / AMOSTRA / LIMITE.

---

## Schema Airtable — Rodada Semanal

**TAREFA**
Atualizar o schema da base Airtable (Eventos, Dias, Setores, Vendas, Clientes) com o que mudou desde a última rodada — campos novos, fórmulas ajustadas, tabelas novas se pedidas. Se nada mudou numa tabela, não repetir — só listar as que tiveram alteração.

**FORMATO**
- Cabeçalho: `## Rodada [data] — mudou: [lista curta ou "nada"]`.
- Uma tabela markdown por entidade **alterada**, colunas fixas: `Campo | Tipo Airtable | Fórmula/Config`.
- Ordem de dependência quando houver mais de uma tabela: sem link primeiro, com link depois.
- Fórmula cross-tabela (que depende de campo de outra tabela linkada) precisa declarar o `Lookup` intermediário antes da `Formula` que o usa — nunca referenciar campo de outra tabela direto dentro de `Formula`.
- Campos de link reverso não entram na tabela (Airtable cria sozinho).
- Zero texto explicativo fora das tabelas.

**AMOSTRA**
```
### Setores
| Campo | Tipo Airtable | Fórmula/Config |
|---|---|---|
| Nome do Setor | Single line text (primary) | — |
| Dia | Link to another record → Dias | — |
| Categoria | Single select | Standard, Premium, VIP, Hospitalidade |
| Capacidade Total | Number (integer) | — |
| Preço Unitário | Currency (BRL) | — |
| Vendido | Rollup ← Vendas.Quantidade | `SUM(values)` com filtro Status=Pago |
| Reservado | Rollup ← Vendas.Quantidade | `SUM(values)` com filtro Status=Reservado |
| Disponível | Formula | `{Capacidade Total} - {Vendido} - {Reservado}` |
```

**LIMITE**
- Não incluir `Pacotes`/Hospedagem (fase 2, fixo).
- Não reabrir Airtable/Softr como escolha de ferramenta (decisão permanente).
- Não adicionar campo não pedido explicitamente na semana — sugestão vai numa lista separada, fora do schema.
- Não regenerar tabela que não mudou.

**Fechamento fixo**
Uma linha: `O que mudou desta rodada pra anterior: [...]`.
