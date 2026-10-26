# GenAI aplicada a testes de integração

Atividade acadêmica — Unidade III.

## Objetivo

Demonstrar como dois tipos de prompt podem apoiar a geração de **casos de teste de integração**, sem implementar uma API real.

## Cenário hipotético

Uma **API de Pedidos** recebe solicitações de compra e se comunica com uma **API de Estoque** para verificar e reservar produtos.

Contrato utilizado como **premissa de exemplo** (não corresponde a uma API existente):

- `POST /pedidos` recebe `clienteId`, `produtoId`, `quantidade` e opcionalmente `chaveIdempotencia`.
- `GET /estoque/{produtoId}` consulta a disponibilidade.
- `POST /reservas` tenta reservar unidades de um produto de forma atômica.
- Respostas esperadas do serviço de Pedidos: `201` (pedido criado), `404` (produto não encontrado), `409` (estoque insuficiente), `422` (entrada inválida) e `503` (serviço de Estoque indisponível).

O comportamento em situações de erro e o uso de idempotência foram definidos para permitir o exercício; em um projeto real, devem ser validados com a documentação do sistema.

## Organização do repositório

- [`prompts.md`](prompts.md) — dois tipos de prompt, versões iniciais e ajustes.
- [`casos-de-teste.md`](casos-de-teste.md) — 12 casos de teste de integração gerados como proposta.
- [`analise.md`](analise.md) — comparação dos resultados e reflexão sobre o uso da GenAI.

## Situação dos casos

Os casos foram **propostos com auxílio de uma LLM (ChatGPT)**. Não há implementação nem execução de testes automatizados. Os resultados esperados representam hipóteses de contrato de API.
