+++
title = "Jquery e UpdatePanel"
+++

Problemas com a inicialização das suas funções em jquery, que não carregam após algum postback vindo de um updatepanel?

Facil resolver.

Ao invés de você usar o famoso _**$(document).ready()**_

```js
$(document).ready(function () {
  alert("Olá mundo!");
});
```

Devemos utilizar o _**function pageLoad()**_

```js
function pageLoad() {
  alert("Olá mundo!");
}
```

O assunto mais a fundo você encontra aqui: [http://encosia.com/2009/03/25/document-ready-and-pageload-are-not-the-same/](http://encosia.com/2009/03/25/document-ready-and-pageload-are-not-the-same/)

