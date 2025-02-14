+++
title = "Comparação de arrays desordenados - Javascript"
+++

Trabalhando com javasacript, em muitos momentos surge a necessidade de manter em hiddens ids selecionados em arrays, concatenados por algum caracter separador. E em algumas dessas situações preciso comparar esses arrays com outros arrays, que podem estar em ordens diferentes.

Sendo assim uma dica rápida de como comparar arrays de objetos, dos quais estão em ordens diferentes:

```js
//Compara string de arrays [strArray1 = lista 1, strArray2 = lista 2, separator = separador]
function disorderlyStrArraysAreEqual(strArray1, strArray2, separator) {
  //Verifica se os valores estao vazios
  if (strArray1 == "" || strArray1 == "undefined" || strArray2 == "" || strArray2 == "undefined") {
    return strArray1 == strArray2;
  }

  //Separa os valores pelo caracter separador
  //e remove o ultimo separador se for o ultimo caracter da string
  var array1 = rtrim(strArray1, separator).split(separator);
  var array2 = rtrim(strArray2, separator).split(separator);

  //Compara arrays pelo tamanho
  if (array1.length != array2.length) {
    return false;
  }

  for (var index = 0; index < array1.length; index++) {
    //Verifica na lista 2 se o item atual da lista 1 não existe
    if (strArray2.indexOf(array1[index]) == -1) return false;
    //Verifica na lista 1 se o item atual da lista 2 não existe
    if (strArray1.indexOf(array2[index]) == -1) return false;
  }
  return true;
}
```

Para garantir que seja removido ultimo caracter de separação vamos utilizar a seguinte função, que remove caracter passado por parametro, se ele for o ultimo da string:

```js
function rtrim(str, chars) {
  chars = chars || "s";
  return str.replace(new RegExp("[" + chars + "]+$", "g"), "");
}
```

Utilizando a função de comparação:

```js
var isEqual = disorderlyStrArrayIsEqual(newValues, oldValues, ";");
```

