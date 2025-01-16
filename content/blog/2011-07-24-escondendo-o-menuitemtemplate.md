+++
title = "Escondendo o MenuItemTemplate"
+++

Trabalhando com sharepoint, uma situação que eu sempre tive dificuldade em resolver e não sabia como, era a dúvida de como esconder itens do MenuTemplate do Sharepoint, associados ao um SPMenuField dentro da Gridview de acordo com cada registro.

**Por exemplo:**
Em uma lista de clientes, que irei carregar na gridview, podem possuir duas situações.
O cliente pode estar como cadastrado ou excluído.
E meu sharepoint menu template pode possuir duas opções, ver os detalhes do cliente ou excluí-lo.
Sendo assim, não faz sentido manter a opção de excluir visível quando o cliente já estiver sido excluído.
Não é?

A solução para isso é muito simples.
Utilizando o evento "RowCreated" da Gridview, será percorrido linha-à-linha da Gridview. Recuperando o 1º Controle da 1ª Célula de cada linha, que neste exemplo é onde o SPMenufield está localizado.
Com o SPMenuField recuperado, basta passar o Id do MenuItemTemplate que não deverá estar visível na linha atual.

Primeiro monte a página aspx, com os controles Sharepoint:MenuTemplate, Sharepoint:MenuItemTemplate, SharePoint:SPMenuField e SPGridView ou a GridView padrão mesmo.

```html
<!-- Sharepoint MenuTemplate -->
<SharePoint:MenuTemplate ID="mtEventMenu" runat="server">
  <%-- 1º Item Menu - Detalhe --%>
  <SharePoint:MenuItemTemplate
    ID="mitDetails"
    runat="server"
    Text="Detalhes"
    ImageUrl="/_layouts/images/details.gif"
    ClientOnClickScript="performPostBack('DETAILS,%Id%');"
    Title="Detalhes"
  >
  </SharePoint:MenuItemTemplate>

  <%-- 2º Item Menu - Excluir --%>
  <SharePoint:MenuItemTemplate
    ID="mitDelete"
    runat="server"
    Text="Excluir"
    ImageUrl="/_layouts/images/delete.gif"
    ClientOnClickScript="performPostBack('DELETE,%Id%');"
    Title="Excluir"
  >
  </SharePoint:MenuItemTemplate>
</SharePoint:MenuTemplate>

<!-- Poderia ter utilizado uma SPGridView ao invés da GridView-->
<asp:GridView ID="GridView1" runat="server" DataKeyNames="Id" onrowcreated="GridView1_RowCreated">
  <Columns>
    <SharePoint:SPMenuField
      HeaderText="Nome"
      TextFields="Nome"
      MenuTemplateId="mtEventMenu"
      TokenNameAndValueFields="Id=Id"
      SortExpression="Id"
    />
    <%-- adicione codigo para outras colunas --%>
  </Columns>
</asp:GridView>
```

E no codebehind, veja como é facil esconder o MenuItemTemplate de cada linha da gridview, utilizando o evento RowCreated:

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace WebApplication1 {
  public partial class
  _Default : System.Web.UI.Page {
    // Como demonstraçao, foi criado um enum de situacoes do cliente
    enum Situacao { Cadastrado = 0, Excluido = 1 }
    ;

    protected void Page_Load(object sender, EventArgs e) {
      // Uma lista para preencher a gridview
      var Clientes = new List() { new Cliente() { Id = 1, Nome = "Pedro", Situacao = (int)Situacao.Cadastrado },
                                  new Cliente() { Id = 2, Nome = "João", Situacao = (int)Situacao.Excluido } };

      // Carregando a gridview
      GridView1.DataSource = Clientes;
      GridView1.DataBind();
    }

    protected void GridView1_RowCreated(object sender, GridViewRowEventArgs e) {
      // Só se a linah for do tipo DataRow
      if (!(e.Row.RowType == DataControlRowType.DataRow))
        return;

      // Cria um objeto cliente, referente aos dados da linha atual
      var item = e.Row.DataItem as Cliente;
      if (item == null)
        return;

      // Recupera o controle SPMenuField desta linha
      var thisSPMenu = e.Row.Cells\[0\].Controls\[0\] as Microsoft.SharePoint.WebControls.Menu;
      if (thisSPMenu == null)
        return;

      if (item.Situacao == (int)Situacao.Excluido) {
        /\*
\* Esconde o 2º Item Menu - Excluir
\* Passando o Id do MenuItemTemplate desejado
\* / thisSPMenu.HiddenMenuItemIds = "mitDelete";

        /\*
\*Para esconder mais itens, separe os Ids por ','
\*Exemplo :
\*thisSPMenu.HiddenMenuItemIds = "mitDetails,mitDelete";
        \* /
      }
    }
  }

  class Cliente {
    public int Id { get; set; }
    public string Nome { get; set; }
    public int Situacao { get; set; }
  }
}
```

