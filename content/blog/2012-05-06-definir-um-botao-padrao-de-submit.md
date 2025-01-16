+++
title = "Definir um botão padrão de submit"
+++

O artigo de hoje é super simples, mas eu passei por uma situação nessa semana, na qual quando eu teclava o botão ENTER  em um formulário de pesquisa o evento disparado era do botão de limpar ao invés do botão pesquisar. Isso era uma problema, porque a maioria dos usuário já estão acostumados com esse processo e quando realizassem nessa tela iriam ficar frustrados com o resultado.

Então eu deveria definir qual o botão seria disparado quando eu teclasse ENTER dentro do formulário.  Para isso basta definir no PageLoad o nome do botão desejado, como padrão para o formulário atual:

`this.Form.DefaultButton = btnPesquisar.UniqueID;`

Obs: Durante a minha pesquisa, alguns usuários relatavam que funcionava passando o ClientID ao invés do UniqueID, o que eu acho um pouco improvável, porque para um formulário o mais importante é o name. Mas de qualquer forma, você pode tentar essas duas possibilidades.

