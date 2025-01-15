+++
title = "Bind elemento a partir de um item selecionado - Knockout"
+++

Eu precisava carregar uma modal a partir de um item selecionado. Ou seja, cada vez que fosse clicado em um item em uma lista, a modal deveria ser atualizada com informações referente ao item selecionado.

Com knockout a solução é bem simples:

1.  Criar uma property "selected" do tipo observable
2.  Atribuir o item atual ao "selected" através do click sobre o item desejado
3.  Usar algum tipo de bind como "with" para disponibilizar o dados aos elementos html

No final vai ficar algo mais ou menos assim:

```html
<script type="text/javascript">
  var viewModel = {
    students: ko.observableArray([
      { id: 1, name: "Jake" },
      { id: 2, name: "Kate" },
      { id: 3, name: "Mari" },
    ]),
    selected: ko.observable(),
  };

  ko.applyBindings(viewModel);
</script>

<ul data-bind="foreach: students">
  <li>
    <a href="#" data-bind="click: $root.selected, text: name"></a>
  </li>
</ul>

<div data-bind="with: selected">Id: <span data-bind="text: id"></span> Name: <span data-bind="text: name"></span></div>
```

