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
 permission :view_polls, polls: :index
 permission :vote_polls, polls: :vote
```