+++
title = "Listando mensagens do Twitter - MVC"
+++

O Twitter tornou-se uma forte ferramenta de comunicação na web. Cada vez mais empresas e usuários aderem ao Twitter. Pela sua enorme facilidade de disseminar informações.

Sendo assim, se você trabalha com o desenvolvimento de websites, muito provavelmente vai passar por um projeto no qual o usuário vai solicitar que as suas últimas mensagens do twitter apareçam no site.

Dito isso, vamos ao que interessa!

A parte mais iportante desse código é representada pela classe Twitter.cs. Tratando cada mensagem como um objeto, esta classe possui as propriedades como **Autor da mensagem, Link, Foto do Autor, Conteúdo da Mensagem**. O que essa classe faz é uma requisição para a API do Twitter passando um filtro desejado, formado através de querys strings.

Para entender mais sobre a API e seus filtros acesse [http://dev.twitter.com/](http://dev.twitter.com/).

A classe Twitter:

```cs
public class Twitter {
  public string AuthorName { get; set; }
  public string AuthorUri { get; set; }
  public string Content { get; set; }
  public string Updated { get; set; }
  public string Link { get; set; }

  public static List GetTwitter(string search) {
    // Faz a pesquisa no twitter de acordo com o filtro passado
    var request = WebRequest.Create("http://search.twitter.com/search.atom?" +
                                    search) as HttpWebRequest;
    var twitterViewData = new List();

    if (request != null) {
      using (var response = request.GetResponse() as HttpWebResponse) {
        // Lê o xml do resultado da pesquisa
        using (var reader = new StreamReader(response.GetResponseStream())) {
          var document = XDocument.Parse(reader.ReadToEnd());
          XNamespace xmlns = "http://www.w3.org/2005/Atom";

          // Cria uma lista do resultado
            twitterViewData = (from entry in document.Descendants(xmlns + "entry")
            select new Twitter
            {
                Content = entry.Element(xmlns + "content").Value,
                Updated = Format_DateTime(entry.Element(xmlns + "updated").Value),
                AuthorName = entry.Element(xmlns + "author").Element(xmlns + "name").Value,
                AuthorUri = entry.Element(xmlns + "author").Element(xmlns + "uri").Value,
                Link = (from o in entry.Descendants(xmlns + "link")
                where o.Attribute("rel").Value == "image"
                select new { Val = o.Attribute("href").Value }).First().Val
            }).ToList();
        }
      }
    }

    return twitterViewData;
  }

  public static string Format_DateTime(string value) {
    // Cria um CultureInfo para o Português.
    CultureInfo ci = new CultureInfo("pt-BR");

    value = value.Replace("T", " ").Replace("Z", string.Empty);

    DateTime \_Date;
    if (!DateTime.TryParse(value, out \_Date))
      throw new ArgumentException("Erro ao converter.");

    // Formata a data para o formato de tempo brasileiro.
    return \_Date.ToString(ci);
  }
```

O controller, na Action Result Index, solicita a lista de mensagens do Twitter pelo metodo GetTwitter da classe Twitter e passa esses valores para a propriedade dinâmica Twitter na ViewBag:

```cs
public class TwitterController : Controller {
  //
  // GET: /Twitter/

  public ActionResult Index() {
    // Passa como parametro o valor da pesquisa
    // Neste exemplo:
    // from -> Filtra as mensagens por usuário
    // rpp -> Retorna apenas os últimos 15 resultados
    ViewBag.Twitter = Models.Twitter.GetTwitter("from=twitterapi&rpp=15");
    return View();
  }
}
```

A View recebe a lista através da ViewBag e carrega seus valores de acordo com a formatação seguinte:

```cs
@{ ViewBag.Title = "Lista de Mensagens do Twitter"; }

## Index

@{
    var userTwitter = ((List)ViewBag.Twitter).FirstOrDefault();
    [![Twitter Logo](@userTwitter.Link)@userTwitter.AuthorName](@userTwitter.AuthorUri)
}

@foreach (var item in ViewBag.Twitter)
{
    *@Html.Raw(item.Content)
    @item.Updated
}

[![Twitter](@Url.Content\()Twitter]()
```

Estilo css, pra dar uma melhorada no visual:

```css
/*
-------Twitter*/
#Twitter {
  border: 1px solid #8aa2aa;
  padding: 1px;
}
.titleTwitter {
  background: #8aa2aa;
  padding: 5px 10px;
}
.titleTwitter a {
  font-size: 8pt;
  color: #fff;
}
.titleTwitter img {
  margin-right: 5px;
  vertical-align: middle;
}
.userTwitter img {
  width: 30px;
}
#Twitter ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
#Twitter ul li {
  font-size: 8pt;
  border-bottom: 1px solid #ccc;
  padding: 10px 5px;
  text-align: justify;
}
#Twitter ul li a {
  font-weight: bold;
}
#Twitter ul li span {
  display: block;
  text-align: left;
  font-size: 7pt;
}
#Twitter ul li:hover {
  background: #cceef9;
}
```

Fonte:[Using ASP.NET MVC 2 to Query Twitter's Public API - FIFA 2010](http://www.dotnetcurry.com/ShowArticle.aspx?ID=536)

