<img align=left width=200 src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Slack_Technologies_Logo.svg/2000px-Slack_Technologies_Logo.svg.png" /><br clear=left>

Durante os próximos dias nós vamos explorar a criação de um [Slackbot](https://translate.google.com.br/translate?hl=&sl=en&tl=pt&u=https%3A%2F%2Fwww.entrepreneur.com%2Farticle%2F302409) usando Python. É claro que exitem várias plataformas de terceiros por aí que podem automatizar o processo de criação de chat-bots. Mas, como engenheiros de software emergentes, entendemos que, mergulhando profundamente e explorando os detalhes, adquirimos um rico conhecimento de _como_ as tecnologias que nos ajudarão no futuro funcionam.  A _Fase 1_ não é uma tarefa pequena, você vai precisar pesquisar, planejar, experimentar, tudo isso enquanto concilia com outras atividades que fará também essa semana. 

Esta tarefa integra vários conceitos vistos nas últimas semanas. Além disso, depois que terminar esta atividade, vai poder no futuro adaptar para outras necessidades.

## Objetivos do Aprendizado
 - Sentir-se à vontade com a pesquisa e experimentação de APIs auto-guiadas
 - Crie, teste, implemente e gerencie programas na nuvem
 - Aplicar boas práticas na estrutura do repositório
 - Usar o __virtual enviornment__ em um projeto
 - Aprenda a integrar APIs

## Objetivo
O objetivo é criar um Slackbot que responda aos comandos do usuário e se integre a uma API secundária para adquirir dados. A escolha da API secundária depende do aluno e pode ser qualquer API aberta que forneça alguns dados interessantes que possam estar presentes em um canal slack e que não exija uma assinatura paga.  Uma lista de APIs públicas gratuitas está disponível [aqui](https://github.com/toddmotto/public-apis)

### Setup - Slack
Para esta tarefa, usaremos duas contas privadas e separadas da equipe Slack. Essas contas são usadas apenas pelas coortes SE para esta atribuição, não há outros usuários. O motivo do uso de duas contas separadas é porque as contas da equipe do Slack free limitam o número de integrações de aplicativos a no máximo 10. Solicite ao seu instrutor que envie os links de autoinscrição quando estiver pronto. Depois de entrar, envie uma mensagem ao seu instrutor para solicitar privilégios de administrador. Você precisará de `admin` para criar um aplicativo de bot.


### Part A: Slackbot Client
 - Criar um virtual environment para seu projeto
 - Instale a biblioteca python [slackclient](https://python-slackclient.readthedocs.io/en/latest/) em seu virtual environment.
 - Implemente seu slackbot usando uma classe python. O código inicial está disponível em [slackbot.py](slackbot.py)
 - Conecte seu aplicativo a um dos espaços de trabalho do KenzieBot como um usuário de bot
 - Envie uma mensagem para um canal padrão anunciando que seu bot está online
 - Aguarde e processe eventos/mensagens em um loop while infinito
 - Ignore todas as mensagens que não contenham uma `@menção` direta 'do nome do seu bot
 - Crie um analisador de comandos que possa agir sobre qualquer comando direcionado ao seu bot
 - Certifique-se de implementar um comando `help` que lista todos os comandos que seu bot entende
 - Implemente o comando `ping` que mostrará o tempo de atividade do bot
 - Implemente um comando interno de autoteste que irá `raise` diferentes tipos de exceções dentro do seu bot. Isso ajudará a "proteger" seu bot contra erros imprevistos e testar seu caminho de recuperação.   
 - Saia do seu programa bot se você receber um comando de saída. Por exemplo, se o seu bot se chama `example-bot`, seu programa deve sair normalmente quando receber uma mensagem de folga como`@example-bot exit`

### Part B: API Integração
Agora que você possui um cliente Slack que responde aos comandos, conecte-o a outra API. Busque uma foto, preço das ações, previsão do tempo ou relatório de tráfego. Faça o bot renderizar os dados de volta ao canal slack.


### Detalhes de Implementação

NOTA: Esta seção pressupõe que você já criou uma conta Heroku a partir de atribuições anteriores e que você tem as [ferramentas Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) instaladas em sua máquina de desenvolvimento local. 
A maioria dos exemplos de implantação em python do Heroku na Internet pressupõe que você esteja desenvolvendo um aplicativo Web, mas, neste caso, está implantando um script python autônomo simples sem uma estrutura ou [gateway WSGI](https://www.ullstackpython.com/wsgi-servers.html).
Para implantar seu bot no Heroku, seu repositório do github deve conter alguns arquivos especiais chamados `Procfile` e` runtime.txt`. Procfile informa ao Heroku qual programa executar quando você ativa sua instância gratuita do dinamômetro. O conteúdo do arquivo de busca deve ficar assim:
    worker: python slackbot.py

O Runtime.txt informa ao Heroku qual versão do interpretador python usar quando ele cria uma imagem do docker para o seu slackbot. Você deve selecionar uma versão que corresponda à usada no desenvolvimento em seu ambiente virtual. Veja quais versões python são suportadas pelo Heroku [aqui](https://devcenter.heroku.com/articles/python-runtimes). Exemplo de conteúdo de `runtime.txt`:

    python-3.7.2

Seu aplicativo slackbot foi projetado para ser de longa duração; portanto, parece natural tentar implantá-lo em uma plataforma de hospedagem em nuvem como o Heroku. No entanto, o Heroku limita os dynos de hospedagem em nuvem de nível gratuito a um tempo de atividade máximo de 18 horas por período de 24 horas. Portanto, seu bot será forçado a eutanásia de vez em quando (a menos que você atualize para o nível Hobby - US $ 7,00 / mês).

    heroku create my_unique_slackbot_name
    git push heroku master

Lembre-se de que seu arquivo .env deve conter seus tokens da API do slackbot e não deve fazer parte do seu repositório (ou seja, `.env_` deve estar listado em seu `.gitignore`). Você precisará copiar seus tokens de API diretamente nos vars de configuração do Heroku:

    heroku config:set BOT_USER_TOKEN="xoxb-431941958864-124971466353-2Ysn7vyHOUkzjcABC76Tafrq"
    

Agora tudo deve estar pronto para ser executado. Inicie seu slackbot e verifique os logs:

    heroku ps:scale worker=1

## Notas de Orientação
Você enviará um link para um repositório do github, mas criará e administrará esse repositório por conta própria, em vez de criar um repositório Kenzie. Lembre-se de que seu trabalho no github se tornará seu próprio portfólio pessoal que você deseja exibir para recrutadores e potenciais empregadores. Com isso dito, aqui estão algumas práticas recomendadas que procuraremos em seu repositório:

### Boas Práticas
 - PEP8: Sem warnings
 - if `__name__ == "__main__"` Python idiom
 - Docstrings e #comments para funções e modulos
 - `__authors__` = "Seu Nome"
 - EStrutura não monolítica (classes e métodos concisos) que aderem principalmente à [responsabilidade única](https://medium.com/@angelomribeiro/princ%C3%ADpio-da-responsabilidade-%C3%BAnica-6d633087fa4e)
 - Legibilidade, manutenção

### Práticas Recomendadas de Desenvolvimento
 - Colabore com seu companheiro de equipe. Use o VSCode Liveshare para sessões de emparelhamento
  - Crie e use um ambiente virtual de projeto
  - Instale todos os novos pacotes no seu virtualenv
  - Configure seu IDE para usar o intérprete em seu ambiente virtual
  - Use seu IDE e depurador para executar, avançar e visualizar
  - Use variáveis de ambiente local para tokens e chaves de API
  - Use o log python (não imprima instruções) para todas as mensagens de saída.

### Melhores Práticas para Repositório 
 - Tenha um `README.md` de nível superior descritivo. Se você não sabe como é um bom README, pesquise no Google "melhores práticas do README"
 - `.gitignore` está presente, ignorando o `.vscode/`, `.env`, `.log` e `venv/`
 - `requirements.txt` do `pip freeze`
 - Sem hard-coded API keys ou tokens em qualquer lugar.  [**NÃO VAZAR TOKENS**](https://translate.google.com.br/translate?sl=en&tl=pt&u=https%3A%2F%2Flabs.detectify.com%2F2016%2F04%2F28%2Fslack-bot-token-leakage-exposing-business-critical-information%2F)
 - [Commits Pequenos](https://blog.hartleybrody.com/git-small-teams/) com mensagens significativas, e não "mais alterações" ou "blah foo bar" ou "asdfadfadfadfadfasdfasdf"
  - Não envie arquivos de log ou envs virtuais para o repositório!

**Logging** - Seu aplicativo Slackbot deve fazer logon no console e em um arquivo. Eventos de baixa frequência devem ser registrados no nível INFO e de alta frequência no DEBUG. Manipuladores de exceção devem fazer logon em ERROR ou acima para qualquer coisa não tratada. Outros níveis são auto-explicativos. Use uma variável de ambiente para selecionar o nível de saída do registro quando o seu Bot for iniciado. Registre eventos de inicialização e desligamento, bem como informações de conexão do cliente frouxas e quaisquer eventos de desconexão. Registre todas as mensagens que seu Bot recebe e envia para a API do Slack. Gerencie o log de arquivos com algum tipo de rotação de tempo ou exclusão de agendamento para que os logs não cresçam sem limites. O módulo de log do Python possui maneiras integradas de fazer isso.

**OS Signal Handling** - Seu bot deve lidar com SIGTERM e SIGINT, como na atribuição Dirwatcher. Registre todos os sinais do SO que o seu Bot receber. Em uma implantação gratuita do Heroku, seu programa receberá um SIGTERM pelo menos uma vez por dia, quando desejar que o seu bot durma. Aproveite essa oportunidade para fechar facilmente todas as conexões abertas com o Slack ou outra API e enviar uma mensagem de despedida.

**Exception Handling** - O bot nunca deve sair, a menos que seja solicitado (por comando do usuário ou sinal do SO). Seu bot deve lidar com exceções na ordem do mais detalhado (restrito, específico) ao mais amplo. Exceções não tratadas (aquelas sem manipuladores específicos) devem registrar rastreamentos de pilha completos. Busque alta disponibilidade executando seu Bot em um ambiente de teste local pelo maior tempo possível. Temos um servidor de desktop linux no local para isso. Proteja-se contra interrupções de Wi-Fi e desconexões caprichosas de seus clientes de folga e twitter, inserindo manipuladores de exceção para casos específicos que encontrar. Quando você capturar uma exceção não tratada, faça uma pausa por alguns segundos antes de reiniciar seu loop. Não faça spam nos logs com uma tonelada de mensagens "Reiniciando ..." - aguarde alguns instantes para o gerente de processos do SO enviar ao seu Bot um SIGINT ou SIGTERM se o monitor do processo achar que o Bot está se comportando mal. Você pode testar seu manipulador de exceções adicionando um comando bot especial de seu próprio design, que gerará manualmente qualquer exceção de dentro do seu programa. Dica: Isso usa a função Python `raise`.

## Demos
As demonstrações do grupo de seus projetos de slackbot são TBD, dependendo da rapidez com que o final do trimestre está se aproximando. Consulte o seu instrutor se desejar demonstrar um recurso extra interessante que você adicionou ao seu aplicativo. Caso contrário, veremos principalmente os frutos do seu esforço de projeto diante de nós, nos espaços de trabalho frouxos do KenzieBot e KenzieBot2.

## Final Words
Essa tarefa reúne muitos conceitos que você aprendeu nos meses anteriores. Embora não seja um projeto do SE, ele tem um valor significativo em pontos e incentivamos sua equipe a começar cedo. Boa sorte!!
