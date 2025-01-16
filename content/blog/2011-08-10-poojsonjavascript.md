+++
title = "POO JSON JavaScript"
+++

Quando começamos a trabalhar mais com javascript, aproveitando as vantagens de uma linguagem client-side e o código começar a crescer e ficar mais complexo, surge nessecidade de seguir algum padrão.

E nada melhor, do que implementar algo que você já esteja acostumado a utlizar, como o Paradígma Orientado a Objetos(**POO**).

Trantando como uma **CLASSE**, as informações manipuladas no código, torna-o mais legível, organizado e de fácil manutenção.

**Declarando uma classe em javascript:**

```js
//Declaração da classe Pessoa
function Pessoa() {
  //Definição dos atributos
  this.nome = "";
  this.idade = 0;
  this.formado = false;
}

//Instaciação de um objeto da classe Pessoa
var _fulano = new Pessoa();

//Acessando e alterando o valor de um atributo
_fulano.idade = 15;
```

**Supondo que possuímos o seguinte formulário:**

```html
<script type="text/javascript" src="jquery-1.4.1.js"></script>

Nome: <input id="txtNome" type="text" /> Idade: <input id="txtIdade" type="text" /> Formado:
<input id="chkFormado" type="checkbox" />Sim

<input id="btnEnviar" type="button" value="Enviar" />
```

**Com a classe declarada, fica mais legível e organizado manipular seus dados:**

```js
$(function () {
  //Evento click do botão Enviar
  $("#btnEnviar").click(function () {
    //Instaciação de um objeto da classe Pessoa
    var _pessoa = new Pessoa();

    //Acessando e alterando os valores dos atributos
    _pessoa.nome = $("#txtNome").val();
    _pessoa.idade = $("#txtIdade").val();
    _pessoa.formado = $("#chkFormado").attr("checked");

    alert(
      "Meu nome é " +
        _pessoa.nome +
        ", tenho " +
        _pessoa.idade +
        " anos e " +
        (_pessoa.formado ? "já" : "ainda não") +
        " sou formado",
    );
  });
});
```

Com o objeto definido e os valores dos seus atributos preenchidos, podemos aproveitar esta estrutura e converte-lo para o formato JSON, com a finalidade de passar esses dados para o code behind, através de um ajax, \_doPostBack ou outra necessidade. Para desfrutar dessa vantagem, de maneira fácil e simples, vamos utilizar o plugin [Jquery JSON](http://code.google.com/p/jquery-json/).

**Adicionando referência do plugin à pagina:**

```html
<script type="text/javascript" src="jquery.json-2.2.js"></script>
```

**Realizando a conversão do objeto para o formato JSON:**

```js
//Converte o objeto '\_pessoa' para o formato Json
var string_Json = $.toJSON(\_pessoa);
```

**Realizando a conversão de uma string no formato JSON para um objeto:**

```js
//Converte uma string no formato Json para objeto
var obj = $.parseJSON(string_Json);
```

Este artigo abordou conceitos básicos do POO, mas caso você tenha mas interesse sobre esse assunto, eu indico os artigos dessa [série em PT/BR](http://joaopedropereira.com/blog/2009/01/13/javascript-oop-1/) e dessa [série em EN](http://www.1stwebdesigner.com/design/object-oriented-basics-javascript/) .
Que são muito bons!

