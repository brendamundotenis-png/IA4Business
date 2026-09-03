# Automações Conectadas

Registro dos conectores (MCP) ativos na conta da Brenda e o que já foi testado com cada um. Conexão é feita em **Settings → Connectors** do Claude (nível de conta, não deste repositório) — este arquivo só documenta o que está ativo e o que foi validado.

## Gmail

- **Conta:** brendamundotenis@gmail.com
- **Conectado em:** 03/09/2026
- **Teste realizado:** envio de e-mail pra própria conta — assunto "Teste", corpo "E-mail de teste do Cloud Code". Entregue com sucesso.
- **O que dá pra fazer:** ler, buscar, responder, encaminhar, criar rascunho e enviar e-mail; gerenciar labels; marcar spam/lixeira.
- **Regra de uso:** nenhuma ação de e-mail (enviar, responder, encaminhar) é feita sem pedido explícito na conversa.

## Notion

- **Conectado em:** 03/09/2026
- **Teste realizado:** listagem de páginas recentes (`Lista de tarefas semanal - Pendências`, `Bem-vindo ao Notion!`, `Orçamento mensal`) + leitura e edição da página "Pendências" (inserção de resumo automático de status das tarefas da semana).
- **O que dá pra fazer:** buscar, ler, criar e editar páginas e databases; comentários; anexos.
- **Regra de uso:** edição de conteúdo é feita quando pedida explicitamente, refletindo o estado real da página (sem inventar status de tarefa).

## Pendências

- Escolha de **3 das 5 skills** (Qualifica-Lead, Schema-Airtable, Proposta-Cliente, Contexto-Negocio, Estoque-Tempo-Real) pra criar, testar e commitar em `.claude/skills`.
