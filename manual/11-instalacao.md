# 11. Instalação e implantação

> O produto é hospedado pelo dono, e ninguém mais o instala. O que na versão aberta era a
> experiência de instalar vira aqui a implantação em produção e o cadastro de um novo cliente.
> As regras de fundo continuam: nada é dado como pronto sem conferir, e uma mudança que funciona
> na máquina de quem desenvolve e quebra no ambiente real é defeito de produto.
>
> **Estado:** capítulo reescrito para produto fechado. Os erros citados vêm dos dois primeiros
> produtos, que eram instalados por terceiros; a prova de que cada regra vale também para
> implantação hospedada é **não provada** até o primeiro produto fechado rodar.

## A promessa

Duas, e as duas são do dono:

1. **Implantar uma versão nova em produção** sem derrubar o serviço, com volta automática se
   quebrar.
2. **Entrar um cliente novo** do cadastro até a primeira tela útil, sem passo manual do dono.

## Decisões a registrar

| Decisão | Ponto de partida |
|---|---|
| Quais ambientes existem | Produção e homologação, no mínimo. Homologação roda a mesma implantação que a produção |
| Onde o produto roda | Decisão de stack (capítulo 03). Um registro de decisão diz o que o dono assume operar |
| Como se implanta | Por script do repositório, sem passo manual. Nada de editar arquivo na máquina |
| A implantação volta sozinha se quebrar? | Sim |
| Onde fica o backup, e quem o confere | Fora da máquina de produção, conferido por restauração |
| Como entra um cliente novo | Cadastro cria a organização, o dono da organização e o primeiro acesso, sem intervenção |

## O que existe

Uma pasta com nome genérico (`deploy/`), no repositório do produto.

| Peça | Faz | Confere antes de dizer que deu certo |
|---|---|---|
| Implantação | Constrói ou baixa a imagem, aplica as migrations, troca a versão | Cada peça essencial respondendo |
| Checagem de saúde | Uma linha por peça e por ligação entre peças | Não resume |
| Backup | Copia o banco e a configuração | Restaura num banco de rascunho e conta as linhas |
| Restauração | Substitui o banco por um backup, com confirmação digitada | Compara as contagens com o manifesto |
| Volta | Retorna à versão anterior e restaura o banco | A saúde passa de novo |
| Diagnóstico | Gera um arquivo para análise interna | Segredos mascarados, dados pessoais encurtados |
| Recuperação de acesso | Troca a senha de um dono de dentro do servidor | Encerra as sessões |
| Cadastro de cliente | Cria organização, dono e primeiro acesso | A tela de entrada aceita o usuário criado |
| Testes do kit | Rodam sem depender do ambiente real | Saem com erro se algo falhar |

## As regras

### 1. Nada que exija editar arquivo na máquina

Nem na implantação, nem para ligar um recurso. Variável nova nasce com padrão, ou é a
implantação que a acrescenta. O arquivo de ambiente só guarda o que a implantação gera.

*Como se cobra:* uma cerca reprova variável de ambiente sem padrão que não esteja na lista do
que a implantação gera.

### 2. Rodar de novo não quebra nada

A implantação retoma de onde parou e nunca troca um segredo que já existe. Ela tem um modo que
mostra o que faria sem alterar nada, e um modo sem perguntas, que torna o processo testável.

### 3. "Pronto" só depois de conferir

Nenhum script diz que deu certo sem ter verificado o resultado.

*Os erros:* no primeiro produto, o instalador dizia "concluído" com o site fora do ar; o backup
dizia "concluído" sem parte dos dados; a restauração dizia "restaurado" sem restaurar.

A conferência usa o mesmo caminho que o cliente vai usar. Um contêiner "saudável" por uma
verificação interna não prova que o domínio responde.

### 4. A saúde confere as ligações, e nunca resume

Cada peça de pé não significa que elas se falam. A checagem tem uma linha para cada peça e uma
para cada ligação entre peças, com três estados: funcionando, aviso e falha. O código de saída
distingue falha de peça essencial de falha de peça secundária, para servir a monitoramento.

*O erro:* na primeira instalação real do segundo produto, todos os serviços estavam de pé e um
não conseguia ler o que o outro gravava. A capacidade central não funcionava, e nada dizia.

### 5. A implantação grava com permissão explícita

Tudo o que um serviço vai ler é gravado com dono e permissão declarados. Não se depende da
configuração da máquina nem do usuário que rodou.

*O erro:* quem instalava tinha restringido as permissões padrão da sessão, e o instalador
herdou. Nenhum teste viu, porque os testes usavam um substituto do Docker.

*Como se cobra:* o teste do kit roda a implantação com as permissões padrão restritas.

### 6. Segredos separados por quem usa

Os segredos são divididos para cada serviço receber só o que usa. A aplicação não recebe a senha
de dono do banco. A senha de um usuário nunca passa como argumento de comando.

Segredos de contas de terceiros dos clientes (capítulo 15) seguem uma regra a mais: cifrados em
repouso, por organização, e nunca em registro de log.

### 7. A implantação tem backup antes e volta automática

Faz backup, troca a versão, aplica as migrations e confere a saúde. Se uma peça essencial não
responde, volta para a versão anterior e restaura o banco.

*O erro:* no primeiro produto, atualizações deixaram instalações presas numa página de
manutenção, e houve atualização sem backup.

**Migração com dados de clientes.** Num produto hospedado, o banco de produção sempre tem dados
de organizações. O CI aplica a mudança sobre um banco da última versão, com dados (lição A3),
e uma migração que não se desfaz é decisão registrada antes de ir a produção.

### 8. Uma implantação por vez

Duas implantações simultâneas no mesmo ambiente derrubam o serviço. A implantação confere uma
trava e recusa rodar se já há uma em curso.

### 9. O que a implantação chama é testado pela linha de comando

Um comando usado pelo script é testado como o script o chama, com os argumentos de verdade. O
teste da função interna não vê o erro de argumento.

### 10. A versão em execução se vê de fora

Uma rota diz que versão está rodando, lendo o que está em execução. Nenhuma tela afirma "é a
mais recente" com dado guardado.

### 11. A mensagem de erro diz a causa e o próximo passo

Toda falha termina com o que aconteceu e o comando a rodar. Um defeito de infraestrutura nunca
aparece para o cliente como outra coisa: no primeiro produto, o banco sobrecarregado aparecia
como "e-mail ou senha incorretos".

### 12. Toda porta publicada tem uso, e o navegador usa uma origem só

Cada porta aberta para fora serve a algo que funciona. Tudo o que a tela usa chega pelo mesmo
endereço do painel, encaminhado pelo proxy.

*O erro:* no segundo produto, o dono abriu o painel por túnel com uma porta só. A capacidade
nova usava outra, e a tela respondeu "confira sua internet".

### 13. Configuração inválida é recusada, e nunca ignorada

Um valor ou uma opção que o ambiente não vai obedecer faz a implantação falhar, nomeando a
opção. O valor é validado em todo modo. O endereço que a implantação anuncia no fim é o que ela
testou.

*O erro:* no segundo produto, uma opção ignorada em silêncio publicou o painel para fora.

### 14. A implantação termina provando que a pessoa entra

A última coisa é conferir que a tela de entrada abre e aceita um usuário de teste. O mesmo vale
para o cadastro de cliente novo: termina conferindo que o dono da organização entra.

*O erro:* na comunidade do primeiro produto, o relato era "a instalação conclui e a tela de
login nunca abre".

### 15. Cadastro de cliente é automático e isolado

Criar uma organização não toca em nenhuma outra. O cadastro cria a organização, o dono e o
primeiro acesso, e o teste de isolamento (capítulo 07) roda com a organização recém-criada.

**Não provado:** os dois produtos eram instalados, e não hospedavam várias organizações novas por
dia. A regra vem do capítulo 07 e espera o primeiro produto.

### 16. O que o cliente pulou no cadastro tem lugar na tela

Se o cliente pulou uma credencial ou uma resposta, existe um lugar evidente no painel para
informar depois.

### 17. O padrão do ambiente sustenta produção

Nenhum padrão aponta para um recurso que não aguenta uso real sem dizer isso na mesma tela
(capítulo 02, regra 4).

## Como a implantação é provada

| Nível | O que prova | O que não prova |
|---|---|---|
| Testes do kit, com um substituto do ambiente | A lógica dos scripts: validações, retomada | Nada sobre o ambiente real |
| CI: banco do zero e atualização com dados | O banco chega ao mesmo estado pelos dois caminhos | Rede, permissões, proxy |
| CI: as imagens constroem e sobem | Os processos iniciam | Que se falam |
| **Homologação** | O produto, implantado como em produção | — |
| **Primeira implantação em produção** | O produto que o cliente usa | — |

Os dois últimos níveis são a fase 8 do método, e não têm substituto.

## Como saber que ficou pronto

- A implantação rodou duas vezes seguidas sem efeito na segunda.
- Um backup foi restaurado em outra máquina e o sistema voltou.
- Uma implantação que quebra de propósito voltou sozinha.
- A checagem de saúde acusou uma ligação cortada de propósito.
- Um cliente de teste foi cadastrado do zero e entrou, sem passo manual.

## O que este capítulo não resolve

- Escolha do provedor de hospedagem e de pagamento.
- Plano de continuidade (o que acontece se a máquina de produção cair e como se volta).
- Retenção dos dados de um cliente que cancela.

## O que mudou em relação à versão aberta

Saíram: a entrada de uma linha, as perguntas ao instalador, o proxy já existente, a arquitetura
de processador do cliente, a porta de entrada para quem não tem servidor, e todo o papel do
parceiro. Entraram: ambientes, cadastro automático de cliente, migração com dados, trava de
implantação. O texto da versão aberta está no primeiro commit deste repositório.

## Origem

Kit de instalação e primeira instalação real do DeskVoice; kit, doutrina de empacotamento e
issues de instalação do DeskcommCRM. Regras 8, 15 e 16: adaptação para produto hospedado, sem
erro medido ainda.
