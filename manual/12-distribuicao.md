# 12. Entrega e versões

> Como uma mudança sai do repositório e chega à produção. Num produto fechado e hospedado, o
> dono é quem recebe a mudança, e o cliente a recebe por ele. Este capítulo cobre imagens,
> versões, notas de versão e o corte. O capítulo mantém o nome de arquivo original para não
> quebrar as referências cruzadas.
>
> **Estado:** capítulo reescrito para produto fechado. Os erros citados vêm dos dois primeiros
> produtos, que distribuíam código para instalação de terceiros. As regras que ainda valem para
> entrega hospedada estão marcadas, e as novas são **não provadas**.

## A pergunta que decide tudo

**Quando isto muda, o que o cliente percebe, e como se volta?** O cliente nunca paga a
construção, e nunca recebe uma versão que ninguém testou como ele a usa.

## Decisões do dono

1. **Onde as imagens ficam.** Num registro **privado**, em organização do dono. O endereço é
   decidido antes do primeiro commit (capítulo 10).
2. **Quem corta uma versão maior.** Só o dono. Nenhum agente e nenhuma automação.
3. **O que o cliente vê de cada versão.** Uma página de novidades, avisos por e-mail ou nada. É
   decisão de produto, e muda o que as notas precisam dizer.

## Imagens

### 1. Todo serviço de produção aponta para uma imagem publicada

Nenhum serviço é construído na máquina de produção. A instrução de construir pode existir ao lado,
e nunca sozinha.

*O erro:* no primeiro produto, um dos serviços não tinha imagem publicada. Era construído na
máquina de cada cliente, e depois disso nenhuma atualização o reconstruía. Ele nunca era
atualizado.

*Como se cobra:* uma cerca reprova serviço de produção sem imagem.

**Mudança em relação à versão aberta.** Construir no ambiente de produção deixa de ser uma saída
de emergência: num produto hospedado, o próprio dono construiria sobre o serviço no ar. Em
emergência, constrói-se no CI e publica-se.

### 2. Publicar é ato do CI

Nunca da máquina de alguém. Uma imagem construída num processador diferente do da produção não
roda lá, e a falha só aparece no ambiente real. O CI constrói para a arquitetura da produção, e
uma contribuição que quebra a construção de qualquer imagem é reprovada antes de entrar. O job
não só constrói: sobe cada imagem e confere que o processo inicia (lição D8).

### 3. A implantação aponta para número de versão

Uma etiqueta móvel significa uma coisa diferente a cada dia. Produção presa a ela não sabe o que
está rodando, e homologação e produção "iguais" não são.

| Etiqueta | Significa | Quem usa |
|---|---|---|
| Número da versão | Imutável | Produção e homologação |
| Topo do ramo principal | O que acabou de entrar | Quem desenvolve |

Não existe canal estável móvel em produção. A política de baixar acompanha a etiqueta: imutável,
baixa se faltar; móvel, baixa sempre.

*Como se cobra:* a implantação grava o número da versão, e a rota de versão (capítulo 11, regra
10) devolve esse número.

### 4. A etiqueta nasce no CI, de um commit do ramo principal, e nunca se move

Conserto é a versão seguinte. Etiqueta movida quebra a implantação que já a tinha, com uma
mensagem que aponta para outro lugar. O repositório restringe quem cria etiqueta de versão.

### 5. O nome da imagem e o formato da versão têm uma fonte só

O que o CI publica e o que a implantação pede são lidos do mesmo lugar. No segundo produto, um
lado escrevia a versão com um prefixo e o outro sem, e a atualização nunca encontrava a imagem
(lição E11).

### 6. Dependência de terceiros é referenciada com versão fixa

E a base de cada imagem que o produto constrói também. Nunca se republica software de terceiros
sem conferir a licença dele.

## Versões

### O número responde a uma pergunta

**O que o dono e o cliente precisam saber ou fazer?**

Cada mudança que alguém percebe traz um fragmento: um arquivo pequeno numa pasta própria. Ele
declara o efeito, e o número é calculado a partir do conjunto.

| O fragmento declara | Versão |
|---|---|
| Nada mudou para quem usa (conserto) | Correção |
| Dá para fazer algo que antes não dava | Menor |
| O cliente precisa fazer algo | Menor, com o aviso em destaque |
| — | Maior: só por decisão do dono |

*Por que um arquivo por mudança:* um arquivo de notas editado por todos dá conflito em toda
contribuição paralela, e um número digitado depende de alguém lembrar o último. É a regra de um
arquivo por peça (capítulo 05) aplicada às notas.

### "Exige ação" é um campo, e não um número

Isso é dito num bloco próprio do fragmento, com o passo exato. Não se usa o número maior para
avisar.

*O erro:* no primeiro produto, um aviso de ação necessária saiu rebaixado, porque marcá-lo
forçaria uma versão maior que ninguém queria cortar.

Num produto hospedado, a ação raramente cabe ao cliente: ela é do dono (migrar dados, trocar
um segredo). Quando cabe ao cliente (reconectar uma conta de anúncio, por exemplo), vira aviso
dentro do produto, e não só nota de versão.

### O fragmento é escrito para quem lê

Um parágrafo, na voz de quem usa e nunca viu o código. O título vira a linha da nota: confere-se
que ele afirma o que a mudança faz, porque a máquina só valida a forma.

### Forma, e não presença

O CI confere que todo fragmento é bem formado. Não reprova contribuição sem fragmento: isso
barraria atualização automática de dependência e a própria contribuição de versão. A presença é
cobrada na revisão.

## Cortar uma versão

Dois atos, os dois pelo CI.

1. **Abrir.** Um comando lê os fragmentos, calcula o número, monta a seção das notas, apaga os
   fragmentos e abre uma contribuição de versão.
2. **Cortar.** Aceitar essa contribuição cria a etiqueta e publica as imagens no registro
   privado. **A implantação em produção é um terceiro ato, separado:** a versão passa antes por
   homologação (capítulo 11).

Regras:

- **Só se corta com o ramo principal verde.**
- **Nada entra durante o corte.** O congelamento é feito pela máquina.
- **A nota é gerada do que está na etiqueta,** e não do que se lembrava.
- **O corte não depende de ser o último envio.** O passo que cria a etiqueta decide pelo commit
  que mudou a versão nas notas, e não pelo envio que o disparou.
- **Uma arquitetura lenta não segura a outra,** quando houver mais de uma.
- **O corte confere os documentos de entrada** (README interno, documento de autoridade, roteiro
  de quem chega): nenhuma frase de estado contradiz o que a versão entrega.

## Implantar uma versão em produção

A implantação (capítulo 11) é quem troca a versão. As regras daqui:

- **Homologação primeiro,** com dados parecidos com os de produção e um cliente de teste por
  papel.
- **Um cliente, uma versão ao mesmo tempo.** Se o produto liberar por etapas (primeiro poucos
  clientes), a etapa está declarada, e uma organização nunca vê duas versões numa mesma tela.
  **Não provado:** os dois produtos não liberavam por etapas.
- **A volta é um ato testado,** e não um plano. A versão anterior continua disponível no
  registro.
- **Aviso de incidente sai com a correção,** com uma forma de o cliente conferir se foi afetado,
  e de acordo com os termos de uso (capítulo 02).

## Documentação para o cliente

**Vídeo divulga; a documentação mora no repositório.** O "como fazer" do cliente é escrito,
conferido contra o produto a cada versão, e todo vídeo aponta para ele.

*O erro:* no primeiro produto o vídeo era a documentação, e envelhecia mais rápido que o
produto.

## Idiomas

Documentação e tela nascem na língua do público principal, com tradução para as outras que o
dono declara. Os identificadores do código ficam em inglês. No primeiro produto, o pedido de
outros idiomas veio depois e custou um retrabalho em toda a tela; no segundo, os dicionários
existem desde o primeiro commit.

## Como saber que ficou pronto

- O arquivo de composição de produção não tem serviço sem imagem.
- A implantação grava um número de versão.
- Uma versão foi cortada inteira pelo CI, sem ninguém digitar número.
- A versão anterior foi atualizada para ela em homologação, com dados.
- A volta foi exercitada.
- A nota de versão diz, para cada item, o que muda para quem usa.

## O que mudou em relação à versão aberta

Saíram: instalação por canal móvel, etiqueta apontada pelo instalador do cliente, imagens que
baixam sem credencial, várias arquiteturas por padrão, a atualização pela tela com agente no
servidor do cliente, a descoberta de versão nova pela instalação e a divulgação pública das
notas. Entraram: registro privado, homologação, volta exercitada, liberação por etapas (não
provada). O texto da versão aberta está no primeiro commit deste repositório.

## Origem

Doutrinas de empacotamento e de versionamento e registro de decisão de distribuição do
DeskcommCRM; fragmentos, workflows de versão e de imagens do DeskVoice; revisão fria do
DeskVoice. Regra de liberação por etapas e de aviso de incidente ao cliente: adaptação para
produto hospedado, sem erro medido ainda.
