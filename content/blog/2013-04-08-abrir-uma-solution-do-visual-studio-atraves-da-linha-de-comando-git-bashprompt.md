+++
title = "Abrir uma Solution do Visual Studio através da linha de comando (Git Bash/Prompt)"
+++

Como todos já sabem eu gosto muito do controle de versão GIT. E eu gosto de utilizá-lo através do GIT Bash porque é possível realizar mais operações do que qualquer outra ferramente com interface para GIT e porque através da linha de comando é fácil de executar operações e automatizá-las com scripts.

Desde que comecei a utilizar o GIT o comportamento mais comum quando vou mexer em algum projeto é abrir antes de tudo o GIT Bash e acessar o repositório do projeto. O que eu fazia em seguida era executar o comando abaixo para abrir no windows explorer no diretório atual e por último abrir a Solution do projeto.

```
explorer .
```

Mas já que estamos trabalhando com linha de comando podemos automatizar um pouco esse processo através de um dos scripts abaixo:

```
# Open-a-Visual-Studio-Solution-from-a-command-Prompt-Batch-File-And-Navigate-to-Directory.bat
cmd.exe /K "cd full/path/to/MySolutionFolder/ && start MySolution.sln /D ."

# Open-a-Visual-Studio-Solution-from-a-command-Prompt-Batch-File.bat
cd full/path/to/MySolutionFolder/ && start MySolution.sln /D .

# Open-a-Visual-Studio-Solution-from-a-Git-Alias-And-Navigate-to-Directory
alias open-my-project='cd "full/path/to/MySolutionFolder/" && start MySolution.sln /D .'
#example:
#alias open-mp='cd "C:/Users/max.nunes/Projects/myproject/" && start myproject.sln /D .'
```

Explicando cada solução:

1.  A primeira solução é a que realmente eu tenho utilizado, que consiste em criar um alias no GIT para navegar para o diretório do meu projeto e abrir o Solution no Visual Studio. Para utilizá-la apenas digite o nome do alias no Git Bash.
2.  A segunda solução realiza o mesmo da primeira, mas é especifica para o Prompt do windows. Para utilizá-la crie um arquivo .bat no diretório inicial do seu Git Bash e execute-o pelo nome do .bat.
3.  A terceira solução apenas abre a Solution no Visual Studio, sem navegar para o diretório da solução. Podendo ser utilizada tanto no Git Bash quanto no Prompt do Windows. Para utilizá-la faça da mesma forma da segunda solução.

