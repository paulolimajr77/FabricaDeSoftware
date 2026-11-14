# 02. Modelo de negócio

> O produto é fechado e pago. O dono hospeda, cobra uma assinatura de cada cliente e responde
> pelo serviço. Este capítulo diz o que o dono decide antes de fundar, que regras de dinheiro o
> produto cumpre, e o que mudou em relação à versão aberta da Fábrica.
>
> **Estado:** capítulo reescrito para produto fechado. A parte herdada do original (custo
> medido, teto, caminho recomendado) vem de erros medidos nos dois primeiros produtos. A parte
> nova, de cobrança e de conta de terceiros do cliente, é marcada **Não provado** até um
> produto rodar com ela.

## O modelo, em quatro frases

1. O software é fechado. O código não é distribuído e o repositório é privado.
2. O dono hospeda o produto e responde pela disponibilidade, pelos dados e pelo isolamento de
   cada cliente.
3. Cada cliente pagante é uma organização, e paga uma assinatura ao dono.
4. Os custos de terceiros que o produto gera por cliente (IA, provedores, envio) são medidos
   por organização, têm teto, e entram no preço.

## Decisões do dono

Estas são **entrada obrigatória da fundação**.

1. **Quem é o cliente pagante?** O dono de um negócio, uma agência ou um profissional
   autônomo. A resposta muda o produto: uma agência tem várias contas de clientes dela dentro de
   uma organização, e isso pede um nível a mais de isolamento (capítulo 07).
2. **Como se cobra?** Assinatura fixa, por uso, ou por parcela do que o cliente gasta nos
   provedores. Quem paga o custo de IA: está dentro do preço ou é repassado.
3. **Qual é o primeiro lançamento?** Quais capacidades e quais plataformas externas entram, e
   quais ficam de fora do horizonte.
4. **Há parceiro, afiliado ou revenda?** Se sim, é decisão nova do dono, com texto e regra
   próprios. A Fábrica não traz um padrão.
5. **O que o produto promete e o que ele nunca faz** em nome do cliente. Em produto que mexe
   no dinheiro ou na conta de terceiros do cliente, esta lista é parte do contrato.

## O teste de entrada de uma ideia

Cinco perguntas, antes de fundar.

| Pergunta | Por que importa para este modelo |
|---|---|
| Economiza ou gera dinheiro para quem paga? | É o que justifica a assinatura e segura o cliente depois do primeiro mês |
| O cliente nota o resultado sem explicação? | Cancelamento acontece quando o valor não aparece sozinho |
| Tem motivo para ficar ligado o tempo todo? | É o uso contínuo que sustenta uma receita recorrente |
| O custo por cliente é conhecido e cabe no preço? | Sem isso, cada cliente novo pode dar prejuízo |
| Mantém-se sem o dono disponível todo dia? | **Eliminatória.** Se não, simplifique antes de avançar |

## Os públicos

| Público | O que quer | O que o produto dá a ele |
|---|---|---|
| Cliente (organização) | Resultado e controle do que é feito em nome dele | Tela para tudo, custo e gasto visíveis, aprovação das mudanças grandes |
| Equipe do cliente | Trabalhar sem pedir ajuda ao dono | Papéis, convite, histórico do que cada pessoa fez |
| O dono | Receita previsível e poucos incidentes | Painel da instalação, tetos por organização, trilha de auditoria |

Não há público de implementador, contribuidor externo nem parceiro de instalação: o produto
não é instalado por terceiros.

## As regras

### 1. Cada cliente é uma organização, e o dinheiro é contado por organização

O preço, o teto de gasto, o uso e a fatura são de uma organização. Nenhum número do produto é
global quando serve a uma decisão sobre um cliente. Uma medição sem organização soma clientes
distintos e devolve um número plausível sobre um mundo que não existe.

*Como se cobra:* os itens de multiempresa (capítulo 07) e o teto da regra 3.

### 2. Sem recurso escondido atrás do plano sem aviso

O que o plano inclui está escrito na página de preço e na tela do cliente. O produto nunca
corta uma capacidade que o cliente já usa sem aviso e sem prazo.

**Não provado:** a Fábrica não mediu nenhum erro de plano e cobrança. A regra nasce de risco
reconhecido, e vira lição na primeira falha medida.

### 3. O custo de terceiros é medido, por organização, e tem teto

Tudo o que o produto gasta com provedor aparece na tela, por unidade de trabalho, e respeita um
limite que o operador define. São dois limites: um por unidade (uma chamada, uma execução) e um
somado (por organização e período, ou de unidades simultâneas). Só o primeiro deixa um usuário
logado multiplicar o gasto abrindo várias unidades de uma vez.

*O porquê:* custo imprevisível de IA foi uma das reclamações mais repetidas no primeiro produto,
e a capacidade que virou o segundo produto não entrava no limite de gasto.

Num produto fechado, o teto protege primeiro o dono: o custo de IA de um cliente que abusa sai
do caixa dele.

### 4. O custo por cliente é conhecido antes de vender

Antes do lançamento, mede-se quanto custa por mês atender um cliente típico, em uso de verdade,
e onde cada limite gratuito de um provedor acaba. O preço mínimo sai dali. O padrão do ambiente
sustenta produção; onde não sustenta, diz isso.

*O erro:* o caminho recomendado do primeiro produto usava a camada gratuita de um serviço que
não aguentava uso real, e quem descobria já tinha cliente dentro.

### 5. Conta de terceiros é do cliente, e a autoridade sobre ela é escrita

Quando o produto age na conta de um terceiro em nome do cliente (anúncio, caixa de mensagens,
calendário), valem três regras: a conta é do cliente e sai com ele; o produto declara o que
pode ler e o que pode alterar; cada alteração tem registro de quem pediu, o que mudou e como se
desfaz. O detalhe está no capítulo 15.

**Não provado:** nenhum dos dois produtos agiu em conta de anúncio de terceiros. As regras
nascem do capítulo 15 e esperam o primeiro produto.

### 6. Telemetria é política de privacidade

O produto mede o próprio uso para operar, e o cliente sabe o que é medido. Os termos de uso e a
política de privacidade nomeiam quem é responsável pelos dados e quem os opera. O documento
legal nomeia o dono do produto, e nunca a marca de um cliente.

*O erro:* o primeiro produto nasceu com telemetria ligada por omissão e corrigiu para
consentimento.

### 7. Marca do cliente, se existir, é configuração

Se o cliente puder usar a própria marca no produto, ela é configuração em tempo de execução, e
sobrevive à atualização. As lições vêm do primeiro produto: uma imagem serve todas as marcas, o
que está no banco vence o que está no ambiente, e o código que resolve a marca nunca falha:
degrada para o padrão do produto.

### 8. Preço não entra no repositório

Valores, metas e qualquer número de negócio ficam fora do repositório do produto, mesmo
privado. O repositório diz *como* se calcula; o número mora em outro lugar.

## Como saber que ficou pronto

- As decisões do dono 1 a 5 estão escritas.
- Existe a medição de custo por cliente, com método e data.
- Cada custo de provedor aparece por organização na tela e respeita um teto.
- A política de privacidade e os termos nomeiam o responsável pelos dados.
- A página de preço diz o que cada plano inclui, e o produto cumpre.

## O que este capítulo não resolve

- Modelo de cobrança concreto, e o provedor de pagamento.
- Regra para parceiro, afiliado ou revenda, se o dono decidir ter.
- O que acontece com os dados do cliente quando ele cancela.
- Política de reembolso e de inadimplência.

## O que mudou em relação à versão aberta

A versão original deste capítulo assumia código gratuito, um parceiro de infraestrutura
declarado e instalação feita por terceiros. Saíram: o parceiro e a regra do passo zero, o
instalador condicional, o guia do implementador, os três eixos de dinheiro e o kit de revenda.
Ficaram, com a origem original: o custo medido e com teto, o custo real do caminho
recomendado, a marca como configuração e a telemetria declarada. O texto completo da versão
aberta está no primeiro commit deste repositório.

## Origem

Custo e teto: relato do primeiro e do segundo produto. Marca e telemetria: documentos do
primeiro produto. Regras 2, 5 e 8 e as decisões do dono: adaptação para produto fechado, sem
erro medido ainda.
