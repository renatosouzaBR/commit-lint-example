## CommitLintExample

Neste projeto eu configurei e testei o **CommitLint** para implantar nos projetos da empresa onde trabalho. Basicamente, o **CommitLint** nos ajuda a estruturar a mensagem de commit, sendo que podemos nos beneficiar dessa estrutura para automatizar processos, como por exemplo, um build.

Para começar você pode consultar a [documentação](https://commitlint.js.org/#/guides-local-setup), ou então seguir este tutorial onde eu explico passo a passo como eu fiz.

Eu criei um novo projeto, sem qualquer código, apenas com arquivos de configuração para testar de forma clara. 

Então, crie um pasta com o nome **CommitLintExample** e depois abra o prompt nesta pasta, e digite os seguintes comandos:

	npm init
	npm install -D @commitlint/{cli,config-conventional}
	npm install -D husky
	
O comando **npm init** vai criar um arquivo package.json em seu projeto, para que você possa instalar as dependências. Já o **npm install -D @commitlint/{cli,config-conventional}** adiciona as dependências do **CommitLint** em dev, e o **npm install -D husky** é uma dependência de dev que nos ajuda a configurar ganchos para o git.

Depois de tudo instalado, execute o comando abaixo para criar a estrutura do git.

	git init
	
E também crie na raiz do projeto o arquivo **.gitignore** e adicione a seguinte linha.

	node_modules
	
Isso faz com que a pasta *node_modules** (que é a pasta onde fica as dependências baixadas) não sejam enviadas em seu commit e para o repositorio.

Agora abra o seu arquivo **package.json** e adicione a seguinte configuração:

	"husky": {
	    "hooks": {
	      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
	    }
  	}
  	
No comando acima configuramos um gancho em que, toda vez que um novo commit for criado, ele será disparado. 

Já configuramos tudo, agora vamos realizar um teste com falha e um com sucesso para vermos o resultado. Antes, para deixar claro, o padrão da mensagem do commit agora com o **CommitLint** é **"tipo: assunto"**. Vamos testar:

Execute os comandos abaixo para adicionar os arquivos, e depois para realizar o commit inicial, com falha:

	git add .
	git commit -m "foo: initial commit"
	
A resposta vai ser:

	git commit -m "foo: this will fail"
	husky > commit-msg (node v10.1.0)
	No staged files match any of provided globs.
	⧗   input: foo: this will fail
	✖   type must be one of [build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test] [type-enum]
	
	✖   found 1 problems, 0 warnings
	ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint
	
	husky > commit-msg hook failed (add --no-verify to bypass)
	
Podemos identificar que o **CommitLint** traz diversas regras para a mensagem, e uma delas é o tipo, sendo **build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test**. Agora vamos trocar **foo** por um tipo válido e ver se tudo ocorre bem.

	git commit -m "feat: initial commit"
	
Esse comando deve ser executado com sucesso. 

**Aqui finalizamos, qualquer coisa entre em contato**.