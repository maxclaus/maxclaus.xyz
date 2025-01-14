+++
title = "Definir um botão padrão de submit"
+++

<p>O artigo de hoje é super simples, mas eu passei por uma situação nessa semana, na qual quando eu teclava o botão ENTER &nbsp;em um formulário de pesquisa o evento disparado era do botão de limpar ao invés do botão pesquisar. Isso era uma problema, porque a maioria dos usuário já estão acostumados com esse processo e quando realizassem nessa tela iriam ficar frustrados com o resultado.</p>
<p>Então eu deveria definir qual o botão seria disparado&nbsp;quando eu teclasse ENTER dentro do formulário. &nbsp;Para isso basta definir no PageLoad&nbsp;o nome do botão desejado, como padrão para o formulário atual:</p>
<p><code lang="csharp">this.Form.DefaultButton = btnPesquisar.UniqueID;</code></p>
<p>Obs: Durante a minha pesquisa, alguns usuários relatavam que funcionava passando o ClientID ao invés do UniqueID, o que eu acho um pouco improvável, porque para um formulário o mais importante é o name. Mas de qualquer forma, você pode tentar essas duas possibilidades.</p>
