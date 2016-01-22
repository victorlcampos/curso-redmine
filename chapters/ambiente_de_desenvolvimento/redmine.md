# Redmine

## Introdução

![](http://www.redmine.org/attachments/3458/redmine_logo_v1.png)

O Redmine é um gerenciador de projeto **muito** flexível, extensível e configurável. Como o código é aberto, conseguimos utiliza-lo para atender as mais variadas demandas dos clientes. As principais features nativas são:
- Multiple projects support
- Flexible role based access control
- Flexible issue tracking system
- Gantt chart and calendar
- News, documents & files management
- Feeds & email notifications
- Per project wiki
- Per project forums
- Time tracking
- Custom fields for issues, time-entries, projects and users
- SCM integration (SVN, CVS, Git, Mercurial, Bazaar and Darcs)
- Issue creation via email
- Multiple LDAP authentication support
- User self-registration support
- Multilanguage support
- Multiple databases support

## Instalação

Para instalar o Redmine, vamos utilizar a última versão estável do redmine 2.6, que no momento de criação desse curso era a 2.6.6

```sh
$ svn co https://svn.redmine.org/redmine/branches/2.6.6-stable redmine-2.6.6

$ cd redmine-2.6.6
```

Tendo o código do redmine, precisamos configurar o arquivo do banco de dados e o arquivo de configuração de e-mail. Por enquanto vamos utilizar o examplo do próprio redmine.

```sh
$ cp ./config/database.yml{.example,}
$ cp ./config/configuration.yml{.example,}
```

Assim como no Java temos o Maven para baixar as dependências, no Ruby temos o Bundle, para utiliza-lo basta fazer:

```sh
$ bundle install
```

Ele irá olhar o arquivo Gemfile, na pasta raiz do projeto e instalar todas as dependências que lá estiver.

> O Bundle faz tudo que o Maven faz?

**NÃO**, ele, diferente do Maven, se foca em fazer bem uma única coisa: gerenciar dependências.

Para automatizar tarefas, temos o **rake**, vamos utilizar para gerar o token de segurança de sessão.

```sh
$ rake generate_secret_token
```

Também precisamos gerar as tabelas do banco de dados que o redmine usa. O Rails por padrão possui migrações, arquivos em ruby(.rb) que descreve as operações que devemos realizar no banco.

Podemos com o rake rodar todas essas migrações e o Rails se encarrega de transformar no sql certo para o banco descrito no database.yml

```sh
$ rake db:create
$ rake db:migrate
```

Pronto, agora estamos pronto para rodar o Redmine

```sh
$ rails server
```
