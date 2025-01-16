+++
title = "Padrão de nomes"
+++

Em projetos que existem um time de desenvolvimento e um desenvolvedor depende de outros para construir um sistema, uma padronização no código deve ser tomada como regra para o bem comum do projeto.

Não estou dizendo que os desenvolvedores devem ficar moldados a regras e não podem experimentar novas possibilidades. Mas que se necessário e possível, seja feito com o consentimento de todos.

## Caso do Problema

Vamos usar um exemplo, que com certeza ocorre em muitas empresas. Um desenvolvedor, que cumpria todas as tarefas muito bem, dentro do prazo e funcionalidades exigidas. O qual, nunca passou pela sua cabeça sair da empresa, em um momento inesperado precisa sair da mesma. Até ai tudo bem, mas logo após deste fato, ocorre algum problema em umas das tarefas desenvolvidas por ele e você como desenvolvedor foi encarregado de corrigir esse pequeno bug.

Ao abrir o código para manutenção, você nota que aquilo que estava funcionando muito bem, na verdade estava uma completa bagunça.

- Métodos com letras todas em maiúscula e outros em minúscula.
- Algumas variáveis em inglês e outras em português.
- Algumas variáveis e métodos abreviados.
- Nenhum comentário e summary sobre as funcionalidades. (É desconsiderável se estiver bem estruturado, refatorado e com nomes corretos)
- Métodos repetidos que realizavam exatamente a mesma funcionalidade.

Imaginem como uma simples correção pode se tornar tão complicado, que é possível pensar que seja mais fácil refazer esta tarefa, do que entender toda essa bagunça para conseguir corrigir o bug.

## Solução

Desde o .NET Framework 1.1 a Microsoft disponibiliza no MSDN um Guia de Nomenclaturas, que indica uma série de padrões para nomes.

[http://msdn.microsoft.com/en-us/library/xzf533w0(v=vs.71).aspx#feedback](<http://msdn.microsoft.com/en-us/library/xzf533w0(v=vs.71).aspx#feedback>)

### Estilos de Capitalização

Utilize o seguinte 3 estilos de convenções para as capitalizações.

|             |                                                                                                                                                                                                                      |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Pascal case | A primeira letra do identificador e a primeira letra de cada palavra concatenada subsequente forem capitalizados. Você pode usar caso Pascal para identificadores de três ou mais caracteres. Por exemplo: BackColor |
| Camel case  | A primeira letra de um identificador é minúscula ea primeira letra de cada palavra concatenada subsequente é maiúscula. Por exemplo: backColor                                                                       |
| Uppercase   | Todas as letras no identificador são capitalizados. Use esta convenção apenas para identificadores que consistem em duas ou menos letras. Por exemplo: System.IO System.Web.UI                                       |

A seguinte tabela resume as regras de capitalização e disponibiliza exemplos de diferentes tipos de capitalização.

|                      |          |                                        |
| -------------------- | -------- | -------------------------------------- |
| **Identificador**    | **Case** | **Exemplo**                            |
|  Classes             |  Pascal  |  OrdemDeServico                        |
|  Enum                |  Pascal  |  Situacao                              |
|  Eventos             |  Pascal  |  AlteracaoDoValor                      |
|  Read-only, Estático |  Pascal  |  ValorVermelho                         |
|  Interfaces          |  Pascal  |  IRepositorio (Sempre com prefixo 'I') |
|  Métodos             |  Pascal  |  BuscarPeloNome                        |
|  Namespace           |  Pascal  |  Projeto.Dados.Repositorio             |
|  Parâmetro           |  Camel   |  dataNascimento                        |
|  Propriedade         |  Pascal  |  DataNascimento                        |
|  Atributo            |  Camel   |  dataCadastro                          |

### Abreviações

Para evitar confusão, siga estas regras relativa a utilização de abreviaturas:

- Não use abreviações ou contrações de partes de nomes identificadores. Por exemplo, use ObterAltura ao invés de ObterAlt.
- Não utilize siglas que não são geralmente aceitados no campo da computação.
- Quando apropriado, use o bom conhecimento de siglas para substituir frases e nomes compridos. Pro exemplo, use UI ao invés de User Interface.
- Quando utilizar siglas, use Pascal case ou Camel case para siglas com mais de 2 caracteres. E quando for apenas 2 caracteres, mantena tudo em maiúsculo.

### Evitando a Confusão com Nomes e Tipos

Use nomes que descrevem o significado de um tipo ao invés de nomes que descrevem o tipo. No caso raro que um parâmetro não tem significado semântico para além do seu tipo, usar um nome genérico. Por exemplo:

| Errado                                                     | Certo                                             |
| ---------------------------------------------------------- | ------------------------------------------------- |
|  public Pessoa BuscarPessoa(string stringNome);            |  public Pessoa BuscarPessoa(string nome);         |
|  public int CalcularImc(int intPeso, double doubleAltura); |  public int CalcularImc(int peso, double altura); |

### Namespace

Uma regra comum para a nomenclatura de namespaces é utilizar o nome da empresa seguido pelo nome da tecnologia ou camada de um projeto e opcionalmente pela funcionalidade e design.

| NomeEmpresa.NomeTecnologia\[.Funcionalidade\]\[.Design\] |
| -------------------------------------------------------- |

Por exemplo:

| Tam.Dados              |
| ---------------------- |
| Tam.Dados.Repositorios |

Não utilize o mesmo nome para um namespace e uma classe. Um namespace chamado Tam.Dados e uma classe chamada Dados.

### Nome de Classes

- Utilize um substantivo como nome de classes.
- Utilize Pascal case.
- Utilize abreviações com moderação.
- Não utilize prefixos como c para classes. Por exemplo, utilize Pessoa ao invés de cPessoa.
- Não utilize '\_' para separar nomes.
- Quando apropriado, utilize uma palavra composta para nomear uma classe derivada. A segunda parte do nome da classe derivada deve ter o nome da classe base. Por exemplo, ArgumentException.

| public class Pessoa |
| ------------------- |

### Nome de Interfaces

- Nome de interfaces com substantivos ou adjetivos que descrevam um comportamento. Por exemplo, a interface com o nome IRepositorio descrevendo um substantivo. E a interface IValidador com uma adjetivo.
- Utilize Pascal case.
- Utilize abreviações com moderação.
- Não utilize '\_' para separar nomes.
- Utilize o prefixo 'I' em maiúsculo para indicar que é uma interface.
- Utilize similaridade de nomes quando você definiu uma classe/interface par onde a classe é um padrão da implementação da interface. O nomes devem serem diferenciados somente pela letra I como prefixo no nome da interface.

| public interface IValidador |
| --------------------------- |

### Nome de Enums

- Use Pascal case para Enums e seus valores
- Utilize abreviações com moderação.
- Não utilize Enum como sufixo ou prefixo.

### Nome de Campos Estáticos

- Utilize substantivos ou abreviações de substantivos para campos estáticos.
- Utilize Pascal case.
- Não utilize a notação Hungara para nomes de campos estáticos.
- É recomendado que utilize propriedades estáticas ao invés de campos estáticos.

Referências:

- [http://msdn.microsoft.com/en-us/library/ab6a8y1b(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/ab6a8y1b(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/4df752aw(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/4df752aw(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/fzcth91k(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/fzcth91k(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/h0eyck3s(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/h0eyck3s(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/04xykw57(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/04xykw57(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/426s83c3(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/426s83c3(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/9cws5bzd(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/9cws5bzd(v=vs.71).aspx>)
- [http://msdn.microsoft.com/en-us/library/czefa0ke(v=vs.71).aspx](<http://msdn.microsoft.com/en-us/library/czefa0ke(v=vs.71).aspx>)
