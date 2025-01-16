+++
title = "Partial class e MetaDataType"
+++

Se você esta acostumado a utilizar frameworks ORM (Object-relational mapping) como LINQ to SQL, LINQ to Entity e dentre outros. Uma grande vantagem, se não necessidade, é utilizar **Partial Class** (classes parciais).

Com partial class, é possível dividir a estrutura de classes em arquivos separados. E todas as partes são combinados quando o aplicativo é compilado.

Pode ser usada quando trabalhamos com grande projetos, para separar definições de classes, por simples organização ou para dar autonomia que cada desenvolvedor trabalhe em arquivos separados, mas relacionados a mesma classe. E também, trabalhando com arquivos gerados automaticamente, como de frameworks ORM, atribuindo novas definições as classes geradas pelo framework.

![](./partial-class.jpg "partial class")
_Partial Class_

Para definir uma classe como parcial é necessário incluir a palavra-chave **partial** em sua definição Além das classes possuírem exatamente o mesmo nome e namespace.

Ficando assim:

```cs
//Classe parcial no arquivo Pessoa1.cs
public partial class Pessoa
{
    public string nome { get; set; }
    public int idade { get; set; }

    public string GetNome()
    {
        return this.nome;
    }
}

//Classe parcial no arquivo Pessoa2.cs
public partial class Pessoa
{
    public string sobreNome { get; set; }

    public string GetNomeCompleto()
    {
        return this.nome + this.sobreNome;
    }
}
```

Estanciando a classe Pessoa, temos acesso as definições contidas em as classes:

```cs
Pessoa p = new Pessoa();
p.nome = "Fulano";
p.sobreNome = "Ciclano";
p.idade = 23;
string nome = p.GetNome();
string nomeCompleto = p.GetNomeCompleto();
```

Porém, se em uma partial class você define um atributo ou método que já exista, ao compilar retonará a seguinte mensagem:

##### _The type 'NOME_DA_CLASSE' already contains a definition for 'NOME_PROPRIEDADE_REPITIDA'._

Mas então você se pergunta, porque eu teria a idéia de criar um atributo que já existe?  Bem, se você trabalha com MVC, conhece as ótimas vantagens do [Data Annotation](http://msdn.microsoft.com/en-us/library/dd901590%28v=vs.95%29.aspx "Entenda mais sobre Data Annotation"), customizações de classes para especificar regras de validações. O que necessita que reescreva a propriedade, adicionando regras de validações do data annotation.

Porém, utilizando o ORM, as classes já são geradas automaticamente pelo framework. Complicando a definição de atributos de validação do Data Annotation para as propriedades da classe.

Sendo assim, a solução para esse problema é utilizar **MetadataType**. Habilitando a associação de uma classe diferente à outra do tipo partial class.

Entenda melhor no exemplo:

```cs
//Supondo que a classe Pessoa ja tenha sido definida

//Associa a classe PessoaMetaData à Pessoa
\[MetadataType(typeof(PessoaMetaData))\]
public partial class Pessoa
{
//Aqui você pode adicionar novas propriedades e metodos à classe
}

public class PessoaMetaData
{
//Data Annotation
\[Required(ErrorMessage = "O Nome deve ser preenchido")\]
public string nome { get; set; }
}
```

Obs: Imagem utilizada para demonstrar a partial class foi retirada [deste artigo](http://www.codeproject.com/KB/dotnet/PartialMethod.aspx).

