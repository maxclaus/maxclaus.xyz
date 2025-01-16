+++
title = "Imprimir uma parte da página com prévia de impressão"
+++

Plugin JQuery desenvolvido por mim, para impressão de uma parte selecionada de p gina web com prévia de impressão em uma nova janela.

Demo: [WindowPrint](http://www.maxcnunes.com/demos/windowprint/ "Demo WindowPrint")

Download: [Projeto Demo WindowPrint](http://www.maxcnunes.com/demos/windowprint/WindowPrint.rar "Download")

O código HTML - index.html

```html
<!DOCTYPE html PUBLIC -//W3C//DTD XHTML 1.0 Transitional//EN http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd >
<html xmlns= http://www.w3.org/1999/xhtml >
<head id= Head1 runat= server >
<title>Window Print</title>
<script src="Js/jquery-1.4.1.js" type="text/javascript"></script>
<script src="Js/jquery-JCustom-Print.js" type="text/javascript"></script>
</head>
<body>
<form id="form1" runat="server">
<script type="text/javascript">
$(document).ready(function () {
$('.buttonPrint').WindowPrint();
});

</script>
<div id="divPrint" style="text-align: center;">
Cras rutrum, nulla quis facilisis hendrerit, mauris augue bibendum urna, a ultrices
libero leo in quam.
<br />
Nullam ultrices cursus leo nec venenatis. Suspendisse facilisis, massa in pretium
pellentesque, nisi sem pretium ipsum, et dapibus diam orci non arcu. Mauris venenatis
fringilla mauris mollis condimentum.
<br />
<img src="Img/Penguins.jpg" width="250" />
<br />
Phasellus arcu enim, lacinia eu vestibulum vitae, congue eu nunc. Curabitur nec
metus sapien. In a dolor enim.
<br />
Ut ultrices ultrices est eget suscipit.
<br />
</div>
<a href="javascript:;">Imprimir Conteudo da Div ID = divPrint </a>
</form>
</body>
</html>
```

Aplicando o plugin ao botão de impressão

```js
$(document).ready(function () {
  $(".buttonPrint").WindowPrint();
});
```

Caso sua dive possua outro ID, basta passar um novo parametro:

```js
$(document).ready(function () {
    $('.buttonPrint').WindowPrint({ area: #MeuID });
});
```

Lista de parametros opcionais:

1.  **area**: `#divPrint`, Seletor Css do conteudo a ser impresso
2.  **title**: `Imprimir`,  Nome da pagina prévia
3.  **srcImgHead**: `http://www.google.com.br/intl/en\_com/images/srpr/logo1w.png`, Origem da imagem do cabeçalho
4.  **srcIconePrint**: `http://cdn1.iconfinder.com/data/icons/aspneticons\_v1.0\_Nov2006/print\_16x16.gif`,  Origem do icone de impressão
5.  **head**: `true`,  Se possui o cabeçalho na impressão
6.  **windowStatus**: `false`, Se estar visivel na nova janela o Status
7.  **windowScrollbars**: `true`,  Se estar visivel na nova janela a Barra de Rolagem
8.  **windowToolbar**: `false`,  Se estar visivel na nova janela o ToolBar
9.  **windowWith**: `0`,  Largura da janela, se for 0, pega automatico
10. **windowHeight**: `0`,  Altura da janela, se for 0, pega automatico
11. **srcCSS**: ` `,  Origem do css da p gina

