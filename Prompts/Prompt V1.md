# Prompt V1

## Prompt

eu preciso que a IA me entregue uma solução para esses problemas:

* Risco de erro de interpretação: como as planilhas têm múltiplas abas e categorizações, o vendedor pode ler a disponibilidade errada e prometer ao cliente algo que não existe (ou perder uma venda por achar que não existe algo que existe).
* Alto custo de manutenção: manter as planilhas atualizadas, estruturadas e sem inconsistências demanda tempo considerável - tempo que poderia ir para venda ou atendimento.
* Ineficiência no atendimento: mesmo em vendas simples e de baixo ticket, o cliente precisa passar pelo mesmo processo manual (WhatsApp/ligação → vendedor abre planilha → verifica → responde), o que consome capacidade de venda que poderia estar concentrada nos produtos de maior valor.

Ou seja: o problema não é "o processo não funciona", é "o processo não escala e depende demais de esforço manual repetitivo, mesmo nas vendas que não precisam de tanto cuidado."

*(Antes de responder, foram feitas 2 perguntas de esclarecimento: se havia time técnico disponível, e qual a urgência. Respostas: "sem time técnico" e "pode ser o sistema definitivo".)*

## Resposta

**Stack recomendada: Airtable como banco central + Softr (ou Stacker) como camada de aplicação**

Sem time técnico, isso é o que chega mais perto de "definitivo" sem virar planilha disfarçada:

### Arquitetura

| Componente | Ferramenta | Função |
|---|---|---|
| Banco central | **Airtable** | Substitui todas as planilhas — uma base única, estruturada, com tabelas relacionadas (Eventos → Dias → Seções → Setores → Estoque) |
| Painel do vendedor | **Airtable Interface Designer** (nativo, incluso no plano) | Tela de busca/filtro por evento/dia/setor, sem abas — o vendedor vê disponibilidade real, não navega planilha |
| Site de autoatendimento | **Softr** ou **Stacker** (conectam direto no Airtable) | Cliente final consulta e compra sozinho, sem passar por vendedor |
| Sincronização | **Airtable Automations** (nativo) | Toda venda (site ou vendedor) desconta o estoque na hora, nos dois canais |
| Pagamento | **Stripe** ou **Mercado Pago**, via integração no Softr | Checkout automático no site; gera link de cobrança pro vendedor no fluxo consultivo |

### Como isso resolve cada problema

**Risco de erro de interpretação** → Acaba a navegação entre abas. O vendedor abre uma tela, filtra por evento/dia/setor, vê só o que está disponível — sem interpretar planilha cruzada.

**Alto custo de manutenção** → Uma única fonte de verdade. Atualizar estoque é uma automação (venda → desconta), não um trabalho manual de manter abas sincronizadas.

**Ineficiência no atendimento (baixo ticket)** → O site de autoatendimento tira 100% do vendedor do fluxo de tickets avulsos. Ele só entra na conversa consultiva de alto valor.

### Estrutura de dados (schema no Airtable)

```
Eventos (nome, torneio, datas)
  └── Dias (data, evento vinculado)
        └── Setores (nome, capacidade, preço)
              └── Estoque (ingressos disponíveis, reservados, vendidos)
Vendas (canal: site/vendedor, setor vinculado, valor, data, vendedor — log de auditoria)
```

### Ordem de implementação

1. Estruturar o schema no Airtable (migrar 1-2 eventos primeiro, não tudo de uma vez).
2. Montar o painel do vendedor (Interface Designer) — já elimina os problemas 1 e 2.
3. Conectar o Softr pro autoatendimento — resolve o problema 3.
4. Automations de sincronia + pagamento por último, depois que o fluxo manual novo já estiver estável.

### Limite que você precisa saber

Isso é robusto, mas **não é infinito**: Airtable tem teto de registros por base (dependendo do plano) e a sincronização via automations tem uns segundos de delay, não é instantânea como um banco de dados real. Se o volume de eventos/vendas crescer muito (múltiplos eventos grandes simultâneos, alto volume concorrente), vai chegar um ponto em que migrar pra um sistema customizado vira necessário. Pra o estágio atual, resolve os 3 problemas que você listou.
