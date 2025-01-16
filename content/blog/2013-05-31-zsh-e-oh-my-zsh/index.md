+++
title = "ZSH e Oh My ZSH"
+++

### ZSH

O **ZSH** é um shell para Unix que vai fazer você não querer mais usar qualquer outro shell além dele.

### O Oh My ZSH

Existe um [projeto no github](https://github.com/robbyrussell/oh-my-zsh) chamado "**O Oh My ZSH**" que contêm plugins e configurações para customizar o ZSH que aumentam ainda mais a usabilidade neste terminal. Outro detalhe legal é a possibilidade de alterar temas do terminal: [exemplos](https://github.com/robbyrussell/oh-my-zsh/wiki/themes).

[![OhMyZsh](./OhMyZsh.png)](http://blog2.maxcnunes.com/wp-content/uploads/2013/05/OhMyZsh.png)

Para ter uma ideia melhor de uma olhada nesse vídeo: [http://vimeo.com/57123291](http://vimeo.com/57123291) (em inglês)

#### Instalando ZSH e Oh My ZSH no Ubuntu

1.  Instalar ZSH:

    sudo apt-get update && sudo apt-get install zsh

2.  Configurar oh-my-zsh:

    wget –no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O – | sh

3.  Tornar ZSH terminal padrão:

    chsh -s /bin/zsh

4.  Reinicie o ubuntu.

