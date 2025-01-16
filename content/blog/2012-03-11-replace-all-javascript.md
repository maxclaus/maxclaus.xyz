+++
title = "Replace All JavaScript"
+++

Dica rápida:

Para você que precisa substituir vários caracteres repetidos em um texto javascript. E já percebeu que o  simples "replace" não funciona. No caso do javascript, o replace substitui apenas o 1º caracter encontrado.

Basta utilizar split + join ao invés do replace: 

```js
.split(CaracterAntigo).join(CaracterNovo);
```

Exemplo:

```js
var texto = "1. 2. 3. 4. 5";
var textoFormatado = texto.split(".").join("");
//Ficaria assim: 1 2 3 4 5
```

