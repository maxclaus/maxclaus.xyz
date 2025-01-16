+++
title = "Recuperar Query String com Javascript"
+++

Quando se está trabalhando com client side, tudo que for possível manter do lado do cliente e não for comprometer a segurança das informações, é preferível que não passe para o lado do servidor, por diminuir as requisições ao servidor e aumentar dessa forma a performance.

Uma situação bem comum é enviar informações de uma página à outra através de query string na url. Na forma que irei apresentar aqui neste artigo, torna a tarefa de recuperar essas informações no javascript, bem mais simples e prática.

A função a seguir utiliza de replace e regex para preparar a url e filtrar pela chave da query string desejada.

```js
//Função comum para receuperar querystrings
function getQuerystring(key, default_) {
  if (default_ == null) default_ = "";
  key = key.replace(/[[]/, "[").replace(/[]]/, "]");
  var regex = new RegExp("[?&]" + key + "=([^&#]*)");
  var qs = regex.exec(window.location.href);
  if (qs == null) return default_;
  else return qs[1];
}
```

Para utiliza essa função, basta passar por parâmetro a chave da query string desejada:

```js
//Função que recuperar a querystring cod
function getValueCod() {
  txt.value = getQuerystring("cod");
}
```

O html completo para testar essa funcionalidade fica assim:

```html
<html>
<head>
    <title>JS</title>
</head>
<body>
    Link com querystring: <a href="index.html?cod=1980">index.html?cod=1980</a></p>
    Recuperar Querystring:
    <input type="text" id="txt" />
    <input type="submit" onclick="getValueCod()" />
    <script type="text/javascript">
        //Função que recuperar a querystring cod
        function getValueCod() {
            txt.value = getQuerystring('cod');
        }
        //Função comum para receuperar querystrings
        function getQuerystring(key, default_) {
            if (default_ == null) default_ = "";
            key = key.replace(/[[]/, "[").replace(/[]]/, "]");
            var regex = new RegExp("[?&]" + key + "=([^&#]*)");
            var qs = regex.exec(window.location.href);
            if (qs == null)
                return default_;
            else
                return qs[1];
        }
    </script>
</body>
</html>
```

