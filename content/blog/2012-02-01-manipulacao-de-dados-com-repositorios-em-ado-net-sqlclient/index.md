+++
title = "Manipulação de Dados com Repositórios em ADO.NET SqlClient"
+++

![](./Projeto-ADONET-186x300.png "Projeto-ADONET")

Hoje venho apresentar uma forma simples e produtiva para persistência e manipulação de dados.

Para isso vou utilizar a seguinte arquitetura:

**Camada de Apresentação** : Telas Windows Form

**Camada de Entidades**: Classes referente aos objetos de negócio do projeto

**Camada de Repositório**: Classes relacionadas aos objetos de negócios, com a responsabilidade de persistência e manipulação de dados.

Para o acesso aos dados, implementarei os repositórios utilizando o ADO.NET SqlClient. O que por um lado vai gerar um pouco mais de serviço, precisando implementar todos os comandos SQL para manipulação dos dados. Por outro lado, será um ponto positivo. Pois vamos ter a autonomia nessas manipulações, por não estar dependendo de Frameworks ORMs e tendo todas as possibilidades de comandos que o SQL pode nos proporcionar.

O código do projeto foi compartilhado no Github: [https://github.com/maxclaus/WindowsForm-Entidades-RepositorioAdoNet](https://github.com/maxclaus/WindowsForm-Entidades-RepositorioAdoNet)

### Projeto Entidades

Aqui implementamos dentro do projeto de Entidades, nossa classe referente ao objeto Aluno. Tente sempre manter, para as classes de entidade, a real características e comportamentos referente ao  objeto que a classe representa. Não deixe métodos CRUD aqui, porque no mundo real um aluno não possui tais comportamentos.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace MDI_Escola.Entidades
{
    /// <summary>
    /// Entidade Aluno
    /// </summary>
    public class Aluno
    {
        public int Id { get; set; }
        public string Nome { get; set; }
        public string Email { get; set; }
        public string Telefone { get; set; }
        public string Endereco { get; set; }
        public int Genero { get; set; }
        public bool Ativo { get; set; }
    }
}
```

### Projeto Repositório

A classe a seguir será herdada por todos os repositórios concretos. Nela manteremos métodos e atributos comum aos outros repositórios.  Neste caso, foi implementado o método criar conexão, que recupera a string de conexão que está no arquivo app.config, que está localizado no projeto principal da Solution (Camada Apresentação).

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Data.SqlClient;
using System.Configuration;

namespace MDI_Escola.Repositorios
{
    public abstract class RepositorioBase
    {
        /// <summary>
        /// Cria um objeto de conexao com o banco
        /// </summary>
        /// <returns></returns>
        protected SqlConnection CriarConexao()
        {
            return new SqlConnection
                       {
                           //Recupera a string de conexao com o banco
                           ConnectionString = ConfigurationManager.
                               ConnectionStrings["MDI_Escola"].
                               ConnectionString
                       };
        }
    }
}
}
}
```

Interface que define os comportamentos comuns ao todos os repositórios. Dessa forma garantimos que todos os repositórios deverão seguir esta implementação. O mais legal nesse ponto, é que foi utilizado generics. Deixando esta interface bem genérica e possibilitando que qualquer classe consiga implementá-la.

```cs
using System.Collections.Generic;

namespace MDI_Escola.Repositorios
{

    /// <summary>
    /// Interface para o Repositorio de dados : Define contratos que as classes repositórios deverão implementar
    /// </summary>
    /// <typeparam name="T">Classe entidade referente ao repositório implementado</typeparam>
    public interface IRepositorio<T> where T : class
    {
        /// <summary>
        /// Recuperar uma entidade do banco
        /// </summary>
        /// <param name="id">Código do registro no banco</param>
        /// <returns>Entidade carregada com os valores do banco</returns>
        T Obter(int id);

        /// <summary>
        /// Salvar um novo registro no banco
        /// </summary>
        /// <param name="entidade">Entidade com propriedades preenchidas, para ser inserida no banco</param>
        /// <returns>Sucesso da operação</returns>
        bool Inserir(T entidade);

        /// <summary>
        /// Remover registro do banco
        /// </summary>
        /// <param name="id">Código do registro no banco</param>
        /// <returns>Sucesso da operação</returns>
        bool Excluir(int id);

        /// <summary>
        /// Atualizar um registro no banco
        /// </summary>
        /// <param name="entidade">Entidade com propriedades preenchidas, para ser atualizada no banco</param>
        /// <returns>Sucesso da operação</returns>
        bool Atualizar(T entidade);

        /// <summary>
        /// Recuperar todos os registros da tabela no banco
        /// </summary>
        /// <returns>Lista de entidades referente a todos os registros encontrados na tabela no banco</returns>
        List<T> Todos();
    }
}
```

Repositório Aluno implementa a interface IRepositorio e herda as definições da classe RepositorioBase.  Aqui sim realizamos o acesso aos dados, através do SqlClient do ADO.NET.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using MDI_Escola.Entidades;
using System.Data.SqlClient;
using System.Data;

namespace MDI_Escola.Repositorios
{
    public class AlunoRepositorio : RepositorioBase, IRepositorio<Aluno>
    {
        /// <summary>
        /// Obter aluno : Recupera registro do aluno no banco e retorna o objeto aluno
        /// </summary>
        /// <param name="id">Codigo do aluno</param>
        /// <returns>Entidade aluno</returns>
        public Aluno Obter(int id)
        {
            //Define comando SQL
            var comando = new SqlCommand
                              {
                                  CommandText = "SELECT * FROM alunos WHERE id = @id"
                              };

            //Define obj aluno como null
            Aluno aluno = null;

            try
            {
                //Cria objeto de conexao com o banco
                using (comando.Connection = CriarConexao())
                {
                    //Abre conexao com o banco
                    comando.Connection.Open();

                    //Passa paremetro para o comando SQL
                    comando.Parameters.Add(new SqlParameter("id", id));

                    //Executa comando SQL, para leitura dos registros
                    var reader = comando.ExecuteReader();

                    //Le o resultado, linha a linha
                    while (reader.Read())
                    {
                        //Carrega objeto aluno com os valores retornado do banco
                        aluno = new Aluno
                        {
                            Id = (int)reader["id"],
                            Nome = (string)reader["nome"],
                            Email = (string)reader["email"],
                            Telefone = (string)reader["telefone"],
                            Endereco = (string)reader["endereco"],
                            Ativo = (bool)reader["ativo"],
                            Genero = (int)reader["genero"]
                        };
                    }
                    //Libera os recursos de leitura
                    reader.Dispose();
                }
            }
            catch (Exception ex)
            {
                throw new ArgumentException("Erro ao retornar aluno do banco. Msg: " + ex.Message);
            }
            finally
            {
                comando.Dispose();
            }
            return aluno;
        }

        /// <summary>
        /// Inserir aluno: Insere um novo registro no banco na tabela de alunos
        /// </summary>
        /// <param name="entidade">Objeto aluno</param>
        /// <returns>Sucesso da operação</returns>
        public bool Inserir(Aluno entidade)
        {
            //Define comando SQL
            var comando = new SqlCommand
                              {
                                  CommandText = "INSERT INTO alunos " +
                                                "VALUES(@nome,@email,@telefone,@endereco,@genero,@ativo)"
                              };

            try
            {
                //Cria objeto de conexao com o banco
                using (comando.Connection = CriarConexao())
                {
                    //Abre a conexao com o banco
                    comando.Connection.Open();

                    //Definindo valores para os parametros do comando SQL
                    comando.Parameters.Add(new SqlParameter("nome", entidade.Nome));
                    comando.Parameters.Add(new SqlParameter("email", entidade.Email));
                    comando.Parameters.Add(new SqlParameter("telefone", entidade.Telefone));
                    comando.Parameters.Add(new SqlParameter("endereco", entidade.Endereco));
                    comando.Parameters.Add(new SqlParameter("genero", entidade.Genero));
                    comando.Parameters.Add(new SqlParameter("ativo", entidade.Ativo));

                    //Executa o comando
                    comando.ExecuteNonQuery();

                    //Retorna operaçao realizada com sucesso
                    return true;
                }
            }
            catch (Exception ex)
            {
                //Dispara uma exceçao caso algum problema ocorra
                throw new ArgumentException("Erro ao inserir. Msg:" + ex.Message);
            }
            finally
            {
                //Libera os recursos
                comando.Dispose();
            }
        }

        /// <summary>
        /// Excluir aluno: Remove registro do aluno do banco
        /// </summary>
        /// <param name="id">Codigo do aluno</param>
        /// <returns>Sucesso da operação</returns>
        public bool Excluir(int id)
        {
            //Define comando SQL
            var comando = new SqlCommand
                              {
                                  CommandText = "DELETE FROM alunos WHERE id = @id"
                              };
            try
            {
                //Cria objeto de conexao com o banco
                using (comando.Connection = CriarConexao())
                {
                    //Abre conexao com o banco
                    comando.Connection.Open();

                    //Adiciona os parametros do comando SQL
                    comando.Parameters.Add(new SqlParameter("id", id));

                    //Executa comando
                    comando.ExecuteNonQuery();

                    //Retorna operacao realizada com sucesso
                    return true;
                }
            }
            catch (Exception ex)
            {
                //Dispara uma exceçao caso algum problema ocorra
                throw new ArgumentException("Erro ao excluir aluno. Msg: " + ex.Message);
            }
            finally
            {
                //Libera os recursos
                comando.Dispose();
            }
        }

        /// <summary>
        /// Atualizar aluno: Atualiza informações do aluno no banco
        /// </summary>
        /// <param name="entidade"></param>
        /// <returns>Sucesso da operação</returns>
        public bool Atualizar(Aluno entidade)
        {
            //Define o comando SQL
            var comando = new SqlCommand
                              {
                                  CommandText = "UPDATE alunos SET " +
                                                "nome = @nome, " +
                                                "email = @email, " +
                                                "telefone = @telefone, " +
                                                "endereco = @endereco, " +
                                                "genero = @genero, " +
                                                "ativo = @ativo, " +
                                                "where id = @id "
                              };

            try
            {
                //Cria objeto de conexao com o banco
                using (comando.Connection = CriarConexao())
                {
                    //Abre a conexao com o banco
                    comando.Connection.Open();

                    //Adiciona os parametros do comando SQL
                    comando.Parameters.Add(new SqlParameter("id", entidade.Id));
                    comando.Parameters.Add(new SqlParameter("nome", entidade.Nome));
                    comando.Parameters.Add(new SqlParameter("email", entidade.Email));
                    comando.Parameters.Add(new SqlParameter("telefone", entidade.Telefone));
                    comando.Parameters.Add(new SqlParameter("endereco", entidade.Endereco));
                    comando.Parameters.Add(new SqlParameter("genero", entidade.Genero));
                    comando.Parameters.Add(new SqlParameter("ativo", entidade.Ativo));

                    //Executa comaando
                    comando.ExecuteNonQuery();

                    //Retorna operacao realizada com sucesso
                    return true;
                }
            }
            catch(Exception ex)
            {
                //Dispara uma exceçao caso uma problema ocorra
                throw new ArgumentException("Erro ao executar comando no banco. Msg:" + ex.Message);
            }
            finally
            {
                comando.Dispose();
            }
        }

        /// <summary>
        /// Todos alunos: Recupera a lista total alunos do banco
        /// </summary>
        /// <returns>Lista com todos os alunos registrados</returns>
        public List<Aluno> Todos()
        {
            //Define comando SQL
            var comando = new SqlCommand
            {
                CommandText = "SELECT * FROM alunos"
            };

            //Cria um a lista vazia de alunos
            var listaAlunos = new List<Aluno>();

            //Cria um objeto de conexao com o banco
            using (comando.Connection = CriarConexao())
            {
                //Abre conexao com o banco
                comando.Connection.Open();

                //Executa comando SQL, para leitura dos registros
                var reader = comando.ExecuteReader();

                //Carrega objeto aluno com os valores retornado do banco
                while (reader.Read())
                {
                    //Carrega objeto aluno com os valores retornado do banco
                    var alunodb = new Aluno
                                      {
                                          Id = (int)reader["id"],
                                          Nome = (string)reader["nome"],
                                          Email = (string)reader["email"],
                                          Telefone = (string)reader["telefone"],
                                          Endereco = (string)reader["endereco"],
                                          Genero = (int)reader["genero"],
                                          Ativo = (bool)reader["ativo"]
                                      };

                    //Adiciona objeto aluno a lista
                    listaAlunos.Add(alunodb);
                }

                //Libera recursos
                reader.Dispose();
                comando.Dispose();
            }
            return listaAlunos;
        }

        /// <summary>
        /// Verifica se o nome preenchido está disponível para cadastro
        /// </summary>
        /// <param name="nome">Nome do aluno</param>
        /// <param name="id">Codigo do aluno</param>
        /// <returns>Disponibilidade para cadastro</returns>
        public bool VerificarNomeDisponivel(string nome, int id)
        {
            //Define o comando SQL
            var comando = new SqlCommand
                            {
                                CommandText = "PR_VerificarNomeDisponivel",
                                CommandType = CommandType.StoredProcedure
                            };

            try
            {
                //Cria obj de conexao com o banco
                using (comando.Connection = CriarConexao())
                {
                    //Abre conexao com o banco
                    comando.Connection.Open();

                    //Passa o parametro para a PROC
                    comando.Parameters.Add(new SqlParameter("NOME", nome));
                    comando.Parameters.Add(new SqlParameter("ID", id));

                    //Recupera a primeira coluna da primeira linha do resultado
                    //Nesse caso o id do aluno
                    var resultado = comando.ExecuteScalar();

                    //Retorna verdadeiro se nao existir um aluno com esse nome ja cadsrado
                    if (resultado == null)
                        return true;
                    else
                        return false;
                }
            }
            catch (Exception ex)
            {
                //Dispara uma msg de erro, caso ocorra algum problema na execuçao do codigo
                throw new ArgumentException("Erro ao verificar disponibilidade de nome. Msg:" + ex.Message);
            }
            finally
            {
                //Libera os recursos utilizados
                comando.Dispose();
            }
        }
    }
}
```

Note que no código anterior utilizamos 3 tipos de _**"Execute Command"**_, para executar os comandos SQL.

n **ExecuteScalar:** Executa o comando SQL e retorna a primeira coluna da primeira linha do conjunto de resultados retornado na consulta. As restantes linhas e colunas são ignoradas. Normalmente utilizado para COUNT, SUM e outros desse tipo.

- **ExecuteReader:** Executa o comando SQL retorna uma variável que fornece uma maneira de ler um fluxo uni-direcional de uma base de dados SQL Server. Utilizado para comandos de SELECT.
- **ExecuteNonQuery:** Executa o comando SQL e retorna o número de linhas afetadas. Normalmente utilizado para comandos INSERT, DELETE e UPDATE.

### Projeto MDI_Escola (Apresentação)

A tela para gerenciamento do Aluno ficará assim:

![](./Tela-ADONET.png "Tela-ADONET")

Eventos da tela Cadastrar Aluno (CodeBehind da Tela):

```cs
using System;
using System.Windows.Forms;
using MDI_Escola.Entidades;
using MDI_Escola.Repositorios;

namespace WindowsForms_MDI_Escola
{
    public partial class FrmCadAluno : Form
    {
        #region Propriedades
        //Repositorio de dados
        private AlunoRepositorio _repositorioAluno;
        #endregion

        #region Metodos
        public FrmCadAluno()
        {
            //Inicializa repositorio de dados
            _repositorioAluno = new AlunoRepositorio();
            InitializeComponent();

            LimparCampos();
        }

        protected void LimparCampos()
        {
            //Limpa todos os campos textos
            lblCodigo.Text =
            txtNome.Text =
            txtEmail.Text =
            txtEndereco.Text =
            txtTelefone.Text = string.Empty;

            //Desmarca todos os campos de marcação e desativa botoes
            rbFem.Checked =
            rbMasc.Checked =
            chkAtivo.Checked =
            btnAlterar.Enabled =
            btnCancelar.Enabled =
            btnExcluir.Enabled = false;

            btnSalvar.Enabled = true;

            //Remove o auto gerador de colunas da datagridview
            dataGridViewAlunos.AutoGenerateColumns = false;

            //Recupera todos os alunos do banco
            dataGridViewAlunos.DataSource = _repositorioAluno.Todos();
        }

        protected Aluno CarregarObjAluno(int id)
        {
            //Carrega obj ALuno com os valores preenchidos nos componnetes da tela
            var aluno = new Aluno
            {
                Id = id,
                Nome = txtNome.Text,
                Email = txtEmail.Text,
                Telefone = txtTelefone.Text,
                Endereco = txtEndereco.Text,
                Ativo = chkAtivo.Checked,
                Genero = (rbMasc.Checked ? 0 : 1)
            };

            return aluno;
        }

        public void AlterarSituacaoBotao(int id, bool situacao)
        {
            //Ativa botoes Salvar e Alterar

            //id = 0 -> Está inserindo um novo registro
            if (id == 0)
                btnSalvar.Enabled = situacao;
            else
                btnAlterar.Enabled = situacao;
        }
        #endregion

        #region Eventos
        private void btnSalvar_Click(object sender, System.EventArgs e)
        {
            //Carrega valores preenchidos nos componentes da tela para o obj Aluno
            var aluno = CarregarObjAluno(0);

            //Insere aluno :
            //-------------- Retorno = true -> Exibe msg 1; Retorno = false -> Exibe msg 2;
            MessageBox.Show(_repositorioAluno.Inserir(aluno)
                                ? "Incluido com sucesso!"
                                : "Ocorreu um erro ao salvar! Tente novamente.");
            LimparCampos();
        }

        private void dataGridViewAlunos_CellDoubleClick(object sender, DataGridViewCellEventArgs e)
        {
            //Recupera o id referente ao registro selecionado
            var id = int.Parse(dataGridViewAlunos.CurrentRow.Cells["Id"].Value.ToString());

            try
            {
                //Recupera o aluno pelo id
                var aluno = _repositorioAluno.Obter(id);

                if (aluno == null)
                {
                    MessageBox.Show("Aluno não encontrado");
                    return;
                }

                //Carrega componentes com os valores retornados do BD
                lblCodigo.Text = aluno.Id.ToString();
                txtNome.Text = aluno.Nome;
                txtEmail.Text = aluno.Email;
                txtEndereco.Text = aluno.Endereco;
                txtTelefone.Text = aluno.Telefone;
                chkAtivo.Checked = aluno.Ativo;
                rbMasc.Checked = (aluno.Genero == 0);
                rbFem.Checked = (aluno.Genero == 1);

                btnSalvar.Enabled = false;
                btnAlterar.Enabled = btnCancelar.Enabled = btnExcluir.Enabled = true;
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);
            }
        }

        private void btnAlterar_Click(object sender, EventArgs e)
        {
            //Recupera o id do aluno selecionado
            var id = int.Parse(lblCodigo.Text);

            //Carrega o aluno
            var aluno = CarregarObjAluno(id);

            //Atualiza aluno :
            //-------------- Retorno = true -> Exibe msg 1; Retorno = false -> Exibe msg 2;
            MessageBox.Show(_repositorioAluno.Atualizar(aluno) ? "Iserido com sucesso!" : "Erro ao atulizar");

            //Limpa campos da tela
            LimparCampos();
        }

        private void btnCancelar_Click(object sender, EventArgs e)
        {
            //Limpa campos da tela
            LimparCampos();
        }

        private void btnExcluir_Click(object sender, EventArgs e)
        {
            //Recupera codigo do aluno selecionado
            var id = int.Parse(lblCodigo.Text);

            //Exibe msg de confirmaçao e verifica se a opçao selecionada foi = 'sim'
            if (MessageBox.Show("Deseja excluir?",
                                "Excluir",
                                MessageBoxButtons.YesNo,
                                MessageBoxIcon.Question) == DialogResult.Yes)
            {
                //Caso seleciona sim, exclui aluno
                _repositorioAluno.Excluir(id);
            }

            //Limpa campos da tela
            LimparCampos();
        }

        private void txtNome_Leave(object sender, EventArgs e)
        {
            int id;
            //Tenta converter para inteiro, se não der certo define o valor = 0
            int.TryParse(lblCodigo.Text, out id);

            //Verifica se o nome esta disponivel para cadastro
            var disponivel = _repositorioAluno.VerificarNomeDisponivel(txtNome.Text, id);

            //Exibe uma msg caso o nome nao esteja disponivel
            lblMsg.Text = disponivel ? string.Empty : "Este nome não está disponível. Tente outro, por favor.";

            //Libera ou Bloqueia botao, dependendo da disponibilidade
            AlterarSituacaoBotao(id, disponivel);
        }
        #endregion
    }
}
```

Por último adicione um arquivo "App.config" ao projeto da camada de Apresentação e configure nele a conexão com o seu banco:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <connectionStrings>

    <add name="MDI_Escola"
       connectionString="Data Source=NT-001;Initial Catalog=MDI_Escola;User ID=sa;Password=123"
       providerName="System.Data.SqlClient"/>

  </connectionStrings>

</configuration>
```

