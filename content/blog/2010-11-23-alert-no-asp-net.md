+++
title = "Alert no ASP.NET"
+++

Forma simples e pr tica de utilizar o alert no asp.net.

Utilizando essa classe estática, basta chamar diretamente o método em qualquer página que você estiver desenvolvendo.

Métodos da classe:

1.  **br()** -> Pula uma linha dentro do alert.
2.  **Exibir()** - _2 Parâmetros_ -> Exibi uma caixa de mensagem, o alert.
3.  **Exibir()** - _3 Par**â**metros_ -> Exibi uma caixa de mensagem, o alert. Posteriormete redireciona para a p gina desejada.
4.  **ExibirNovaJanela()** -> Exibi uma caixa de mensagem, o alert. Posteriormete abre a p gina desejada em uma nova janela.

A classe `cMensagem.cs`

```cs
public static string br()
{
    return "n";
}

/// <summary>
/// Exibi uma janela de alerta
/// </summary>
/// <param name="mensagem">Mensagem a ser exibida</param>
/// <param name="pagina">Página atual</param>
public static void Exibir(string mensagem, Page pagina)
{
    ScriptManager.RegisterStartupScript(pagina, pagina.GetType(), Guid.NewGuid().ToString(), "alert('" + mensagem + "');", true);
}

/// <summary>
/// Exibi uma janela de alerta e posteriormente redireciona para outra p gina
/// </summary>
/// <param name="mensagem">Mensagem a ser exibid</param>
/// <param name="url">;Página para qual ser redirecionada</param>
/// <param name="pagina">Página atual</param>
public static void Exibir(string mensagem, string url, Page pagina)
{
    //Esconde conteudo da página pagina.
    pagina.Form.Attributes.CssStyle.Add("display", "none");
    ScriptManager.RegisterStartupScript(pagina, pagina.GetType(), Guid.NewGuid().ToString(), "alert('" + mensagem + "'); location='" + url + "';", true);
}

public static void ExibirNovaJanela(string mensagem, string url, Page pagina)
{
    ScriptManager.RegisterStartupScript(pagina, pagina.GetType(), Guid.NewGuid().ToString(), "alert('" + mensagem + "'); window.open('" + url + "','\_blank');", true);
}
```

Exibindo o alert em um click do botão por exemplo:

```cs
protected void btnEnviar_Click(object sender, EventArgs e)
{
    cMensagem.Exibir("Ola Mundo!", this.Page);
}
```

Exibindo um alert e em seguida redirecionando para a p gina desejada:

```cs
protected void btnEnviar_Click(object sender, EventArgs e)
{
    cMensagem.Exibir("Você ser redirecionado para a página de cadastro!", "PaginaCadastro.aspx", this.Page);
}
```

