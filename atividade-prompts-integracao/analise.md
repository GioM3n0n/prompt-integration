# Análise dos prompts e reflexão

## Como os tipos de prompt influenciaram os casos

**Prompt zero-shot:** facilitou a identificação de fluxos funcionais e dependências entre os serviços. Ao receber detalhes de contrato e regras de negócio, os casos ficaram mais específicos e verificáveis, cobrindo consultas de saldo, reservas e confirmação de pedidos.

**Prompt few-shot:** os exemplos ajudaram a orientar o formato e o foco da geração. A versão refinada ampliou a cobertura para falhas de rede, validação, consistência transacional, idempotência e concorrência, além de exigir a checagem de efeitos colaterais indevidos.

## Ajustes mais importantes

1. Especificar endpoints, parâmetros e os códigos HTTP **assumidos para o exercício**.
2. Pedir saídas estruturadas com pré-condições, entradas, interações e resultados esperados.
3. Exigir verificações de que erros não provoquem pedidos confirmados ou reservas duplicadas.
4. Incluir exemplos concretos para induzir cenários de falha e casos menos óbvios.
5. Instruir explicitamente a LLM a **não inventar execuções** ou métricas de aprovação.

## Limitações

Os casos são propostas textuais de teste de integração, não evidências de que uma integração real funciona. A GenAI pode sugerir hipóteses plausíveis, mas o contrato e as regras precisam ser revisados por quem conhece o sistema. Nenhuma API foi desenvolvida e nenhum teste foi executado neste exercício.
