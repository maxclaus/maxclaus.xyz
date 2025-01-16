+++
title = "Uma simples aplicação com DataSet Tipado"
+++

Um tutorial simples de como trabalhar com DataSet Tipado no Windows Form.

Facil e rapido!

Neste artigo foi feito:

1.  Criei uma tabela cliente no banco
2.  Criei um formul rio Principal WindowsForm com funcionalidades de INSERT, DELETE e UPDATE
3.  Criei outro formul rio Pesquisa WindowsForm com funcionalidade de SELECT e com possibilidade de selecionar o cliente desejado, para edit -lo no formul rio Principal.
4.  Criei um DataSet com os métodos de SELECT, INSERT, DELETE e UPDATE.
5.  Criei uma classe est tica para guardar temporariamente o cliente selecionado.

Segue o video tutorial: http://vimeo.com/14344312

Arquivos:

[Sql do Banco](http://www.4shared.com/document/OH6B484i/sqlCliente.html "Sql do banco"), [Projeto no Visual Studio](http://www.4shared.com/file/XnKZb7oR/MinhaAplicacao.html "Projeto no Visual Studio"), [Video do desenvolvimento](http://www.4shared.com/video/NwKgnKd-/VideoTutorial.html)

Como o projeto est em Visual Studio 10, aqui vão os código do formul rio principal e pesquisa.

frmPrincipal:

```cs
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
// INCLUI O NOSSO DATASET
using MinhaAplicacao.dsDadosTableAdapters;

namespace MinhaAplicacao {
  public partial class frmPrincipal : Form {
    // Objeto TableAdapter da tabela cliente no dataset
    ClienteTableAdapter clienteTA = new ClienteTableAdapter();

    // Limpar todos os campos
    public void Limpar() {
      txtCodigo.Text = string.Empty;
      txtCodigo.Enabled = false;
      txtNome.Text = string.Empty;
      txtEmail.Text = string.Empty;
      cSelecinado.codigoSelecionado = 0;
      MessageBox.Show(&amp; amp; amp; amp; amp; amp; amp; amp; amp; quot; Operação realizada com sucesso & amp; amp;
                      amp; amp; amp; amp; amp; amp; amp; quot;);
    }

    public frmPrincipal() {
      InitializeComponent();
    }

    private void btnGravar\_Click(object sender, EventArgs e) {
      try {
        if (txtCodigo.Text == string.Empty) {
          // INSERIR NOVO CLIENTE
          clienteTA.Insert(txtNome.Text, txtEmail.Text);
        } else {
          int codigo = Convert.ToInt32(txtCodigo.Text);
          // ATUALIZAR CLIENTE SELECIONADO
          clienteTA.UpdateCliente(txtNome.Text, txtEmail.Text, codigo);
        }
        Limpar();
      } catch {
      }
    }

    private void btnPesquisar\_Click(object sender, EventArgs e) {
      frmPesquisa fPesquisa = new frmPesquisa();
      // ABRIR JANELA DE PESQUISA
      fPesquisa.ShowDialog();

      // APÃ“S FECHAR A JANELA PESQUISA, SE TIVER ALGUM CLIENTE SELECIONADO
      if (cSelecinado.codigoSelecionado != 0) {
        // BUSCA OS VALORES DO CLIENTE SELECIONADO E PREENCHE OS CAMPOS
        txtCodigo.Text = clienteTA.BuscarPorCodigo(cSelecinado.codigoSelecionado)\[0\].idCliente.ToString();
        txtNome.Text = clienteTA.BuscarPorCodigo(cSelecinado.codigoSelecionado)\[0\].nome;
        txtEmail.Text = clienteTA.BuscarPorCodigo(cSelecinado.codigoSelecionado)\[0\].email;
      }
    }

    private void btnDeletar\_Click(object sender, EventArgs e) {
      try {
        if (txtCodigo.Text != string.Empty) {
          // DELETA O CLIENTE SELECIONADO
          clienteTA.DeleteCliente(Convert.ToInt32(txtCodigo.Text));
        }
        Limpar();
      } catch {
      }
    }
  }
}
```

frmPesquisa:

```cs
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
// INCLUI O NOSSO DATASET
using MinhaAplicacao.dsDadosTableAdapters;

namespace MinhaAplicacao {
  public partial class frmPesquisa : Form {
    // Objeto TableAdapter da tabela cliente no dataset
    ClienteTableAdapter clienteTA = new ClienteTableAdapter();

    public frmPesquisa() {
      InitializeComponent();
    }

    private void frmPesquisa\_Load(object sender, EventArgs e) {
      // TODO: This line of code loads data into the 'dsDados.Cliente' table. You can move, or remove it, as needed.
      this.clienteTableAdapter.FillTodos(this.dsDados.Cliente);
    }

    private void btnPesquisar\_Click(object sender, EventArgs e) {
      if (txtCodigo.Text != string.Empty) {
        int codigo = Convert.ToInt32(txtCodigo.Text);
        // BUSCA CLIENTE PELO CODIGO
        dataGridView1.DataSource = clienteTA.BuscarPorCodigo(codigo);
      } else if (txtNome.Text != string.Empty) {
        // BUSCA O CLIENTE PELO NOME
        dataGridView1.DataSource = clienteTA.BuscarPorNome(txtNome.Text);
      } else {
        // BUSCA O CLIENTE PELO EMAIL
        dataGridView1.DataSource = clienteTA.BuscarPorEmail(txtEmail.Text);
      }
    }

    private void btnSelecionar\_Click(object sender, EventArgs e) {
      // PEGA O CODIGO DO CLIENTE NA LINHA SELECIONADA
      int codigoSelecionado = Convert.ToInt32(dataGridView1.CurrentRow.Cells\[0\].Value);

      // GUARDA ESSE CODIGO EM UMA VARIAVEL ESTATICA, PARA PODERMOS ACESSÃ-LA NO OUTRO FORMULÃRIO
      cSelecinado.codigoSelecionado = codigoSelecionado;

      // FECHA A JENALA DE PESQUISA
      this.Close();
    }
  }
}
```

cSelecionado:

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace MinhaAplicacao {
  static class cSelecinado {
    static public int codigoSelecionado = 0;
  }
}
```

