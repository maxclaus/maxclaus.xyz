+++
title = "Desenvolvendo uma pequena aplicação web com Python e Flask"
+++

Neste artigo será explicado como configurar o seu ambiente de desenvolvimento, desenvolver uma aplicação web em python modularizada e no final publicá-la em um servidor free.

Tópicos abordados:

- [Virtualenv – Ambientes Virtuais Independentes](#virtualenv)
- [Flask – Micro Framework de desenvolvimento web para Python](#flask)
- [Python Any Where – Publicando de maneira simples e rápida](#pythonanywhere)

## O que é o Virtualenv?

<a name="virtualenv"></a>

![](./Virtualenv.jpg)

Virtualenv é um software que permite a criação de ambientes virtuais com total independência dos outros ambientes criados no Virtualenv. Isso permite que cada ambiente tenha autonomia para instalar plug-ins e bibliotecas de forma que a configuração de um ambiente não impacte nos restantes. Um exemplo prático seria a possibilidade de ter várias versões do Python, diferente para cada projeto, instalado na mesma máquina.

###  

### Instalando o Virtualenv

Para facilitar nossa vida vamos utilizar um módulo Python chamado **easy_install** que gerencia pacotes Python e possui ferramentas de instalação que realiza automaticamente download, build e instalação dos pacotes disponíveis (no quais são muitos).

sudo easy_install virtualenv

> Caso você não tenha o easy_install configurado em sua máquina ainda, execute o seguinte comando no Terminal:
>
> sudo apt-get install python-setuptools python-dev build-essential

A mensagem de sucesso deverá ser algo parecida com a seguinte:

```
Searching for virtualenv
Best match: virtualenv 1.8.4
Adding virtualenv 1.8.4 to easy-install.pth file
Installing virtualenv script to /usr/local/bin
Installing virtualenv-2.7 script to /usr/local/bin

Using /usr/local/lib/python2.7/dist-packages
Processing dependencies for virtualenv
Finished processing dependencies for virtualenv
```

### Criando o Primeiro Ambiente Virtual

Antes de criar o primeiro ambiente virtual vamos separar um diretório para centralizar todos os documentos, códigos-fontes, configurações e outros assuntos relacionados a Desenvolvimento em um diretório chamado Development na Home do seu usuário no Linux:

```
cd ~/
mkdir Development
```

Para manter os ambientes virtuais bem organizados, vamos criar um diretório chamado virtualenvs dentro de Development:

```
cd ~/Development
mkdir virtualenvs
```

Agora sim vamos criar nosso primeiro ambiente virtual dentro do diretório virtualenvs. Como mais pra frente nesse post irei explicar como desenvolver uma pequena aplicação, esse ambiente virtual será focado para esse projeto. Então meu ambiente virtual se chamará i-sweated-yesterday:

```
cd virtualenvs
virtualenv i-sweated-yesterday
```

A mensagem de sucesso deverá ser algo parecida com a seguinte:

```
New python executable in i-sweated-yesterday/bin/python
Installing setuptools............done.
Installing pip...............done.
```

> Caso você desejasse criar um ambiente que não utilize qualquer biblioteca pré instalada no sistema operacional, deveria ser executado da forma a baixo:
>
> virtualenv i-sweated-yesterday --no-site-packages

###  

### Utilizando o Ambiente Virtual Criado

#### Ativando

Para utilizar o ambiente virtual você deve ativá-lo antes de realizar qualquer configuração, como instalar ou utilizar alguma biblioteca instalada no mesmo. O comando para ativar o ambiente virtual criado anteriormente é o seguinte:

```
source i-sweated-yesterday/bin/activate
```

Sempre que o ambiente virtual estiver ativado irá exibir o nome do ambiente virtual no lado esquerdo das linhas de comando no Terminal (Prompt). No meu caso fica da seguinte forma:

```
(i-sweated-yesterday)max@Max-Xub-VM:~$
```

A partir de agora qualquer configuração realizada estará associada apenas ao ambiente virtual ativo no momento.

#### Desativando

Quando não existir mais a necessidade de utilizar o ambiente virtual no momento, você pode voltar para o ambiente do sistema operacional desativando o ambiente virtual atual:

```
(i-sweated-yesterday)max@Max-Xub-VM:~$ deactivate
```

####  

#### Visualizar os Pacotes Instalados no Ambiente Virtual Atual

Com o ambiente virtual ativado vamos instalar o pacote chamado yolk, que possui a funcionalidade de listar os pacotes já instalados no ambiente atual:

```
sudo easy_install yolk
```

Após instalado execute o seguinte comando para visualizar os pacotes instalados:

```
yolk -l
```

## O que é o Flask?

![](./Flask.jpg)
<a name="flask"></a>

Flask é um micro framework de desenvolvimento web para Python baseado em duas bibliotecas externas: O Jinja2 e o Werkzeug. Por mais que seja considerado “micro”, Flask suporta extensões de diversas funcionalidades para a sua aplicação, como integração de ORM, validação de formulários, open autenticações e outras diversas extensões.

###  

### Configurações

#### Instalando o Flask

Para utilizar o Flask precisamos instalá-lo primeiro (não esqueça de ativar o ambiente virtual antes):

pip install Flask

Vamos aproveitar que estamos instalando o Flask e já vamos instalar alguns outros pacotes para estender as funcionalidades do Flask e deixar o ambiente configurado para nosso aplicativo.

#### Instalando SQLAlchemy

SQLAlchemy é uma poderosa ferramenta ORM que possibilita aos desenvolvedores trabalhar com dados armazenados no banco de dados de maneira flexível e simplificada.

```
pip install SQLAlchemy
```

####  

#### Instalando o Flask-SQLAlchemy

Flask-SQLAlchemy adiciona funcionalidades ao Flask que facilitam algumas tarefas comuns ao utilizar o SQLAlchemy.

```
pip install flask-sqlalchemy
```

###  

#### Instalando o Flask-WTF

Flask-WTF integra de maneira simples o WTForms ao Flask, fornecendo uma maneira fácil de lidar com a apresentação de dados do usuário.

```
pip install Flask-WTF
```

#### Instalando o SQLite3

SQLite é um pacote que disponibiliza um Sistema Gerenciador de Banco de Dados Relacional e permite ser executado através de linha de comando, possibilitando executar qualquer query SQL básica de maneira simples. (Nossa aplicação não dependerá desse pacote para ser executada, mas seria bom já instalá-lo caso surja a necessidade de executar alguma query SQL no banco)

```
sudo apt-get install sqlite3 libsqlite3-dev
```

### Criando uma Aplicação Flask para Teste

Antes de continuar com a nossa aplicação vamos criar uma aplicação em Flask o mais simples possível, apenas para verificar se o Flask foi instalado corretamente e está funcionando da maneira esperada.

Crie um arquivo chamado hello.py em qualquer diretório dentro de ~/Development:

Os parâmetros debug e port são opcionais mas facilita muito que o debug esteja ativado para visualizar erros que ocorram durante a codificação e utilizando uma porta diferente do padrão 80 possibilita que você rode mais de uma aplicação ao mesmo tempo sem ocorrer o conflito de portas.

> **Definição do \_\_name\_\_** :
>
> - Se o módulo é executado diretamente o \_\_name\_\_ é \_\_main\_\_
> - Se o módulo é importado por outro módulo então \_\_name\_\_ é o nome do próprio módulo

Com o virtualenv ativado, rode a linha de comando a baixo e em seguida abra o navegador no endereço “http://localhost:8085 ”:

```
python hello.py
```

Caso esteja tudo correto então você deverá visualizar uma página com a mensagem “Hello World”.

### Desenvolvendo a Aplicação “I Sweated Yesterday”

#### I Sweated Yesterday

É uma aplicação pequena e simples para controlar a frequência  de dias que um usuário realiza exercícios físicos. É chamado “Yesterday” porque na empresa em que trabalho, sempre marcamos o exercício físico ao chegarmos na empresa no dia seguinte. Foi baseada na aplicação exemplificada por Armin Ronacher nesta [página no Github](https://github.com/mitsuhiko/flask/wiki/Large-app-how-to).

> O código fonte da versão dessa aplicação exemplificada nesse post está disponível para download [aqui](https://github.com/maxclaus/i-sweated-yesterday/tree/v1.0) e a versão mais atual está [aqui](https://github.com/maxclaus/i-sweated-yesterday), qual ainda darei mais alguns retoques.

#### Visão Geral da Estrutura do Projeto

Minha dica aqui é que você crie os arquivos e diretórios abaixo para facilitar quando formos codificar ou faça o download do código fonte a cima.

##### Diretório Base

_Possui arquivos comuns para toda aplicação_

| /i-sweated-yesterday                  |                                                 |
| ------------------------------------- | ----------------------------------------------- |
| /i-sweated-yesterday/initialize-db.py | Configurações para inicializar o banco de dados |
| /i-sweated-yesterday/config.py        | Variáveis globais da aplicação                  |
| /i-sweated-yesterday/app.db           | Arquivo de banco de dados                       |
| /i-sweated-yesterday/run.py           | Roda a aplica cação                             |

##### Diretório da Aplicação

_Possui o arquivo main, responsável por toda aplicação_  

| /i-sweated-yesterday/app                 |                                                                                                                                               |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| /i-sweated-yesterday/app/\_\_init\_\_.py | Faz com que o Python trate o diretório como um módulo, realiza a inicialização do Flask, SQLAlchemy e realiza a configuração de rotas básicas |

##### Diretório – Módulo Users

_Possui arquivos relacionados ao módulo Users, como formulários, classes modelos (entidade/mapeamento banco), páginas htmls…_

| /i-sweated-yesterday/app/users                 |                                                                |
| ---------------------------------------------- | -------------------------------------------------------------- |
| /i-sweated-yesterday/app/users/\_\_init\_\_.py | Faz com que o Python trate o diretório como um módulo          |
| /i-sweated-yesterday/app/users/views.py        | Onde será tratado as requisições (Similar a Controller no MVC) |
| /i-sweated-yesterday/app/users/forms.py        | Definição formulários                                          |
| /i-sweated-yesterday/app/users/constants.py    | Definição de variáveis Constantes                              |
| /i-sweated-yesterday/app/users/models.py       | Definição de Modelos                                           |
| /i-sweated-yesterday/app/users/decorators.py   | Definição de Decorators                                        |
| /i-sweated-yesterday/app/users/requests.py     | Definição de Requisições Comuns                                |

##### Diretório – Módulo Exercises

_Possui arquivos relacionados ao módulo Exercises, como formulários, classes modelos (entidade/mapeamento banco), páginas htmls…_

| /i-sweated-yesterday/app/exercises                 |                                                                |
| -------------------------------------------------- | -------------------------------------------------------------- |
| /i-sweated-yesterday/app/exercises/\_\_init\_\_.py | Faz com que o Python trate o diretório como um módulo          |
| /i-sweated-yesterday/app/exercises/helpers.py      | Definição de algumas funções uteis                             |
| /i-sweated-yesterday/app/exercises/views.py        | Onde será tratado as requisições (Similar a Controller no MVC) |
| /i-sweated-yesterday/app/exercises/forms.py        | Definição formulários                                          |
| /i-sweated-yesterday/app/exercises/constants.py    | Definição de variáveis Constantes                              |
| /i-sweated-yesterday/app/exercises/models.py       | Definição de Modelos                                           |

##### Diretório Static

_Possui arquivos estáticos como css, imagens  e javascripts_

| /i-sweated-yesterday/app/static               |
| --------------------------------------------- |
| /i-sweated-yesterday/app/static/css           |
| /i-sweated-yesterday/app/static/css/reset.css |
| /i-sweated-yesterday/app/static/css/main.css  |
| /i-sweated-yesterday/app/static/js            |
| /i-sweated-yesterday/app/static/js/main.js    |
| /i-sweated-yesterday/app/static/img           |

##### Diretório Templates

_Possui arquivos de templates, ou seja, todas as páginas htmls puras ou com definições Jinja2_

| /i-sweated-yesterday/app/templates                      |
| ------------------------------------------------------- |
| /i-sweated-yesterday/app/templates/base.html            |
| /i-sweated-yesterday/app/templates/users                |
| /i-sweated-yesterday/app/templates/users/register.html  |
| /i-sweated-yesterday/app/templates/users/profile.html   |
| /i-sweated-yesterday/app/templates/users/login.html     |
| /i-sweated-yesterday/app/templates/forms                |
| /i-sweated-yesterday/app/templates/forms/macros.html    |
| /i-sweated-yesterday/app/templates/404.html             |
| /i-sweated-yesterday/app/templates/exercises            |
| /i-sweated-yesterday/app/templates/exercises/i_did.html |

#### Configuração

O arquivo **run.py** será utilizando durante o desenvolvimento para inicializar a aplicação e disponibilizar o acesso pelo browser no endereço [http://localhost:8090](http://localhost:8090):

```py
from app import app as application
application.run(debug=True,port=8090)
```

O arquivo **config.py** será utilizado para definição de valores comuns de toda aplicação. Mantendo essas informações centralizadas nesse arquivo facilitará durante a publicação para o ambiente de produção ou de testes, que essas informações sejam alteradas de acordo com a necessita de cada ambiente:

```py
import os

_basedir = os.path.abspath(os.path.dirname(__file__))

DEBUG = False

ADMINS = frozenset(['youremail@yourdomain.com'])
SECRET_KEY = 'SECRET_KEY_FOR_SESSION_SIGNING'

# Define the path of our database inside the root application,
# where 'app.db' is the database's name
SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(_basedir, 'app.db')
DATABASE_CONNECT_OPTION = {}

THREADS_PER_PAGE = 8

CSRF_ENABLED = True
CSRF_SESSION_KEY = 'SOMETHING_IMPOSSIBLE_TO_GUEES'
```

> **\_basedir** : mantém o caminho de onde os scripts são executados
>
> **DEBUG** : utilizado como _True_ em ambientes de desenvolvimento, permite que seja exibido detalhadamente a requisição, caso ocorra algum erro durante o processo
>
> **SECRET_KEY** : utilizado na criação dos cookies
>
> **ADMINS** : email do Administrador responsável pelo site
>
> **SQLALCHEMY_DATABASE_URI** e **DATABASE_CONNECT_OPTIONS** : opções de conexão do SQLAlchemy
>
> **CSRF_ENABLED** e **CSRF_SESSION_KEY** : proteção contra fraude de posts

O arquivo **initialize-db.py** será utilizado para criar a estrutura de tabelas que iremos configurar mais adiante em nossas Models:

```py
from app import db

# Drop all tables from db file
db.drop_all()

# Create all tables on db file,
# copying the structure from the definition on the Models
db.create_all()
```

#### Main

O arquivo **\_\_init\_\_.py** será o main da aplicação, nele estará a definição do Flask, SQLAlchemy e as rotas comuns:

```py
from flask import Flask, render_template, redirect, url_for
from flask.ext.sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config.from_object('config')

db = SQLAlchemy(app)


# Basic Routes #

@app.route('/')
def index():
	return redirect(url_for('users.index'))

@app.errorhandler(404)
def not_found(error):
	return render_template('404.html'), 404


# BluePrints - Modules #

# Users Module
from app.users.views import mod as usersModule
app.register_blueprint(usersModule)

# Exercises Module
from app.exercises.views import mod as exercisesModule
app.register_blueprint(exercisesModule)
```

> **Blueprint** basicamente permite que um módulo estenda a aplicação principal e funcione similarmente a aplicação Flask. Sendo esta uma das grandes vantagem para aplicações maiores, por permitir a modularização de uma aplicação, o que facilita em muito a organização, desenvolvimento e manutenções do código fonte.
>
> Veja mais sobre [Blueprints](http://flask.pocoo.org/docs/blueprints/).

#### Módulo Usuário

Como este será um módulo estendido da aplicação principal, o nosso arquivo **\_\_init\_\_.py** estará vazio, pois objetos que precisamos já foram inicializados pela Main.

O arquivo **constants.py** é simples e contêm apenas valores Constantes, centralizados todos no mesmo arquivo para priorizar a organização e facilitar caso surja a necessidade de alguma manutenção futura:

```py
# User role
ADMIN = 0
STAFF = 1
USER = 2
ROLE = {
	ADMIN: 'admin',
	STAFF: 'staff',
	USER: 'user',
}

# User status
INACTIVE = 0
NEW = 1
ACTIVE = 2
STATUS = {
	INACTIVE: 'inactive',
	NEW: 'new',
	ACTIVE: 'active',
}

# Session Name
SESSION_NAME_USER_ID = 'user_id'
```

O **models.py** contém a definição da model User, sendo basicamente estruturada pela definições de propriedades, mapeamentos model/database, construtores da classe e métodos GET e SET:

```py
# third party imports
from sqlalchemy.sql import extract, func

# local application imports
from app import db
from app.users import constants as USER
from app.exercises.models import Exercise
from app.exercises.helpers import DateHelper



class User(db.Model):
	# Map model to db table
	id = db.Column(db.Integer, primary_key=True)
	name = db.Column(db.String(50), unique=True)
	email = db.Column(db.String(120), unique=True)
	password = db.Column(db.String(20))
	role = db.Column(db.SmallInteger, default=USER.USER)
	status = db.Column(db.SmallInteger, default=USER.NEW)
	exercises = db.relationship('Exercise', backref='user', lazy='dynamic')


	# Class Constructor
	def __init__(self, id=None):
		self.id = id


	# Factory Constructor of a new user to register
	@classmethod
	def NewUserToRegister(cls, name=None, email=None, password=None):
		_user = cls()
		_user.name = name
		_user.email = email
		_user.password = password
		return _user



	def getNameTitle(self):
		return self.name.title()


	def getStatus(self):
		return USER.STATUS[self.status]


	def getRole(self):
		return USER.ROLE[self.role]


	def getTotalExercises(self):
		return len(self.exercises.all())


	def getTotalExercisesCurrentWeek(self):

		start_end_week = DateHelper.get_start_end_days_current_week()
		start_week = start_end_week[0]
		end_week = start_end_week[1]

		return len(db.session.query(Exercise.id)
						.filter(Exercise.user_id == self.id)
						.filter(Exercise.date >= start_week)
						.filter(Exercise.date <= end_week)
						.all())


	def getTotalExercisesCurrentMonth(self):

		current_month = DateHelper.current_month()

		return len(db.session.query(Exercise.id)
						.filter(Exercise.user_id == self.id)
						.filter(extract('month', Exercise.date) == current_month)
						.all())


	def alreadyDidExerciseYesterday(self):

		yesterday = DateHelper.get_yesterday()

		return (len(db.session.query(Exercise.id)
						.filter(Exercise.user_id == self.id)
						.filter(Exercise.date == yesterday)
						.all()) > 0)


	def __repr__():
		return '<User %r><user   %r>' % (self.name)
```

O **forms.py**  utiliza o módulo _WTForms_ para gerar formulários de maneira fácil e simples, como campos associados a labels e propriedades _Required_, caso seja obrigatório:

```
from flask.ext.wtf import Form, TextField, PasswordField, BooleanField
from flask.ext.wtf import Required, Email, EqualTo

class LoginForm(Form):
	email = TextField('Email address', [Required(), Email()])
	password = PasswordField('Password', [Required()])


class RegisterForm(Form):
	name = TextField('NickName', [Required()])
	email = TextField('Email address', [Required()])
	password = PasswordField('Password', [Required()])
	confirm = PasswordField('Repeat Password', [
		Required(),
		EqualTo('confirm', message='Passwords must match')
		])
```

O **decorators.py** possui a declaração para o decorator requires_login, no qual verifica se o usuário está logado, caso não esteja redireciona o usuário para a página de login:

```py
from functools import wraps
from flask import g, flash, redirect, url_for, request

def requires_login(f):
	@wraps(f)
	def decorated_function(*args, **kwargs):
		if g.user is None:
			flash(u'You need to be signed in for this page.')
			return redirect(url_for('users.login', next=request.path))
		return f(*args, **kwargs)
	return decorated_function
```

> **flask.g** : é um objeto no Flask que possui a responsabilidade de armazenar e compartilhar dados através do tempo de execução de uma requisição
>
> **\*args** : o **\*** é utilizado para permitir que um parâmetro aceite um número não definido de argumentos para sua função (parecido com a keyword _params_ no C#)
>
> **\*\*kwargs** : Da mesma forma, **\*\*** permite lidar com argumentos nomeados que não foram previamente definidos (parecido com a keyword _params+dictonary_ no C#)

O **requests.py** possui a declaração de uma função comum a várias páginas na aplicação. No qual se existir um usuário logado na requisição atual será recuperado o Id do usuário na sessão, então recupera-rá o objeto usuário no banco e será mantido até o final da requisição no objeto **flask.g.user**:

```py
# third party imports
from flask import g, session

# local application imports
from app.users.constants import SESSION_NAME_USER_ID
from app.users.models import User


def app_before_request():
	# pull user's profile from the db before every request are treated
	g.user = None
	if SESSION_NAME_USER_ID in session and session[SESSION_NAME_USER_ID] is not None:
		g.user = User.query.get(session[SESSION_NAME_USER_ID])
```

O **views.py**, para quem está acostumado com MVC funciona como uma Controller, é onde será definido as requisições possíveis para esse módulo, associando as rotas existentes as funcionalidades específicas de cada uma. Como dito anteriormente esse módulo não será uma aplicação e sim um módulo estendido da aplicação principal. Então ao invés de utilizarmos um objeto do tipo **Flask**, como para a definição de rotas, iremos utilizar o **Blueprint**.

```py
# third party imports
from flask import Blueprint, request, render_template, flash, g, session, redirect, url_for
from werkzeug import check_password_hash, generate_password_hash
from datetime import datetime
from sqlalchemy.sql import or_


# local application imports
from app import db
from app.users.forms import RegisterForm, LoginForm
from app.users.models import User
from app.users.requests import app_before_request
from app.users.decorators import requires_login
from app.users.constants import SESSION_NAME_USER_ID
from app.exercises.models import Exercise


mod = Blueprint('users', __name__, url_prefix='/users')


@mod.before_request
def before_request():
	app_before_request()


@mod.route('/')
@mod.route('/me/')
@requires_login
def index():
	return render_template('users/profile.html', user=g.user)


@mod.route('/login/', methods=['GET', 'POST'])
def login():
	form = LoginForm(request.form)

	# make sure data are valid, but doesn't validate password is right
	if form.validate_on_submit():

		user = User.query.filter_by(email=form.email.data).first()

		# we use werzeug to validate user's password
		if user and check_password_hash(user.password, form.password.data):

			# the session can't be modified as it's signed,
			# it's a safe place to store the user id
			session[SESSION_NAME_USER_ID] = user.id

			flash('Welcome %s' % user.name)
			return redirect(url_for('users.index'))

		flash('Wrong email or password', 'error-message')

	return render_template( 'users/login.html', form=form)


@mod.route('/register/', methods=['GET', 'POST'])
def register():
	form = RegisterForm(request.form)

	if form.validate_on_submit():

		userRegistered = User.query.filter(or_(User.name == form.name.data,
							User.email == form.email.data)).first()

		if userRegistered is not None:
			flash('Email or user already is registered')
			return render_template('users/register.html', form=form)


		# create an user instance not yet stored in the database
		user = User.NewUserToRegister(form.name.data, form.email.data,
						generate_password_hash(form.password.data))

		# insert the record in our database and commit it
		db.session.add(user)

		db.session.commit()

		# log the user in, as he now has an id
		session[SESSION_NAME_USER_ID] = user.id

		# flash will display a message to the user
		flash('Thanks for registering')

		# redirect user to the 'index' method of the user module
		return redirect(url_for('users.index'))
	return render_template('users/register.html', form=form)


@mod.route('/logout/', methods=['GET'])
def logout():
	# remove the username from the session if it's there
	session.pop(SESSION_NAME_USER_ID, None)

	# flash will display a message to the user
	flash('Do not forget keep the exercises')

	# redirect user to the 'index' method of the user module
	return redirect(url_for('users.login'))
```

> **validate_on_submit** : função do WTForm, no qual verifica se o formulário é valido durante uma requisição do tipo POST ou PUT
>
> **render_template** : retorna um documento renderizado, no nosso caso todos html, de acordo com o arquivo template e o formulário passado
>
> **flash** : possibilita que mensagens declaradas na view sejam recuperadas no template e exibidas para o usuário
>
> **@mod.route('/login/', methods=\['GET', 'POST'\])**  : associa uma rota a uma função que estiver após sua definição. O primeiro parâmetro define por qual caminho será executado e o segundo parâmetro quais os métodos REST serão aceitos por essa rota.

#### Templates

Como Flask já possui integrado o Jinja, vamos utilizar em nossos templates algumas de suas funcionalidades disponíveis, como estruturas de condições, laços de repetições, blocos de conteúdos. Permitindo um alto nível de reutilização e fácil desenvolvimento.

O **base.html** será o template base, do qual será herdado pelos outros templates. Nesse template teremos a definição do corpo _HTML, HEAD, BODY_ e blocos de conteúdo que poderão ser substituídos com outros conteúdos pelos templates que o herdarem.

```html
<html>
  <head>
    <meta name="viewport" content="width=device-width; initial-scale=1.0; maximum-scale=1.0; user-scalable=0;" />
    <title>{% block title %}I Sweated Yesterday{% endblock %}</title>
    {% block css %}
    <link rel="stylesheet" href="/static/css/reset.css" />
    <link rel="stylesheet" href="/static/css/main.css" />
    {% endblock %} {% block script %}
    <script src="/static/js/main.js" type="text/javascript"></script>
    {% endblock %}
  </head>
  <body>
    <div id="wrapper">
      <div id="header">
        {% block header %}
        <h1>I Sweated Yesterday</h1>
        {% endblock %}
      </div>
      <div id="messages">
        {% for category, msg in get_flashed_messages(with_categories=true) %} {{ msg }} {% endfor %}
      </div>
      <div id="content" class="shadow">
        {% block logout %}
        <a class="bt-action-logout" href="{{ url_for('users.logout') }}"> Logout </a>
        {% endblock %} {% block content %}{% endblock %}
      </div>
      <div id="footer">{% block footer %}{% endblock %}</div>
    </div>
  </body>
</html>
```

O **macros.html** possuirá definições de macros. No nosso caso criamos um macro chamado **render_field** no qual será responsável por criar uma estrutura _HTML_ para os campos dos nossos formulários. Um detalhe importante desse macro é que caso exista alguma validação que não tenha passado durante a execução do _validate_on_submit_ na view, então as mensagens de erro serão adicionados no _HTML_ gerado.

```html
{% macro render_field(field) %}
<div class="form_field">
  {{ field.label(class="label") }} {% if field.errors %} {% set css_class = 'has_error' + kwargs.pop('class', '') %} {{
  field(class=css_class, **kwargs) }}
  <ul class="errors">
    {% for error in field.errors %}
    <li>{{ error|e }}</li>
    {% endfor %}
  </ul>
  {% else %} {{ field(**kwargs) }} {% endif %}
</div>
{% endmacro %}
```

O **register.html** herda o base.html e substitui os blocos de conteúdo _logout, content e footer_. Este template será responsável pelo _HTML_ utilizado no registro de um novo usuário, por isso o método utilizado é o POST e action referencia a url atual sem o nome da página, no qual se verificarmos na view estará apontando para a função _register_:

```html
{% extends "base.html" %} {% block logout %}<!-- remove btn logout -->{% endblock %} {% block content %} {% from
"forms/macros.html" import render_field %}
<form method="POST" action="." class="form">
  {{ form.csrf_token }} {{ render_field(form.name, class="input text") }} {{ render_field(form.email, class="input
  text") }} {{ render_field(form.password, class="input text") }} {{ render_field(form.confirm, class="input text") }}
  <input type="submit" value="Register" class="button green" />
</form>
{% endblock %} {% block footer %}
<a class="bt-action bt-action-user" href="{{ url_for('users.login') }}">
  <span>Login</span>
</a>
{% endblock %}
```

A partir de agora será melhor que você siga o resto da aplicação analisando e copiando direto do código fonte disponibilizado mais a cima nesse tutorial, se não este artigo irá ficar muito maior do que já está.

#### Executando a Aplicação

No primeiro momento que executarmos a aplicação é necessário inicializar o banco de dados, para isso abra o Terminal do sistema operacional, acesse o diretório da aplicação e execute a linha de comando a baixo (não esqueça de antes ativar o ambiente virtual):

```
python initialize-db.py
```

Após executado a linha de comando a cima, só a execute novamente caso você deseje resetar o banco de dados, pois será criado as tabelas novamente e perderá todos os dados existentes.

Para inicializar a aplicação execute a linha de comando a baixo:

```
python run.py
```

Se a aplicação não possuir nenhuma falha grave será exibido a seguinte mensagem no Terminal:

```
* Running on http://127.0.0.1:8090/
* Restarting with reloader
```

Para visualizar a aplicação abra qualquer browser com a seguinte URL: [http://127.0.0.1:8090/](http://127.0.0.1:8090/ "http://127.0.0.1:8090/").

## O que é o Python Any Where?

<a name="pythonanywhere"></a>

![](./PythonAnywhere.png)

O [Python Any Where](https://www.pythonanywhere.com) é uma IDE online e serviço de hospedagem baseado em Python. No qual disponibiliza a hospedagem web de aplicações python e acesso ao um console online possibilitando desenvolvimento em python com várias bibliotecas disponíveis de maneira online, fácil e gratuita para o plano mais simples.

### Publicando

1.  Crie um cadastro no Python Any Where
2.  Selecione a opção _**I want to create a web application**_
3.  Selecione a aba **_Web_** e em seguida a opção _**Add a new web app**_
4.  Selecione **_Next_** na primeira janela
5.  Selecione **_Flask_** para definir o Framework Web Python a ser utilizado
6.  Selecione **_Next_** para confirmar o caminho físico da sua aplicação
7.  Após criada a opção _**It is configured via WSGI file stored at**_ para editar arquivo wsgi
8.  Edite o arquivo wsgi, atualizando o valor da variável projec\*home com o caminho físico correto da sua aplicação (abra a aba Files para confirmar o caminho correto) e na última linha certifique-se de estar realizando a importação correta, caso você tenha deixado a variável instanciado do Flask no arquivo \_\_init\_\_.py da aplicação como “app”, então você deixará a última linha da forma que está:\*\**/ > var > www > YOUR-USER-NAME*pythonanywhere_com_wsgi.py\* :\*\*

    ```py
    # This file contains the WSGI configuration required to serve up your # web application at http://<your-username>.pythonanywhere.com/ # It works by setting the variable 'application' to a WSGI handler of some # description. # # The below has been auto-generated for your Flask project
    import sys

    # add your project directory to the sys.path
    project_home = u'/home/YOUR-USER-NAME/i-sweated-yesterday'
    if project_home not in sys.path:
        sys.path = [project_home] + sys.path

    # import flask app but need to call it "application" for WSGI to work
    #from flask_app import app as application #example
    from app import app as application</your-username>
    ```

9.  Salve o arquivo anterior
10. Acesse a aba **Files** e em seguida o diretório : **_/ > home >  YOUR-USER-NAME_**
11. Clique no botão **Escolher Arquivo**
12. Selecione o seu projeto compactado como .zip
13. Clique em Upload
14. Acesse a aba **Consoles** e selecione a opção **Bash**
15. Execute o seguinte comando no console: `unzip SEU-PROJETO-COMPACTADO.zip`
16. Para inicializar o banco de dados execute o seguinte comando no console: `python YOUR-PROJECT-DIRECTORY/initialize-db.py`
17. Acesse a aba Web e clique em **Reload web app**
18. Finalmente para visualizar o aplicativo rodando clique no link **You can see your web app at** disponível na aba Web (caso ocorra algum erro ao abrir a aplicação, veja a descrição mais detalhada do problema nos logs de erros disponíveis também na aba Web)

> **Wsgi** (Web Server Gateway Interface): Define uma simples interface entre Servidores Web e Aplicações Web ou Frameworks para Python.
>
> **Dica** : Utilizamos a função de Upload para enviar o código fonte da nossa aplicação do computador local para o servidor web. Mas seria ainda mais fácil quando utilizado o Dropbox ou melhor ainda o Github. De uma olhada nisso, vai facilitar ainda mais o deploy quando realizado várias vezes.

