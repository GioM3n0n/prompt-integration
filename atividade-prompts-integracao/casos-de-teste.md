# Casos de teste de integração gerados com apoio de GenAI

**Status geral: propostos, não executados.** Cenário e códigos de resposta são hipotéticos.

## Grupo A — Prompt zero-shot refinado (cenários funcionais)

| ID | Cenário e pré-condição | Entrada ou ação | Interações e resultado esperado |
|---|---|---|---|
| CT-01 | Estoque com 10 unidades do produto P1 | `POST /pedidos` com P1, quantidade 2 e cliente C1 | Pedidos consulta Estoque, reserva 2 unidades e retorna **201**; saldo final: 8. |
| CT-02 | Estoque com 3 unidades de P1 | Criar pedido de 3 unidades | Consulta e reserva concluídas; **201**; saldo final: 0. |
| CT-03 | Estoque inicial de 5 unidades de P1 | Dois pedidos sequenciais de 2 e 3 unidades | Ambos retornam **201**; duas reservas distintas; saldo final: 0. |
| CT-04 | Produto P999 não cadastrado | Criar pedido de uma unidade de P999 | Consulta retorna produto inexistente; Pedidos responde **404**, não chama a reserva nem cria pedido. |
| CT-05 | Estoque com 2 unidades de P1 | Criar pedido de 4 unidades | Nenhuma reserva é confirmada; **409**; saldo permanece 2. |
| CT-06 | Estoque com 1 unidade de P1 | Criar pedido de 1 unidade e consultar o estoque após a confirmação | **201** para o pedido; reserva registrada uma única vez; estoque atualizado para 0. |

## Grupo B — Prompt few-shot refinado (erros e resiliência)

| ID | Cenário e pré-condição | Entrada ou ação | Interações e resultado esperado |
|---|---|---|---|
| CT-07 | Corpo da requisição sem `produtoId` | `POST /pedidos` sem o campo obrigatório | **422**; pedido rejeitado antes de consultar ou reservar estoque. |
| CT-08 | API de Estoque não responde durante consulta | Criar pedido válido enquanto ocorre timeout | **503**; pedido não confirmado; nenhuma reserva contabilizada. |
| CT-09 | Consulta indica estoque, mas a operação de reserva falha por indisponibilidade | Criar pedido válido | **503**; nenhum pedido confirmado; verificar que não houve reserva efetivada. |
| CT-10 | Saldo muda entre consulta e reserva: consulta informa 2, mas a reserva detecta saldo 0 | Criar pedido de 1 unidade | API de Estoque recusa a reserva; Pedidos retorna **409**; não cria pedido nem gera saldo negativo. |
| CT-11 | Chave de idempotência habilitada; estoque de P1 igual a 5 | Enviar duas vezes a mesma requisição com a mesma `chaveIdempotencia` | Ambas identificam a mesma operação; apenas uma reserva é feita e o mesmo pedido é referenciado; saldo final: 4. Código da segunda resposta conforme contrato definido. |
| CT-12 | Saldo de P1 igual a 1 | Duas solicitações concorrentes tentando comprar 1 unidade cada | Exatamente uma reserva/pedido é confirmado (**201**); a outra recebe **409**; saldo final: 0 e nunca negativo. |

## Critério de revisão

Antes de transformar essas propostas em testes automatizados, confirmar o contrato real da API, suas regras de transação e idempotência, os códigos de erro, a política de retentativas e os mecanismos de consistência de dados.
