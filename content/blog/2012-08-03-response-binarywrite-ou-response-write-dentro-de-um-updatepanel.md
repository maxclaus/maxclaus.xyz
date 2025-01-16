+++
title = "Response.BinaryWrite ou Response.Write dentro de um UpdatePanel"
+++

Para resolver problemas relacionados Response.BinaryWrite ou Response.Write dentro de um UpdatePanel você deve definir o evento do controle que executa o Response como uma trigger do tipo PostBack, possibilitando um postaback completo da tela. Ao contrário do tipo de trigger padrão AsyncPostBack já definido para os controles dentro de um UpdatePanel, que realizam o post apenas do conteúdo dentro do UpdatePanel de forma assíncrona.

Por padrão devemos definir uma trigger PostBack da seguinte forma:

```html
<asp:updatepanel id="UpdatePanel1" runat="server">
  <contenttemplate> </contenttemplate>
  <triggers>
    <asp:asyncpostbacktrigger controlid="lnkButton" eventname="Click" />
  </triggers>
</asp:updatepanel>
```

Mas quando não temos acesso direto a um controle, por estar dentro de uma grid ou ou tipo de controle. Podemos registrar a trigger para
o controle desejado da seguinte forma:

```cs
protected void MyGrid_RowDataBound(object sender, GridViewRowEventArgs e)
{
   LinkButton lnkButton = e.Row.FindControl("lnkButton") as LinkButton;
   ScriptManager.GetCurrent(this).RegisterPostBackControl(lnkButton);
}
```

