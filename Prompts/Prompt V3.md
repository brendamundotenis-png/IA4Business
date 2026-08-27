# Prompt V3

## Prompt

Este é um pedido que eu vou usar toda semana:
TAREFA
Construir o schema completo da base Airtable (Eventos, Dias, Setores, Vendas, Clientes) — tabelas, campos, tipos exatos do Airtable, relações e fórmulas — pronto pra eu copiar campo por campo dentro do Airtable, sem precisar traduzir nada.
FORMATO
Uma tabela markdown por entidade do Airtable.
Colunas fixas: Campo | Tipo Airtable | Fórmula/Config.
Zero texto explicativo entre as tabelas — nada de "isso serve pra...". Só o campo, o tipo, e a fórmula quando houver.
Se um campo for automático (rollup, lookup, formula), a coluna "Fórmula/Config" tem que ter a fórmula literal, pronta pra colar na caixa de fórmula do Airtable — não descrita em português.
AMOSTRA (padrão de qualidade — é assim que cada tabela deve sair)
### Setores
| Campo | Tipo Airtable | Fórmula/Config |
|---|---|---|
| Nome do Setor | Single line text (primary) | — |
| Dia | Link to another record → Dias | — |
| Categoria | Single select | Standard, Premium, VIP, Hospitalidade |
| Capacidade Total | Number (integer) | — |
| Preço Unitário | Currency (BRL) | — |
| Vendido | Rollup ← Vendas.Quantidade | SUM(values) com filtro Status=Pago |
| Reservado | Rollup ← Vendas.Quantidade | SUM(values) com filtro Status=Reservado |
| Disponível | Formula | {Capacidade Total} - {Vendido} - {Reservado} |
LIMITE
Não incluir tabela de Pacotes/Hospedagem — fica pra fase 2.
Não sugerir alternativa ao Airtable/Softr — já foi decidido, não reabrir essa discussão.
Não adicionar campo que não foi discutido nesta conversa (CRM completo, tags de marketing, campos de margem/comissão) — se achar útil, listar como sugestão separada no final, fora do schema.
Não repetir a explicação de arquitetura ou o "por quê" de cada tabela — isso já foi dado antes.
Rodando
Eventos
Campo	Tipo Airtable	Fórmula/Config
Nome do Evento	Single line text (primary)	—
Torneio	Single select	ATP Barcelona, Miami Open, (adicionar por evento)
Local	Single line text	—
Data Início	Date	—
Data Fim	Date	—
Status	Single select	Planejamento, Ativo, Encerrado
Dias	Link to another record → Dias	—
Dias
Campo	Tipo Airtable	Fórmula/Config
Data	Date (primary)	—
Evento	Link to another record → Eventos	—
Setores	Link to another record → Setores	—
Setores
Campo	Tipo Airtable	Fórmula/Config
Nome do Setor	Single line text (primary)	—
Dia	Link to another record → Dias	—
Categoria	Single select	Standard, Premium, VIP, Hospitalidade
Capacidade Total	Number (integer)	—
Preço Unitário	Currency (BRL)	—
Vendido	Rollup ← Vendas.Quantidade	SUM(values) com filtro Status=Pago
Reservado	Rollup ← Vendas.Quantidade	SUM(values) com filtro Status=Reservado
Disponível	Formula	{Capacidade Total} - {Vendido} - {Reservado}
Vendas
Campo	Tipo Airtable	Fórmula/Config
ID Venda	Autonumber (primary)	—
Setor	Link to another record → Setores	—
Canal	Single select	Site, Vendedor
Vendedor	Single line text	—
Cliente	Link to another record → Clientes	—
Quantidade	Number (integer)	—
Valor Total	Formula	{Quantidade} * {Setor Preço Unitário} (lookup do Setor)
Status	Single select	Reservado, Pago, Cancelado
Forma de Pagamento	Single select	Pix, Cartão, Boleto
Data da Venda	Created time	—
Clientes
Campo	Tipo Airtable	Fórmula/Config
Nome	Single line text (primary)	—
WhatsApp/Contato	Phone number	—
Perfil	Single select	Ticket Avulso, Pacote Completo
Histórico de Compras	Link to another record → Vendas	—

Critique como um revisor exigente: o que está ambíguo, o que falta, o que sobra? Depois reescreva na melhor versão possível.

## Resposta

### Crítica

**Ambíguo**
- "Construir o schema completo" — toda semana? Ou só o que mudou? Se o schema já existe no Airtable, rodar isso toda semana te dá as mesmas 5 tabelas de novo, mesmo que nada tenha mudado. Não define se é criação do zero ou atualização incremental.
- A fórmula de `Valor Total` na tabela Vendas (`{Quantidade} * {Setor Preço Unitário}`) **não funciona no Airtable**: fórmula não acessa campo de tabela linkada direto — precisa de um campo `Lookup` intermediário primeiro. O template não define essa regra, então o output "pronto pra colar" não cola.
- Campos de link reverso (ex: `Dias` dentro de `Eventos`) são criados automaticamente pelo Airtable quando você linka do outro lado — não fica claro se isso deve aparecer na tabela markdown ou não.

**Falta**
- Cabeçalho de versão/data — pra um pedido semanal, sem isso você não sabe se está olhando a rodada de hoje ou de duas semanas atrás.
- Regra do que fazer quando um campo **já existe** e precisa mudar (nova opção de select, fórmula ajustada) vs. criar algo novo do zero.
- Regra de ordem de entrega por dependência (Eventos → Dias → Setores → Vendas → Clientes) — hoje é só convenção, não é regra escrita.

**Sobra**
- Repetir a tabela `Setores` inteira como AMOSTRA toda semana é redundante depois da primeira vez — vira ruído no pedido.
- "Não sugerir alternativa ao Airtable/Softr" é uma decisão permanente, não precisa ser reafirmada toda rodada — isso é regra de projeto, não de tarefa semanal.

### Reescrita

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
Já enviada e validada na rodada de hoje (tabela `Setores`) — vale como padrão fixo, não precisa reenviar toda semana.

**LIMITE**
- Não incluir `Pacotes`/Hospedagem (fase 2, fixo).
- Não reabrir Airtable/Softr como escolha de ferramenta (decisão permanente).
- Não adicionar campo não pedido explicitamente na semana — sugestão vai numa lista separada, fora do schema.
- Não regenerar tabela que não mudou.

**Fechamento fixo**
Uma linha: `O que mudou desta rodada pra anterior: [...]`.
