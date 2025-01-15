+++
title = "Debug Knockout js"
+++

<p>Tenho utilizado knockout js em alguns projetos e o desacoplamento que ele possibilita aplicando o padrão MVVM é fascinante. &nbsp;</p>
<p>Um forma simples que tenho utilizado para facilitar os binds durante o desenvolvimento é exibir serializado em JSON quais os dados estão sendo passados para viewmodel.</p>

{% gist maxcnunes/5229570 %}

<p>Um exemplo dos dados serializados em JSON seria assim:</p>
<pre>
<code>
{
  "categories": [
    "id": 1,
    "name": "Transport",
    "parent": 0,
    "isNullo": false,
    "isDirty": false,
      "_moduleId_": "models/model.category"
  ],
  "pageDisplayName": "List Category",
  "pageDescription": "All your categories",
  "_moduleId_ _": "viewmodels/category/show"
}
</code>
</pre>
