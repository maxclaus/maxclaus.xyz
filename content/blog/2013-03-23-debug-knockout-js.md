+++
title = "Debug Knockout js"
+++

Tenho utilizado knockout js em alguns projetos e o desacoplamento que ele possibilita aplicando o padrão MVVM é fascinante.  

Um forma simples que tenho utilizado para facilitar os binds durante o desenvolvimento é exibir serializado em JSON quais os dados estão sendo passados para viewmodel.

Um exemplo dos dados serializados em JSON seria assim:

```json
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
```

