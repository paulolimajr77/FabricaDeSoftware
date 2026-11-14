# Adaptações da Fábrica para o ThothAds (produto fechado)

> A Fábrica original foi escrita para produtos open source com parceiro de infraestrutura. O
> ThothAds é um SaaS fechado e pago, hospedado pelo dono. A Fábrica não exige obediência: exige
> que cada divergência seja uma decisão escrita. Este arquivo é essa decisão, em lote.
>
> **Como ler o histórico.** O primeiro commit deste repositório é a cópia do original, sem
> nenhuma alteração. Cada commit seguinte altera um capítulo, e `git diff <primeiro commit>`
> mostra tudo o que é do ThothAds. A licença MIT e os créditos do original continuam em
> `LICENSE`.
>
> **Estado:** capítulos 02, 11, 12, 13 e 14 adaptados, capítulo 15 novo, `conformidade.md`
> marcada, README, instalador e skills desvinculados do endereço público. **As adaptações são
> proposta** até o dono confirmá-las na fase 3 (decisões e desenho). Só as duas decisões abaixo
> são do dono.

## Decisões do dono

1. O produto é **fechado e pago**. Não há versão gratuita nem repositório público do `app/`.
2. Cada cliente pagante é uma **organização**. Vazamento entre organizações é o fim do produto.

## Capítulos

| Capítulo | O que foi feito |
|---|---|
| 00 método, 01 visão, 03 stack, 04 design system, 05 engenharia, 06 autenticação, 07 multiempresa, 08 administração, 09 doutrinas | Sem alteração. O 07 passa a ser o mais importante do produto: os itens 7.1 a 7.10 valem como nível F sem exceção |
| 02 modelo de negócio | **Reescrito.** Sai o parceiro, o passo zero, o guia do implementador e os três eixos de dinheiro. Entram cliente pagante, custo por organização com teto, custo por cliente conhecido antes de vender, conta de terceiros, preço fora do repositório |
| 10 contribuição | **Sem alteração no texto.** Contribuição pública não existe no produto fechado. Valem repositório em organização, proteção do ramo e skill de contribuição para a equipe. Os itens 10.3, 10.7, 10.8, 10.9, 10.11 e 10.12 não se aplicam |
| 11 instalação | **Reescrito como implantação hospedada** e cadastro de cliente. Mantém todas as regras de "pronto só depois de conferir", saúde, backup conferido, volta automática, segredos separados |
| 12 distribuição | **Reescrito como entrega e versões.** Registro de imagens privado, versão por número, homologação antes de produção, volta exercitada. Sai a atualização pela tela |
| 13 site | **Reescrito como site comercial.** Preço lido da fonte, segurança e dados, termos e privacidade. Sai instalar, guias e parceiro |
| 14 equipe autônoma | **Seção nova de adaptação.** Separa a equipe que mantém o produto dos agentes que o produto vende. Equipe sem credencial de produção nem dado de cliente fora de pedido de suporte |
| 15 contas de anúncio | **Novo.** Tudo marcado como não provado, com a fonte de cada regra |

## Conformidade

`conformidade.md` mantém todos os itens e marca, na própria linha, o que mudou:

- **32 itens não se aplicam** ao produto fechado: 2.1 a 2.6, 2.9, 2.12, 2.17 a 2.19; 10.7 a
  10.9, 10.11, 10.12; 11.1, 11.11, 11.13, 11.17, 11.24; 12.12 a 12.16, 12.18, 12.21; 14.4, 14.5,
  14.10, 14.11.
- **19 itens são adaptados:** o jeito de conferir vale para o equivalente hospedado (instalação
  lida como implantação, registro público como registro privado).
- **1 item é decidido diferente:** 3.2, o sistema sobe sem conta em serviço de terceiros. Um
  produto hospedado pode depender de provedores, e cada dependência exige registro de decisão
  com a saída.
- **Seção 16 nova,** com 16 itens para o capítulo 15, todos não provados.
- Os demais itens valem como estão.

## Fora do manual, também adaptado

- `install.sh` não tem mais origem pública por padrão. Só `--fonte`, ou um repositório privado
  por variável. **Não rode o instalador do original:** ele traz a Fábrica sem as adaptações.
- `README.md` e as duas skills apontam para esta cópia, e não para o endereço público.
- `referencia.md` continua apontando para o DeskVoice, que é um repositório público e serve só
  como leitura de como cada mecânica foi feita.

## Em aberto, do dono

- Parceiro, afiliado ou revenda: não existe. Se o dono decidir ter, é decisão nova e texto novo.
- Plataformas do primeiro lançamento, preço e forma de cobrança (decisões do capítulo 02).
- Nível de autonomia dos agentes: o ponto de partida é A (recomenda e rascunha), e C não entra
  sem decisão escrita.

## Como manter

- Esta cópia não tem remoto. Se o original mudar e valer a pena puxar uma melhoria, compara-se à
  mão e aplica-se como commit novo, com a origem na mensagem.
- Toda mudança do original que altere um item adaptado aqui reabre a linha correspondente.
