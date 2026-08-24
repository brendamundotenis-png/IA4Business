### **1\. O Problema \- Empresa familiar**

O processo de vendas B2C de ingressos para eventos esportivos **já funciona**, mas de forma pouco eficiente e vulnerável a erros. A gestão do estoque de ingressos (dias, seções e setores) é feita em planilhas complexas e altamente detalhadas, o que gera três consequências diretas:

* **Risco de erro de interpretação**: como as planilhas têm múltiplas abas e categorizações, o vendedor pode ler a disponibilidade errada e prometer ao cliente algo que não existe (ou perder uma venda por achar que não existe algo que existe).  
* **Alto custo de manutenção**: manter as planilhas atualizadas, estruturadas e sem inconsistências demanda tempo considerável \- tempo que poderia ir para venda ou atendimento.  
* **Ineficiência no atendimento**: mesmo em vendas simples e de baixo ticket, o cliente precisa passar pelo mesmo processo manual (WhatsApp/ligação → vendedor abre planilha → verifica → responde), o que consome capacidade de venda que poderia estar concentrada nos produtos de maior valor.

Ou seja: o problema não é "o processo não funciona", é **"o processo não escala e depende demais de esforço manual repetitivo, mesmo nas vendas que não precisam de tanto cuidado."**

**2\. Como é hoje**

* Cada evento tem sua própria planilha.  
* Cada planilha contém diversas abas, organizadas por dia, seção e setor do evento.  
* Todas as vendas — independente do valor do ticket — são feitas por atendimento individual (vendedor via WhatsApp ou ligação).  
* Fluxo de venda atual:  
  1. Cliente entra em contato via WhatsApp ou ligação.  
  2. Vendedor abre a planilha do evento correspondente.  
  3. Vendedor navega entre as abas para checar disponibilidade (dia/seção/setor).  
  4. Vendedor informa ao cliente as opções disponíveis.  
  5. Venda é fechada e a planilha é atualizada manualmente.  
* Não há distinção de fluxo entre uma venda simples (ticket de menor valor) e uma venda de alto valor que exige mais consultoria — **ambas passam pelo mesmo processo manual**, com o mesmo custo operacional de tempo do vendedor.

### **3\. Como vou resolver**

A proposta é segmentar a jornada de vendas em dois fluxos distintos, de acordo com o valor/margem do produto:

**a) Tickets de menor valor (venda de menor complexidade)**

* Migrar para um **sistema de autoatendimento no próprio site**, onde o cliente consulta disponibilidade e compra diretamente, sem depender de um vendedor.  
* Isso libera tempo do time comercial e reduz o gargalo de atendimento em vendas que não exigem consultoria.

**b) Tickets de maior valor/margem (venda consultiva)**

* Mantém o atendimento personalizado via WhatsApp/ligação.  
* Porém, substitui a planilha manual por um **sistema/painel intuitivo de consulta ao "arsenal" de ingressos e produtos**, dando ao vendedor acesso rápido e confiável à disponibilidade real, sem risco de erro de interpretação e sem depender de manutenção manual constante.

Em resumo: **automatizar a ponta de baixo valor (self-service) e instrumentalizar melhor a ponta de alto valor (ferramenta de apoio ao vendedor)**, eliminando a dependência da planilha como fonte única de verdade em ambos os casos.

---

### **Requisitos Técnicos do Sistema**

**Base de dados centralizada:** substituir as planilhas por um banco de dados único, com estoque de ingressos estruturado por evento, dia, seção e setor, atualizado em tempo real para evitar overselling ou informações divergentes entre canais.

**Módulo de autoatendimento (site):** integração de e-commerce que consulte essa base em tempo real, exibindo disponibilidade, preços e checkout (pagamento online, emissão de comprovante/ticket) para os produtos de menor valor.

**Painel do vendedor (venda consultiva):** interface simples e rápida (web ou app interno) para consulta de disponibilidade por evento/dia/seção/setor, com filtros e busca — substituindo a navegação manual entre abas. Deve permitir reserva temporária do ticket durante a negociação e confirmação da venda com atualização automática do estoque.

**Sincronização entre canais:** qualquer venda (site ou vendedor) precisa refletir instantaneamente no estoque compartilhado, para que os dois fluxos nunca vendam o mesmo ticket.

**Controle de acesso e permissões:** diferenciação entre o que o cliente vê no site (só disponível/preço) e o que o vendedor vê no painel (mais detalhes, possivelmente margem, histórico do cliente etc.).

**Integração de pagamento:** gateway de pagamento para o site (self-service) e, idealmente, opção de gerar link de pagamento pelo vendedor no fluxo consultivo.

**Log/auditoria de vendas:** histórico de todas as transações (quem vendeu, quando, canal) para relatórios e reconciliação financeira.

**Escalabilidade por evento:** o sistema precisa suportar fácil cadastro de novos eventos (estrutura de dias/seções/setores configurável), já que hoje cada evento vira uma planilha nova.

