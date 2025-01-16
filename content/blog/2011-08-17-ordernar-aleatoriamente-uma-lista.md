+++
title = "Ordernar aleatoriamente uma lista"
+++

**LINQ** _(Language Integrated Query)_, além de oferecer um modelo consistente para trabalhar com dados em vários tipos de fontes de dados e formatos, outra importante característica, é facilidade em criar consultas, desde simples _selects_ à outas mirabolantes expressões que uma query possa conter.

Neste exemplo demonstrarei como é fácil, **utilizando LINQ**, aplicar uma consulta aletória em uma `List<int>`.

O mesmo poderia ser feito em uma lista de objetos do tipo Pessoa ou qualquer outro do seu domínio.

Isto porque o **LINQ** oferece suporte ao IEnumerable(Of T) ou interfaces derivadas, como o genérico IQueryable(Of T) -  _chamados de tipos consultáveis_.

```cs
//Lista de exemplo
var lista = new List<int>()
{
    0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
};
```

Passando a expressão OrderBy(i => Guid.NewGuid()) a lista retornará aletória.

```cs
//Retorna a lista aletória
var lista\_aleatoria = lista.OrderBy(i => Guid.NewGuid());
```

Agora, por que isto acontece?
Basta saber três coisas para entender:

1.  **GUID**:  Representa um Globally Unique Identifier, ou seja, um Identificador Único Universal.
2.  **LINQ - Variável de Consulta** :  Quando você cria uma query com LINQ, o formato dessa consulta será armazenado nessa variável. Mas a consulta ainda não será realizada.
3.  **LINQ - Consulta Adiada** : A real execução da consulta é adiada até que você faça a iteração pela variável de consulta em uma instrução foreach. Esse conceito é conhecido como execução adiada.

Sendo assim, cada linha na consulta, a expressão de ordernação será diferente.
_**Obs:** No LINQ é possível forçar a execução imediata de uma consulta. Para entender mais sobre LINQ eu indico [Introdução às consultas do LINQ (C#)](http://msdn.microsoft.com/pt-br/library/bb397906.aspx) ._

