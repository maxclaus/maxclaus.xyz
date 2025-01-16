+++
title = "Usando o Dictionary"
+++

Existem muitas formas de armazenar e recuperar dados no c#. Muitas vezes recorremos a arrays, listas, datatables e dentre outros. Porêm, algumas dessas alternativas, como o array, deixa a desejar na performance.

Neste artigo vou mostrar um pouco do Dictionary, uma classe contida no `System.Collections.Generic`, que possui uma alta performance mesmo quando utilizada com grandes quantidades de dados.

Definição - **Dictionary:** Representa um coleção de chaves e valores.

Inicializando o objeto com valores pré-definidos

```cs
public Dictionary Frutas = new Dictionary()
{
    {0, Laranja },
    {1, Maçã },
    {2, Goiaba }
};
```

Adicionando novos intens em tempo de execução

```cs
Frutas.Add(3, Pêssego );
Frutas.Add(4, Melancia );
Frutas.Add(5, Abacaxi );
```

Removendo um item em tempo de execução

```cs
Frutas.Remove(3);//Remove o Pêssego
```

Recuperando um valor de um item em tempo de execução

```cs
//Uma opção
string minhaFruta = Frutas\[1\];//Retorna a Melancia

//Outra opção - usando linq e lambda
string outraFruta = Frutas.First(f => f.Key == 2).Value;
```

Populando um dropdownlist através de um Dictionary

```cs
DropDownListFrutas.DataSource = Frutas;
DropDownListFrutas.DataValueField = Key ;
DropDownListFrutas.DataTextField = Value ;
DropDownListFrutas.DataBind();
```

Como vocês viram, o dictionary trabalha com uma chave e um valor. Sendo assim, ele não aceita valores repetidos para a chave.

