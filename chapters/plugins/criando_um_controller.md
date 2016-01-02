# Criando um controller

Você já deve ter imaginado que o Redmine possui um generator para o controller

```sh
$ rails generate redmine_plugin_controller <plugin_name> <controller_name> [<actions>]
```

Seguindo nosso exemplo, podemos criar um controller para as enquetes

```sh
$ rails generate redmine_plugin_controller polls polls
  create  plugins/polls/app/controllers/polls_controller.rb
  create  plugins/polls/app/helpers/polls_helper.rb
  create  plugins/polls/test/functional/polls_controller_test.rb
```

É uma boa prática seguir o padrão rest no rails que consiste nas actions:

| método HTTP | Nome da Action | Motivo                    |
| ----------- | -------------- | ------------------------- |
| GET         | index          | Listar                    |
| GET         | show           | Mostrar                   |
| GET         | new            | Formulário de criação     |
| POST        | create         | Gravar modelo no banco    |
| GET         | edit           | Formulário de ediçào      |
| PUT/PATCH   | update         | Atualizar modelo no banco |
| DELETE      | destroy        | Apagar modelo do banco    |

Por exemplo, se quisermos criar uma *action* **index** para o controller devemos editar o controller para:

```rb
class PollsController < ApplicationController
  unloadable

  def index
    @polls = Poll.all # busca todas as enquetes do banco
  end
end
```

> Mas o que é essa variável com @?

Em ruby o escopo da variável é definido da seguinte forma:

* Sem @, escopo local, somente dentro do bloco de código que se encontra e bloco de códigos filhos
* @, escopo instância, a variável é vista dentro de qualquer método do objeto
* @@, escopo classe, no Java seriam as variáveis estáticas.

Como já mencionado, as views enchergam as variáveis de instância dos controllers que as renderizaram.
