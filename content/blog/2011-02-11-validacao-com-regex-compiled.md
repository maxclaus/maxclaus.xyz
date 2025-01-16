+++
title = "Validação com REGEX Compiled"
+++

Uma classe simples de expressões regulares com vários tipos de validações.

Antes dos código, um pouco da definição da "Regex"

**Expressão Regular:** _Uma composição de símbolos, caracteres com funções especiais, que, agrupados entre si e com caracteres literais, formam uma sequência, uma expressão. Essa expressão é interpretada como uma regra, que indicaría sucesso se uma entrada de dados qualquer casar com essa regra, ou seja, obedecer exatamente a todas as suas condições._

Segundo testes publicados neste artigo [http://dotnetperls.com/regex-performance](http://dotnetperls.com/regex-performance) o Regex Compiled toma 10x mais tempo na partida, mas os rendimentos de tempo de execução são 30% melhor.

Primeiro crie a classe de validação:

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Text.RegularExpressions;

/// summary
/// Classe de validação
/// summary
public static class ValidateField {
  /// summary
  /// Enum com os tipos de Validacoes
  /// summary
  public static enum TipoValidacao { CPF, CNPJ, Data, Tempo, Email, CEP, Telefone, Decimal, URL }
  ;

  /// summary
  /// Expressão regular com as regras do CPF
  /// summary
  protected static Regex _cpf = new Regex(@"^d{3}.?d{3}.?d{3}-?d{2}$", RegexOptions.Compiled);

  /// <summary>
  /// Expressão regular com as regras de cnpj
  /// </summary>
  protected static Regex _cnpj = new Regex(@"(^(d{2}.d{3}.d{3}/d{4}-d{2})|(d{14})$)");

  /// <summary>
  /// Expressão regular com as regras de data Ex: 23/09/2011
  /// </summary>
  protected static Regex _data =
      new Regex(@"^((0\[1-9\]|\[12\]d)/(0\[1-9\]|1\[0-2\])|30/(0\[13-9\]|1\[0-2\])|31/(0\[13578\]|1\[02\]))/d{4}$",
                RegexOptions.Compiled);

  /// <summary>
  /// Expressão regular com as regras de tempo Ex: 12:35
  /// </summary>
  protected static Regex _tempo = new Regex(@"((\[0-1\]\[0-9\])|(\[2\]\[0-3\])):(\[0-5\]\[0-9\])");

  /// <summary>
  /// Expressão regular com as regras de e-mail Ex: nickname@email.com
  /// </summary>
  protected static Regex _email = new Regex(
      @"^\[A-Za-z0-9\]((\[_.-\]?\[a-zA-Z0-9\]+)\*)@(\[A-Za-z0-9\]+)((\[.-\]?\[a-zA-Z0-9\]+)\*).(\[A-Za-z\]{2,})$");

  /// <summary>
  /// Expressão regular com as regras de cep Ex: 87094-132
  /// </summary>
  protected static Regex _cep = new Regex(@"^\[0-9\]{5}-\[0-9\]{3}$");

  /// <summary>
  /// Expressão regular com as regras de telefone Ex: (44)3344-4332
  /// </summary>
  protected static Regex _telefone = new Regex(@"^(d{2})d{4}-d{4}$");

  /// <summary>
  /// Expressão regular com as regras de telefone Ex: 405,00 ou 405.00
  /// </summary>
  protected static Regex _decimal = new Regex(@"^(?=d+)d{0,4}(\[.,\]d{0,2})?$");

  /// <summary>
  /// Expressão regular com as regras de url Ex: http://www.maxcnunes.com
  /// </summary>
  protected static Regex _url = new Regex(
      @"^(ht|f)tp(s?)://\[0-9a-zA-Z\](\[-.w\]\*\[0-9a-zA-Z\])\*(:(0-9)\*)\*(/?)(\[a-zA-Z0-9-.?,'/+&amp;amp;%$#_\]\*)?$");

  /// <summary>
  /// Método de validação
  /// </summary>
  /// <param name="value">Valor string a ser validado</param>
  /// <param name="type">Tipo enum da validação</param>
  /// <returns>False se estiver inv lido</returns>
  public static bool ValidateField(string value, TipoValidacao type) {
    bool validate = false;

    switch (type) {
      case TipoValidacao.CPF:
        validate = _cpf.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.CNPJ:
        validate = _cnpj.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.Data:
        validate = _data.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.Tempo:
        validate = _tempo.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.Email:
        validate = _email.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.CEP:
        validate = _cep.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.Telefone:
        validate = _telefone.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.Decimal:
        validate = _decimal.IsMatch(value) ? true : false;
        break;
      case TipoValidacao.URL:
        validate = _url.IsMatch(value) ? true : false;
        break;
      default:
        validate = false;
        break;
    }
    return validate;
  }
}
```

Utilizar a classe onde desejar, exemplo ao disparar o evento click do botão em uma p gina ASP.NET:

```cs
protected void ButtonValidate_Click(object sender, EventArgs e) {
  if (!Validate(TextBoxCPF.Text, ValidateField.TipoValidacao.CPF)) {
    Response.Write("CPF est em um formato inv lido!");
    return;
  }
  if (!Validate(TextBoxDataNascimento.Text, ValidateField.TipoValidacao.Data)) {
    Response.Write("Data de nascimento est em um formato inv lido!");
    return;
  }
}
```

