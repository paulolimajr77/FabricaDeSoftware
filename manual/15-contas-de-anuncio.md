# 15. Contas de anúncio de clientes

> Capítulo para produto que age em nome do cliente na conta que ele tem num terceiro, com
> dinheiro dele. O exemplo que o motivou é a gestão de tráfego pago por agentes de IA. Vale
> igualmente a qualquer produto que lê ou altera a conta do cliente num provedor externo.
>
> **Estado deste capítulo: não provado.** Nenhum dos dois primeiros produtos agiu em conta de
> anúncio de terceiros. As regras abaixo vêm de três fontes: das regras de isolamento, custo e
> auditoria dos capítulos 05, 07 e 08, que têm erro medido; do desenho do projeto de referência
> `claude-ads`, que só lê e rascunha e por isso marca a escrita como o nível de maior risco; e do
> raciocínio sobre o que dá errado quando um agente mexe em dinheiro alheio. Cada regra diz de
> qual fonte vem. **Nenhuma está validada por produto rodando.** Ao rodar o primeiro, a
> experiência volta para `licoes.md`, e este aviso sai.

## O problema

Um produto que mexe na conta de anúncio de um cliente combina quatro riscos, e cada um já tem
precedente isolado:

1. **Dinheiro do cliente.** Um erro gasta verba real, sem desfazer.
2. **Credencial de terceiro.** O token de acesso vale mais que a senha do próprio produto.
3. **Isolamento.** A conta de um cliente nunca aparece para outro (capítulo 07).
4. **Autoridade delegada a um agente.** Quem aprovou o quê, e dentro de que limite.

## Decisões do dono

1. **Nível de autonomia de cada tipo de mudança** (regra 3). O ponto de partida é recomendar e
   rascunhar; cada degrau acima é uma decisão escrita.
2. **Quais plataformas, na ordem.** Cada plataforma exige acesso aprovado à API, com revisão do
   provedor. **Não verificado:** o prazo e as condições de aprovação de cada plataforma. É
   levantamento da fase 2.
3. **Quem aprova mudança grande:** o cliente, o gestor humano do cliente ou o dono.
4. **O teto padrão de gasto por organização,** e quem pode alterá-lo.
5. **O que o produto nunca faz sozinho,** em nenhum nível, e que entra nos termos de uso.

## O ciclo que os agentes cobrem

As etapas de quem gere tráfego, e o que cada uma pede em termos de autoridade:

| Etapa | O que o agente faz | Autoridade mínima |
|---|---|---|
| Configuração | Entende o negócio, a meta, a conversão principal, o orçamento | Leitura do cadastro |
| Auditoria | Mede a conta contra uma lista, com evidência por item | Leitura da conta |
| Plano | Canal, verba, medição, concorrência | Nenhuma escrita |
| Criação | Texto, imagem, vídeo, estrutura da campanha | Nenhuma escrita (rascunho) |
| Lançamento | Cria ou altera objetos na conta | **Escrita** |
| Monitoramento | Ritmo de gasto, entrega, medição, fadiga, política | Leitura; pausar sob regra, se permitido |
| Otimização | Lance, orçamento, público, criativo | **Escrita** |
| Teste | Desenha e lê um experimento | Rascunho; escrita ao rodar |
| Relatório | Resultado, com o que passou, falhou e faltou | Leitura |

Só duas etapas pedem escrita, e é nelas que as regras abaixo pesam.

## Os níveis de autonomia

Da referência `claude-ads`, adaptados. Cada tipo de mudança está num nível, e o cliente vê o
nível de cada um.

| Nível | O agente | Exige |
|---|---|---|
| **A. Recomenda** | Recomenda e escreve o rascunho. Uma pessoa aplica | Evidência e revisão do diff |
| **B. Aplica com limites** | Aplica mudanças pequenas dentro de tetos declarados. O resto pede aprovação | Tetos, desfazer, monitoramento, auditoria |
| **C. Aplica sem aprovação** | Cria e altera campanha e verba sozinho | Plano de mutação completo, verificação independente, resposta a incidente. **Só por decisão escrita do dono** |

O nível da conta é o mais alto que está ativo, mas o mapa por tipo de mudança é o que decide.
Um mesmo cliente pode ter "pausar anúncio" no nível B e "criar campanha" no A.

## As regras

### 1. A conta é do cliente, e o acesso é o menor que serve

*Fonte: raciocínio.* O produto pede acesso pela autorização da plataforma, com o menor escopo que
a etapa precisa. Auditoria só lê, e por isso o primeiro acesso é de leitura. Escrita é um
segundo acesso, pedido quando o cliente liga o nível B. A conta sai com o cliente: ao cancelar, o
produto revoga o acesso.

### 2. Token de terceiro é segredo de organização

*Fonte: capítulos 06 e 07, regra de cifra de segredos.* O token fica cifrado em repouso, por
organização. Nunca aparece em log, em endereço, em mensagem de erro nem em resposta de rota. A
aplicação só o decifra ao falar com o provedor, e só pela porta da organização dona dele.

*Como se cobra:* um teste lista toda tabela que guarda token e confere que a leitura sem a
organização falha; uma cerca reprova o token em log.

### 3. O nível de autonomia é por tipo de mudança, e nasce no A

*Fonte: raciocínio, apoiado no desenho do `claude-ads`.* Toda conta nova começa em A. Subir de
nível é ato do cliente, na tela, com o que muda dito em linguagem simples e o teto dito junto.
Cair de nível é imediato e não pede confirmação. O nível C não aparece na tela até o dono
decidir por escrito que existe.

### 4. Toda escrita passa por um plano de mutação

*Fonte: `claude-ads` (contrato de plano de mutação).* Antes de alterar a conta, o agente produz
um plano: o que muda, em qual objeto, o valor antigo e o novo, a evidência que o justifica, e como
se desfaz. A aplicação lê o plano e só ele. Um agente nunca chama o provedor com um parâmetro que
não está no plano.

### 5. Tetos, e a mais restritiva vence

*Fonte: capítulo 02, regra 3.* Cada organização tem teto de verba total por período, variação
máxima de verba por mudança, número máximo de mudanças por dia, e lista do que nunca se altera
sozinho. O produto nunca aplica uma mudança que passa de qualquer teto, mesmo que o agente peça e
o plano esteja correto. Um teto é do dono, outro é do cliente, e vale o menor.

### 6. Aprovação humana por classe de mudança, e com identidade

*Fonte: capítulo 08.* Mudança fora do nível ou acima de um teto pede aprovação de quem tem
papel para isso. A aprovação diz a quem, quando e o que foi aprovado, e só vale para o plano
exato: um plano alterado depois pede nova aprovação. Aprovação não é herdada de uma mudança
parecida.

### 7. Quem executa não verifica

*Fonte: capítulo 14 e a lição "quem implementa não verifica".* Depois de aplicar, um verificador
independente lê a conta no provedor e confere que o resultado é o do plano. Divergência abre um
incidente e pausa o nível B daquela organização.

### 8. Toda mudança se desfaz, ou o plano diz que não

*Fonte: raciocínio.* O plano guarda o valor antigo. O desfazer é uma ação do produto, com o
mesmo registro. Mudança que não se desfaz (excluir uma campanha, por exemplo) está fora dos
níveis A e B por padrão.

### 9. O registro de auditoria é só de acréscimo

*Fonte: capítulo 07, regra 11.* Cada leitura relevante, plano, aprovação, aplicação e
verificação é registrada, por organização, com quem, quando e o quê. Nem o papel de sistema
altera nem apaga. O cliente vê o registro da conta dele.

### 10. Leitura sem evidência é "desconhecido"

*Fonte: `claude-ads` (contrato de status).* Todo item de auditoria termina em passou, falhou,
desconhecido ou não se aplica. **Desconhecido é uma resposta real:** o dado faltou, e o agente não
adivinha. Um relatório mostra o que passou, o que falhou e o que faltou, e mostra a data da
evidência.

### 11. Controladores conflitantes são declarados

*Fonte: `claude-ads` (classificador de delegação).* Quando a plataforma já automatiza algo
(lance automático, orçamento dinâmico) e o produto também quer alterar, existem dois
controladores. O produto detecta, declara ao cliente e não escreve no que a plataforma controla.

### 12. Os agentes do produto só veem a organização em que trabalham

*Fonte: capítulo 07.* Todo acesso a dado, a ferramenta e a provedor de IA leva a organização
da sessão. O agente nunca recebe contexto, exemplo ou memória de outra organização. O que o
produto aprende com um cliente não vira instrução para outro sem decisão registrada, e dado
identificável nunca atravessa.

*O erro que isto fecha, medido no primeiro produto:* um vazamento entre organizações veio por um
cliente de serviço que contorna a política de linha. Um agente que age com poder de serviço é o
mesmo caminho, com mais autonomia.

### 13. O modo simulado cumpre o contrato do provedor real

*Fonte: capítulo 05, regra do modo simulado.* Testes e demonstração usam um provedor simulado
que reproduz também o comportamento: limites de taxa, erros, atraso, o que acontece ao fim de um
orçamento. O desvio que ainda existe está escrito junto do substituto. Nunca se testa contra a
conta real de um cliente.

### 14. Dado do cliente fora do produto

*Fonte: raciocínio.* O produto declara que dado de conta de anúncio vai para o provedor de IA,
e qual. Sem prova de que o provedor não treina com ele, o dado identificável é encurtado antes
do envio. **Não verificado:** os termos de cada provedor de IA sobre isso. É levantamento da
fase 2.

### 15. Incidente tem dono, prazo e comunicação

*Fonte: raciocínio.* Um gasto fora do plano, uma conta alterada sem aprovação ou um vazamento
são incidentes. Cada um tem dono, uma forma de pausar tudo (pausa geral da organização e pausa
geral do produto), e um aviso ao cliente afetado no prazo dos termos de uso.

## Como saber que ficou pronto

- Uma conta de teste de cada plataforma foi ligada em leitura, auditada e desligada, com o
  acesso revogado.
- Um plano foi aplicado no nível B, verificado, e desfeito.
- Uma mudança acima do teto foi recusada pelo produto, mesmo com plano correto.
- Um token foi procurado em log, e não apareceu.
- Uma organização tentou ler a conta de outra por todas as rotas de agente, e falhou.
- A pausa geral parou uma organização, e depois o produto inteiro, em minutos.

## Não provado

Tudo o que está neste capítulo. Em especial: a viabilidade do acesso à API de cada plataforma,
os tetos que de fato protegem sem impedir o trabalho, e se a verificação independente acha
erros que o executor não acha. O primeiro produto fechado que rodar este capítulo devolve o que
mediu.

## Origem

Regras 2, 5, 6, 9 e 12: capítulos 02, 06, 07 e 08, com erro medido nos dois primeiros produtos.
Regras 4, 10 e 11: contratos e referências do projeto `claude-ads`, que só lê e rascunha.
Regras 1, 3, 8, 14 e 15: raciocínio, sem erro medido. Capítulo escrito na adaptação da Fábrica
para o ThothAds.
