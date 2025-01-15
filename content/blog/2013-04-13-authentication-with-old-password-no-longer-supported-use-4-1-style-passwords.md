+++
title = "Authentication with old password no longer supported, use 4.1 style passwords"
+++

Esta semana eu estava trabalhando normalmente usando NHibernate conectado em um banco de dados MySQL local. Quando tentei mudar minha conexão para um servidor remoto na minha hospedagem, começou a exibir a seguinte mensagem de erro bem genérica ao tentar abrir a conexão:

**Arithmetic operation resulted**

Após várias tentativa frustantes de corrigir o problema, tentei atualizar a versão do MySQL connector de 6.6.4 para 6.6.5. Então começou a exibir uma mensagem de erro diferente:

**Authentication with old password no longer supported, use 4.1 style passwords**

Com essa mensagem de erro mais específica consegui encontrar a solução para o meu problema.

1.  Atualize a versão do seu MySQL connector para 6.6.5.
2.  Conecte no seu banco de dados remoto através de alguma ferramente como o MySQL Workbench.
3.  Execute a query abaixo alterando o nome e senha do seu usuário:

```sql
SET SESSION old_passwords=0;
SET PASSWORD FOR my_user=PASSWORD('my_password');
```

Tente conectar novamente através do c# e deverá funcionar.

