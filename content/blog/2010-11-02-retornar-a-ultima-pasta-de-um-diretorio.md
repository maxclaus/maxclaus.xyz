+++
title = "Retornar a última pasta de um diretório"
+++

Uma das maneiras de retornar a última pasta de um diretório.

Por exemplo, voce tem uma url _**http://blog2.maxcnunes.com/topicos/csharp/default.aspx**_ e gostaria de retornar apenas a última pasta.

O jeito que encontrei foi este:

```cs
public string RetornaUltimaPasta(string url)
{
    return System.IO.Path.GetFileName(Path.GetDirectoryName(url);
}
```

