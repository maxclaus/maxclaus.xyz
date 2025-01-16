+++
title = "Salvar como no ASP.NET"
+++

Muitas vezes quando estamos trabalhando em uma p gina, surge a necessidade de disponibiliar o download de um arquivo aos usu rios.

E para que o usu rio possa escolher onde salvar e como, devemos implementar uma janela de salvar como.

De maneira simples segue o código. No qual o arquivo é aberto e lido no lado do servidor e em seguida o navegador é-  forçado a-  abrir sua janela de "salvar como".

```cs
protected void Button1_Click(object sender, EventArgs e)
{
    //Pasta dentro do site
    string caminho = AppDomain.CurrentDomain.BaseDirectory + "pasta/";

    //Nome do arquivo
    string arquivo = "imgTemp.png";

    System.IO.FileStream fs = null;
    fs = System.IO.File.Open(caminho + arqivo, FileMode.Open);
    byte[] btFile = new byte[fs.Length];
    fs.Read(btFile, 0, Convert.ToInt32(fs.Length));
    fs.Close();
    Response.Buffer = true;
    Response.Expires = 0;
    Response.ContentType = "plain/text";
    Response.AddHeader("Content-Type", "plain/text");
    Response.AddHeader("Content-Disposition", "attachment;filename=" + arquivo);
    Response.Cache.SetCacheability(HttpCacheability.NoCache);
    Response.BinaryWrite(btFile);
    Response.End();
}
```

