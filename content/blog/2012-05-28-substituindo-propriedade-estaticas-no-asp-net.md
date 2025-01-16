+++
title = "Substituindo propriedade estáticas no ASP.NET"
+++

Utilizar variáveis estática no ASP.NET é um sério problema, porque objetos estáticos possuem apenas uma estância em toda aplicação.

Ou seja, se eu estivesse em um e-commerce e começasse a adicionar itens ao meu carrinho, e um outro usuário logo a após eu, acesse a página de itens do carrinho dele. Estaria todos os meus itens visíveis para ele.

### Para escopo de toda a aplicação: Utilize SESSION

**ERRADO**

Onde estava um atributo estático:

`public static int nomeVariavel;`

**CERTO**

Deverá ser refatorado em uma propriedade que servirá apenas de acesso para o valor.

Ou seja, ao definir um valor para essa propriedade, será armazenado em uma Session.

E ao recuperar um valor dessa propriedade, ela irá buscar na mesma Session.

````cs
public int nomeVariavel
{
    get { return (Session["NOME_SESSAO"] == null) ? 0 : int.Parse(Session["NOME_SESSAO"].ToString()); }
    set { Session["NOME_SESSAO"] = value; }
}```

No exemplo a cima, utilizei if ternário. Se a session estiver nula, então retorna um valor padrão. Se não, converte a session para tipo correto e retorna o valor em seguida.

No caso de uma variável do tipo Lista devemo sempre inicializar a lista caso ela ainda seja nula, poderíamos fazer assim:

```cs
public List nomeVariavel
{
    get {
        if(Session["NOME_SESSAO"] == null)
            Session["NOME_SESSAO"]  = new List();
        return (Session["NOME_SESSAO"] as List);
    }
    set { Session["NOME_SESSAO"] = value; }
}
````

### Para escopo de apenas uma página: Utilize ViewState

Caso o escopo dessa variável seja apenas de uma tela, ou seja esse valor deixará de existir quando for deixado a página, poderia ser utilizado uma ViewState no lugar da Session.

````cs
public List nomeVariavel
{
    get {

        if(ViewState["NOME_SESSAO"] == null)
            ViewState["NOME_SESSAO"] = new List();
        return (ViewState["NOME_SESSAO"] as List);
    }
    set { ViewState["NOME_SESSAO"] = value; }
}```
````

