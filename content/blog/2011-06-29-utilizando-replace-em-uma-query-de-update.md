+++
title = "Utilizando Replace em uma query de UPDATE"
+++

Depois de passar um tempo sem escrever nenhum artigo, após a transferência do servidor do blog, estou retomando as atividades por aqui.

E este artigo envolve exatamente esta transferência de servidor.

Por algum motivo, todos os caractéres com acentos especiais no banco, se transformaram em símbolos. Para resolver esse problema, utilizei o REPLACE, substituido todos estes carácters inválidos pelos acentos originais.

O REPLACE funciona da seguinte forma:

```sql
--Definicao
replace(`texto_original`, 'valor_atual', 'novo_valor');

--Exemplo pratico
replace(`papagaio`, 'a', '-');
--O retoro -> p-p-g-io
```

Como demonstrado o REPLACE retorna a cópia do texto passado com a  substituição dos caractéres definidos.

Para salvar esse retorno, faremos assim:

```sql
--Definicao
UPDATE `nome_tabela`
SET `nome_coluna` = replace(`nome_coluna`, 'valor_atual', 'novo_valor')
WHERE `nome_coluna` LIKE '%valor_atual%';

--Exemplo pratico
UPDATE `usuarios`
SET `nome` = replace(`nome`, 'Maria', 'Mariana')
WHERE `nome` LIKE '%Maria%';
--Atualiza todos os registros da tabela usuarios, na coluna nome,
--substituindo Maria por Mariana
```

