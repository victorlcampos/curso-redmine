# Permissões

Muitas vezes queremos que só um determinado grupo de pessoas dentro do redmine possa executar uma determinada tarefa. Para esse controle de acesso o redmine  um mecanismo de permissões.

Esse mecanismo permite que os plugins definem novas permissões dentro do seu init.rb. As permissões funcionam da seguinte forma:

```rb
    permission permission_name, action, options
```

Action são quais rotas essa permissão da acesso ao usuário no formato:

```rb
{ controller_name: [:action1, :action2]}
ou
{ controller_name: :action1 }
```

e options podem ser:

* Public _(true/false)_: onde setar como true implica em dar essa permissão específica para todos os usuários

* Require _(:loggedin/:member)_: retringe para quem você pode dar a permissão, somente usuários logados ou somente membros do projeto

* Read _(true/false)_: Permissão continua valendo mesmo para projetos fechados.

Vamos dizer que na nossa enquete, a gente divida em dois grupos: Usuários que podem votar na enquete e usuários que podem ver os resultados. Para isso teremos que editar o *init.rb* do nosso plugin para criar essas duas permissões.

```rb
Redmine::Plugin.register :polls do
  name 'Polls plugin'
  author 'Author name'
  description 'This is a plugin for Redmine'
  version '0.0.1'
  url 'http://example.com/path/to/plugin'
  author_url 'http://example.com/about'

  permission :view_polls, polls: :index
  permission :vote_polls, polls: :vote
end
```

Com isso a seguinte opção aparece na edição de papeis e permissões:

![](Screenshot from 2016-01-23 12:24:19.png)

Para as permissões valerem para o redmine, é necessário fazer um pequeno ajuste nos controller. Antes de qualquer action é necessário setar um @project para o projeto que o usuário está acessando e chamar o método authorize do redmine.



