# Prompt V2

## Prompt

Agora eu quero que você preencha de acordo com o que definimos até agora:
A TAREFA
um verbo claro, uma entrega só
O FORMATO
como você quer receber
A AMOSTRA
um exemplo do que é bom, colado
LIMITE
o que não fazer, o que não tocar
Depois de rodar, uma linha por escrito:
o que mudou da V1 para a V2?

*(Antes de responder, foi feita 1 pergunta de esclarecimento: se o template era pra um briefing de construção da base Airtable ou pra virar um template reutilizável genérico. Resposta: "briefing pra construir a base Airtable".)*

## Resposta

### Brief preenchido

**TAREFA**
Construir o schema completo da base Airtable (Eventos, Dias, Setores, Vendas, Clientes) — tabelas, campos, tipos exatos do Airtable, relações e fórmulas — pronto pra eu copiar campo por campo dentro do Airtable, sem precisar traduzir nada.

**FORMATO**
- Uma tabela markdown por entidade do Airtable.
- Colunas fixas: `Campo | Tipo Airtable | Fórmula/Config`.
- Zero texto explicativo entre as tabelas — nada de "isso serve pra...". Só o campo, o tipo, e a fórmula quando houver.
- Se um campo for automático (rollup, lookup, formula), a coluna "Fórmula/Config" tem que ter a fórmula literal, pronta pra colar na caixa de fórmula do Airtable — não descrita em português.

**AMOSTRA** (padrão de qualidade — é assim que cada tabela deve sair)
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
- Não incluir tabela de `Pacotes`/Hospedagem — fica pra fase 2.
- Não sugerir alternativa ao Airtable/Softr — já foi decidido, não reabrir essa discussão.
- Não adicionar campo que não foi discutido nesta conversa (CRM completo, tags de marketing, campos de margem/comissão) — se achar útil, listar como sugestão separada no final, fora do schema.
- Não repetir a explicação de arquitetura ou o "por quê" de cada tabela — isso já foi dado antes.

---

### Rodando

#### Eventos
| Campo | Tipo Airtable | Fórmula/Config |
|---|---|---|
| Nome do Evento | Single line text (primary) | — |
| Torneio | Single select | ATP Barcelona, Miami Open, (adicionar por evento) |
| Local | Single line text | — |
| Data Início | Date | — |
| Data Fim | Date | — |
| Status | Single select | Planejamento, Ativo, Encerrado |
| Dias | Link to another record → Dias | — |

#### Dias
| Campo | Tipo Airtable | Fórmula/Config |
|---|---|---|
| Data | Date (primary) | — |
| Evento | Link to another record → Eventos | — |
| Setores | Link to another record → Setores | — |

#### Setores
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

#### Vendas
| Campo | Tipo Airtable | Fórmula/Config |
|---|---|---|
| ID Venda | Autonumber (primary) | — |
| Setor | Link to another record → Setores | — |
| Canal | Single select | Site, Vendedor |
| Vendedor | Single line text | — |
| Cliente | Link to another record → Clientes | — |
| Quantidade | Number (integer) | — |
| Valor Total | Formula | `{Quantidade} * {Setor Preço Unitário}` (lookup do Setor) |
| Status | Single select | Reservado, Pago, Cancelado |
| Forma de Pagamento | Single select | Pix, Cartão, Boleto |
| Data da Venda | Created time | — |

#### Clientes
| Campo | Tipo Airtable | Fórmula/Config |
|---|---|---|
| Nome | Single line text (primary) | — |
| WhatsApp/Contato | Phone number | — |
| Perfil | Single select | Ticket Avulso, Pacote Completo |
| Histórico de Compras | Link to another record → Vendas | — |

---

**O que mudou de V1 pra V2:** V2 tirou toda explicação de arquitetura/justificativa e virou tabela pura com fórmula literal pronta pra colar — V1 era pra entender a solução, V2 é pra executar.
