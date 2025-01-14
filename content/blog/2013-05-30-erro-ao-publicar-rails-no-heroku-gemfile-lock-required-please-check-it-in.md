+++
title = "Erro ao publicar Rails no Heroku - Gemfile.lock required. Please check it in."
+++

Passei um bom tempo quebrando a cabeça por causa desse problema a baixo que ocorria toda vez que eu tentava publicar o Rails no Heroku:

{% gist maxcnunes/5675847 %}

No final das contas a solução foi simples, mas como pode ser que ocorra com alguém mais, então esta foi a minha solução:

- Remova o Gemfile.lock do `.gitignore`

- Execute esse comando para remover a pasta de bundle local, executar o bundle localmente, adicionar o Gemfile.lock e commitar

```bash
rm -rf .bundle && bundle install && git add Gemfile.lock && git commit -m "Added Gemfile.lock"
```

- Execute esse comando para publicar no heroku (considerando que você esteja trabalhando com github simultaneamente).

```bash
git push heroku branch_do_github:master
```
