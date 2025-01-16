+++
title = "500 Internal Error when accessing layouts/images virtual directory"
+++

Outro dia passei por esse problema, fiquei cerca de 1 dia procurando uma solução de como resolver. Como estava utilizando Autenticação Baseada em Declarações (Claims Authentication), imaginei que fosse alguma falta de permissão para os usuários e acabei ficando preso a essa possibilidade. O que me levou a ficar todas essas horas definindo permissões de todas formas possíveis, na pasta LAYOUTS, no IIS, nas configurações do site collection. Mas nada resolvia e o bendito 500 Internal continuava a aparecer.

A minha grande falha foi tentar resolver um problema em cima de um problema genérico. Porque quando adicionei o `<customErrors mode="Off" />` ao webconfig na pasta LAYOUTS o problema foi resolvido em 10 segundos.

Na minha situação, eu havia deixado uma tag incorreta dentro do do webconfig do site que estava apresentando o problema.

Caminho da pasta LAYOUTS: `C:Program FilesCommon FilesMicrosoft SharedWeb Server Extensions14TEMPLATELAYOUTS`

Fica a dica então: **Nunca tente resolver um problema em cima de uma descrição genérica, se é possível obter informações mais específicas.**

