# Adaptações da Fábrica para o ThothAds (produto fechado)

> A Fábrica foi escrita para produtos open source com parceiro de infraestrutura. O ThothAds é
> um SaaS fechado e pago, hospedado pelo dono. A Fábrica não exige obediência: exige que cada
> divergência seja uma decisão escrita. Este arquivo é essa decisão, em lote.
>
> **Nada do manual original foi apagado ou reescrito.** O texto da Fábrica fica como veio do
> original; o que vale para o ThothAds é este arquivo mais o original. A licença MIT e os
> créditos do original continuam em `LICENSE`.
>
> **Estado: proposta de 13/11/2026.** Cada linha marcada "proposta" precisa ser confirmada pelo
> dono na fase 3 (decisões e desenho). Só as duas primeiras decisões abaixo são do dono.

## Decisões do dono

1. O produto é **fechado e pago**. Não há versão gratuita nem repositório público do `app/`.
2. Cada cliente pagante é uma **organização**. Vazamento entre organizações é o fim do produto.

## O que vale como está

Capítulos 01, 03 (critério de escolha de stack), 04, 05, 06, 07, 08, 09, 14 e todo o
`licoes.md`. Em `conformidade.md`, os itens 1.x, 3.1, 3.3, 3.5, 4.x, 5.x, 6.x, 7.x, 8.x, 9.x e
13.x. O capítulo 07 (multiempresa) passa a ser o mais importante do produto: os itens 7.1 a
7.10 valem como **nível F sem exceção**.

## O que muda (todas "proposta")

| Itens | Original assume | No ThothAds | Situação |
|---|---|---|---|
| 2.1 a 2.6, 2.17 a 2.19 | Parceiro de infraestrutura declarado; código gratuito; "caminho genérico nunca sabotado" | Não há distribuição do código. Se houver parceiro ou afiliado, é decisão nova do dono. | Não se aplica, até o dono decidir |
| 2.7 | Telemetria: nenhuma ou com consentimento | Vale, em forma de política de privacidade e termos de uso | Adaptado |
| 2.8 a 2.10, 2.12, 2.13, 2.16 | Custos do instalador e teto por organização | Vale e fica mais forte: teto de gasto por organização para custo de IA e de provedores | Mantido |
| 2.11, 2.14, 2.15 | Marca própria por revenda; caso real | Decidir na fase 3 | Em aberto |
| 3.2 | O sistema sobe sem conta de terceiros | SaaS hospedado pode depender de provedores. Cada dependência exige registro de decisão com a saída | Decidido diferente |
| 3.4 | Memória e disco medidos numa VPS | Medidos no ambiente de produção do SaaS | Adaptado |
| 10.1, 10.5, 10.6, 10.10, 10.13 | Repositório em organização, proteção do ramo, skill de contribuição | Valem | Mantido |
| 10.2, 10.3, 10.4, 10.7, 10.8, 10.9, 10.11, 10.12 | Contribuição pública: CONTRIBUTING, acolhida, triagem, SLA | Sem contribuição externa. `SECURITY.md` e `GOVERNANCE.md` ficam como documentos internos | Não se aplica |
| 11.1 a 11.3, 11.11, 11.13, 11.17, 11.19 a 11.25 | Instalação em uma linha na VPS de terceiros | Quem instala é o dono. Não há instalador para cliente | Não se aplica |
| 11.5, 11.6, 11.8, 11.10, 11.12, 11.14 | Saúde, backup conferido por restauração, diagnóstico mascarado | Valem, para a produção do dono. Backup e restauração por organização entram na fase 3 | Mantido |
| 11.9, 12.12 a 12.15, 12.21 | Atualização pela tela, com agente no servidor do cliente | O dono atualiza. Migração segura em produção (com banco já em uso) continua obrigatória | Não se aplica / adaptado |
| 12.1 a 12.5, 12.16, 12.18 | Imagens públicas, arquiteturas, baixar sem credencial | Imagens em registro **privado** | Adaptado |
| 12.6, 12.7, 12.9, 12.17, 12.19, 12.20 | Etiqueta só do ramo principal, notas por fragmento, corte pelo CI | Valem | Mantido |
| 14.x (site) | Site do projeto open source | Vira site comercial; os itens de paridade entre o produto e a página de instalar não se aplicam | Adaptado |
| 15.1 a 15.6 | Equipe autônoma de agentes | Valem. 15.5 e 15.6 valem com ou sem equipe | Mantido |

## Capítulo novo: contas de anúncio dos clientes

Entra no desenho da fundação (fase 3) como capítulo 15 do manual do ThothAds, com itens na
conformidade. Cada regra precisa de erro de origem, e por isso começa como **"não provado"**,
até um produto rodar. O ponto de partida:

- Token de acesso a conta de anúncio: cifrado em repouso, por organização, nunca em log nem em
  endereço.
- Teto de gasto por organização e por período. Teto somado, além do teto por unidade.
- Autonomia em níveis: **A** (recomenda e rascunha), **B** (aplica com tetos e aprovação das
  mudanças grandes), **C** (sem aprovação). Nível C só com decisão escrita do dono.
- Toda mudança na conta do cliente passa por um plano de mutação, com desfazer, idempotência e
  verificação independente. Registro de auditoria só de acréscimo.
- Relatório mostra `pass`, `fail`, `unknown` ou `não aplicável`. "Desconhecido" nunca é
  adivinhado.
- Origem do conhecimento: `conhecimento/claude-ads/` (somente leitura). O que for reaproveitado
  entra no produto reescrito, com o crédito no arquivo de licenças do `app/`.

## Como manter

- `upstream` é o único remoto do fork. Para puxar melhorias do original:
  `git fetch upstream` e revisar antes de mesclar.
- Toda mudança do original que altere item adaptado aqui reabre a linha correspondente.
