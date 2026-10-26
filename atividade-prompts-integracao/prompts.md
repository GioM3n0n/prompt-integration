# Prompts utilizados e refinamentos

## Tipo 1 — Prompt direto (zero-shot) para cenários funcionais

**Versão inicial:**

> Gere casos de teste de integração para uma API de Pedidos que se comunica com uma API de Estoque.

**Limitação observada na formulação inicial:** o pedido não explicita interfaces, regras de negócio nem o formato de saída; pode resultar em cenários vagos, difíceis de reproduzir.

**Versão ajustada:**

> Considere um cenário hipotético em que `POST /pedidos` recebe `clienteId`, `produtoId` e `quantidade`, consulta `GET /estoque/{produtoId}` e solicita uma reserva por `POST /reservas`. Gere seis casos de teste de integração funcionais, em tabela, com ID, pré-condição, entrada, interação entre os serviços e resultado esperado. Cubra compra com estoque suficiente, estoque exato, pedidos sequenciais, produto inexistente e quantidade maior que o estoque. Considere que reservas são atômicas; não invente resultados de execução.

**Casos relacionados:** CT-01 a CT-06 (arquivo `casos-de-teste.md`).

## Tipo 2 — Prompt com exemplos (few-shot) para erros e resiliência

**Versão inicial:**

> Crie testes de integração para falhas entre as APIs de Pedidos e Estoque. Exemplo: entrada com quantidade zero → retornar erro de validação. Exemplo: Estoque indisponível → Pedido não pode ser confirmado.

**Limitação da formulação inicial:** os exemplos direcionam a LLM para falhas, mas faltam códigos HTTP, verificações de integridade e tratamento de repetição e concorrência.

**Versão ajustada:**

> Usando o contrato hipotético: `POST /pedidos` cria pedidos; `GET /estoque/{produtoId}` consulta saldo; `POST /reservas` reserva unidades. Exemplos de formato: (1) quantidade zero → HTTP 422, sem chamada ao Estoque; (2) produto ausente → HTTP 404, sem reserva. Produza seis casos adicionais de teste de integração voltados a erros e resiliência. Para cada caso, escreva pré-condição, entrada, comportamento esperado das integrações, HTTP esperado e checagem de consistência. Inclua dados obrigatórios ausentes, timeout, falha da reserva, alteração de estoque entre consulta e reserva, requisição repetida com chave de idempotência e solicitações concorrentes. Adote 503 para indisponibilidade e 409 para conflito de estoque. Não afirme ter executado os casos.

**Casos relacionados:** CT-07 a CT-12 (arquivo `casos-de-teste.md`).

> Observação: os códigos e comportamentos são hipóteses estabelecidas no exercício. Em APIs reais, devem ser confirmados no contrato da aplicação.
