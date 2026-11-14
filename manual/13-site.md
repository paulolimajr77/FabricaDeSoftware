# 13. Site

> O site apresenta o produto a quem pode virar cliente, e leva até o cadastro. O que se repete
> entre produtos: quase nada no site é escrito no site. Cada página tem uma fonte, e a ligação
> com a fonte é vigiada.
>
> **Estado:** capítulo reescrito para produto fechado. O método vem do site do primeiro produto,
> que era de um projeto aberto; as regras de ligação com a fonte nasceram do que apodreceu lá.
> Tudo o que é específico de venda (preço, cadastro, prova social, página legal) é **não provado**.

## Estado deste capítulo

Só o primeiro produto tem site. O método abaixo vem do que funciona lá e, em parte igual, do que
apodreceu lá: a lista de guias ficou atrás das skills, a página de instalação tem uma data fixa,
a data da página inicial ficou velha. As regras de ligação com a fonte nasceram desses três.

## Decisões do dono

1. **Quando o site entra.** Num produto pago, antes da divulgação: sem site não há cadastro.
2. **O endereço,** e os idiomas.
3. **A personalidade,** em três palavras, e o que o site nunca faz.
4. **Onde fica o cadastro:** no site ou no próprio produto, e o que um visitante vê antes de
   criar a conta.

## Um repositório separado

O site mora num repositório próprio, e nunca dentro do produto.

*O porquê, que continua valendo:* o site muda por razões comerciais e o produto, por razões de
produto. Misturar os dois acopla o ritmo e deixa a equipe de marketing com acesso ao código do
produto fechado.

## As páginas, e a fonte de cada uma

| Página | O que responde | De onde vem o conteúdo |
|---|---|---|
| Inicial | O que é, para quem, o que resolve, como começar | Escrita no site, a partir de um plano |
| Preço e planos | O que cada plano inclui e quanto custa | **Da fonte:** o catálogo de planos do produto. Nunca digitado nas duas pontas |
| Como funciona | O que o produto faz, passo a passo | Escrita, com cada afirmação apontando para onde conferir |
| Novidades | O que mudou em cada versão | **Lido da fonte:** as notas de versão do produto, na parte que o dono decidiu expor |
| Ajuda | Como usar, tela por tela | **Gerado:** capturado do aplicativo real com dados fictícios (método do curso, abaixo) |
| Segurança e dados | Quem é responsável pelos dados, onde ficam, como se isolam | Escrita, com apontamento para a política de privacidade e os termos |
| Termos e privacidade | O que o cliente aceita | Documento legal, mantido por quem responde por ele |
| Entrar / cadastrar | Leva ao produto | Ligação para o produto. O site não cria contas |

Três tipos de página, e uma regra para cada:

- **Lida da fonte:** nada é escrito no site. Se a leitura falha, a página falha alto, em vez de
  mostrar "nenhuma versão" por cima do conteúdo certo.
- **Gerada:** um script produz, e um recibo diz de qual commit do produto ela saiu.
- **Escrita no site:** só a página inicial e as molduras. Tudo o que nelas afirma um fato sobre
  o produto aponta para onde conferir.

## As regras

### 1. O que pode vir da fonte, vem da fonte

*O erro:* no primeiro produto, a página de guias é uma cópia feita à mão da pasta de skills. Ela
lista menos guias do que o repositório tem. A página de instalação foi "conferida contra o kit"
numa data fixa, e o comando da página inicial é escrito de um jeito e o da página de instalação
de outro.

Num site de produto fechado, a mesma lição vale para **preço e plano**: a página de preço e a
cobrança do produto leem do mesmo catálogo. **Não provado:** nenhum dos dois produtos cobrava.

### 2. O corte de versão confere a consequência

Ao cortar uma versão que o cliente vê, o produto consulta o site e reprova se a versão não
aparecer lá. Confere-se o efeito, e não um passo "atualizar o site" que poderia passar verde sem
mudar nada.

### 3. Toda cópia tem um teste de paridade

O que não der para ler nem gerar, e precisar ser copiado (tokens visuais, o catálogo de planos
enquanto não for lido), tem um teste que compara a cópia com a fonte e reprova a diferença. O
teste roda no CI do site.

### 4. Prova social se busca, e se for verdadeira

Contagem de clientes, de campanhas, de resultado: lida da fonte em tempo de execução, e some da
página se a leitura falhar. Prova social escrita à mão apodrece. **Num produto pago, o número
tem de ser verdadeiro e declarar o recorte:** "clientes ativos hoje", e não "clientes".

### 5. Uma data de atualização, de uma constante só

A data visível vem de um lugar. No primeiro produto ela é fixa dentro de um componente e ficou
meses atrás do conteúdo.

### 6. Cada seção responde uma pergunta de quem visita

A página inicial nasce de um plano em que cada seção é a resposta a uma pergunta real. Seção que
não responde nenhuma sai.

### 7. Todo o conteúdo está no texto servido

A interação é uma camada por cima. Buscadores e assistentes de IA que leem a página não
executam scripts.

### 8. Afirmação técnica aponta para onde conferir, e o site não promete o que não foi medido

Cada "o produto faz X" traz o caminho para o lugar onde se confere. O site não promete resultado
de anúncio, de venda ou de economia que o produto não mediu: prometer desempenho de campanha
sem medição é a falha comercial mais cara de um produto de tráfego.

### 9. Os idiomas têm a mesma estrutura, por tipo e por teste

Um dicionário por idioma, com o mesmo formato obrigado pelo tipo: chave que falta quebra a
construção. Um teste compara a estrutura do que o tipo não cobre. Tradução que encolhe é
conteúdo perdido. As versões não precisam ser literais.

### 10. Os testes do site rodam no CI

Uma suíte que só protege quem lembra de rodá-la não protege ninguém.

### 11. O contrato de marca do site

Um arquivo curto na raiz: quem visita, para que o site serve, a personalidade, o que ele nunca
faz, e a acessibilidade.

### 12. O documento legal nomeia quem responde

A política de privacidade e os termos nomeiam o dono do produto como responsável pelos dados.
Nunca a marca de um cliente (capítulo 02, regra 6).

## A página de ajuda: o método do curso

O material mais elogiado pela comunidade do primeiro produto foi um curso que se percorre, com
as telas reais, em vez de um vídeo. O método continua útil para a ajuda de um produto pago:

1. **Lições em dados.** Um arquivo por lição: a rota, os passos, o texto, a dica, e o que
   destacar na tela, apontado por identificadores estáveis do próprio aplicativo.
2. **Triagem do que mudou.** Um comando compara o commit da última publicação com o atual e diz
   se é preciso recapturar tudo, só candidatos, ou nada.
3. **Bancada isolada.** O aplicativo de verdade, numa construção de teste, com banco local. O
   script recusa rodar contra um banco que não seja local.
4. **Dados de demonstração.** Uma empresa fictícia, semeada por script. **Nunca dados reais de
   cliente.** Isto pesa mais num produto que mexe em conta de anúncio: uma captura com o nome de
   uma campanha real vaza um cliente.
5. **Capturas do aplicativo real,** cada uma com a versão e o commit em que foi tirada. Falha
   deixa a lição pendente, e não com foto velha.
6. **Idiomas.** Onde a interface existe no idioma, captura-se de novo nele.
7. **Montagem.** Um script gera a página. O menu sai do registro de navegação do produto.
8. **Publicação com recibo.** O recibo no produto avança: commit de origem, idiomas, data.

Um efeito colateral que vale o esforço: percorrer o produto tela por tela para o curso achou
uma lista de defeitos de uso.

## A ordem de construção

1. A página inicial e a de preço.
2. Termos e privacidade, antes de qualquer cadastro.
3. As novidades, lidas da fonte, com a conferência no corte de versão.
4. Segurança e dados.
5. A ajuda, quando as telas pararem de mudar toda semana.

## Como saber que ficou pronto

- Uma versão nova do produto apareceu nas novidades sem ninguém tocar no site.
- A página de preço e a cobrança leem o mesmo catálogo.
- A página inicial foi lida com os scripts desligados e todo o conteúdo estava lá.
- Termos e privacidade nomeiam o dono do produto.
- Nenhuma captura da ajuda traz dado de cliente real.

## Não provado

- **Preço e plano lidos da fonte.** Nenhum dos dois produtos cobrava.
- **Prova social lida da fonte com o recorte declarado.**
- **A página de segurança e dados.** Nenhum dos dois produtos tinha cliente que perguntasse isso
  antes de contratar.
- **O teste de paridade rodando no CI do site contra o produto.** Não existe nos dois.

## O que mudou em relação à versão aberta

Saíram: a página de instalar, a de guias de skills, a "para quem desenvolve", a "para quem
implementa", o design system público, o arquivo de resumo com o passo zero, o bloco do parceiro
e o número social de estrelas e contribuidores. Entraram: preço lido da fonte, segurança e dados,
termos e privacidade. O texto da versão aberta está no primeiro commit deste repositório.

## Origem

Repositório do site, plano da página, skill e roteiro do curso, workflow de corte de versão e
doutrina de versionamento do DeskcommCRM; relatos da comunidade do primeiro produto. Regras de
preço, prova social verdadeira e documento legal: adaptação para produto pago, sem erro medido.
