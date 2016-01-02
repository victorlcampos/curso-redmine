# Sobre o Redmine


# Sobre o livro
Neste livro veremos como desenvolver plugins para Redmine 3.1. Com foco na teoria e técnicas de programação da framework Rails e como se aplica ao Redmine.

<sub><sup>Não é um livro de Ruby on Rails</sup></sub>

### Fluxo de Dados


### Estrutura de Pastas

#### Subpastas importantes
##### App

##### Public

[Lista de temas da Comunidade](http://www.redmine.org/projects/redmine/wiki/Theme_List)

### Plugins
#### Quando devemos desenvolver um plugin?


#### Como criar um plugin

#### Criando um modelo

Novamente, para criar um modelo dentro de um plugin, podemos chamar o generator do próprio redmine
```sh
$ rails generate redmine_plugin_model <plugin_name> <model_name> [field[:type][:index] field[:type][:index] ...]
```

Podendo reparar que é muito parecido com o generator de modelo do rails.
Então vamos criar um modelo poll dentro do plugins polls, que vai ter uma question do tipo string, uma contagem de sim e uma contagem de não, ambos do tipo numérico.

```sh
$ rails generate redmine_plugin_model polls poll question:string yes:integer no:integer
    create  plugins/polls/app/models/poll.rb
    create  plugins/polls/test/unit/poll_test.rb
    create  plugins/polls/db/migrate/001_create_polls.r
```

Reparem, ele criou um modelo na pasta models e uma migração na pasta migrations

Vamos olhar essa migração:

```rb
class CreatePolls < ActiveRecord::Migration
  def change
    create_table :polls do |t|
      t.string :question
      t.integer :yes
      t.integer :no
    end
  end
end
```

Vamos entender a migração, ela chama uma função *create_table* que recebe como argumento um símbolo (:polls) e um bloco de código que recebe também um argumento(t).

> Primeiro, o que é um símbolo? Você deve estar se perguntando

Um simbolo é parecido com a String do Java. A diferença entre "polls" e :polls é que o primeiro é mutável, se você fizer:

```rb
  a = "a"
  a += "b"
  print a
    => "ab"
  a = nil
```
Quando o GC for executado, não teremos nada alocado na memória.

Já um símbolo, não possui um método para concatenar e se executácemos:

```rb
  a = :a
  print a
    => "a"
  a = nil
```

O símbolo :a, não seria arrancado da memória. Isso é perigoso quando criamos símbolos dinamicamente e é uma [vunerabilidade](http://brakemanscanner.org/docs/warning_types/denial_of_service/) conhecida do Rails.

Mas porque então usamos ele ao invés de String, a resposta é simples: desempenho.

Se você cria um símbolo de maneira controlada, sempre que você for acessa-lo, não precisará realocar memória e não importa quantas variáveis apontem para ele, elas vão estar consumindo a mesma quantidade de memória, pois estarão apontando sempre para a mesma posição.

> Agora que já entendi o que é um símbolo, o que diabos é passar um bloco de código como argumento?

Bom, em ruby uma função pode receber um bloco de código e executa-lo dentro dela. O equivalente em Java 7 seria instanciar uma interface e preencher os métodos, muito usado nos handlers da vida.

No caso da função create_table ela executa o que tem que executar, instância um objeto e passa para a nossa função anônima. Como o ruby é Duck Typing, se o objeto t recebido pela nossa função anônima tiver todos os métodos necessários para o bloco de código ser executado, então o bloco será executado sem problema.

Esse bloco irá chamar os métodos do objeto t que criam as colunas, o método string cria uma coluna do tipo string, o integer do tipo integer, etc...
O método create_table cria sozinho a coluna id e os timestamps.

Para saber mais sobre as migrações do Rails acesse http://guides.rubyonrails.org/v3.2.21/migrations.html

Para executar as migrações dentro dos plugins:

```sh
$ rake redmine:plugins:migrate
Migrating polls (Polls plugin)...
==  CreatePolls: migrating ====================================================
-- create_table(:polls)
   -> 0.0323s
==  CreatePolls: migrated (0.0324s) ===========================================
```

O rails irá criar a table a as colunas para você, "indendente" do banco que esteja configurado no seu database.yml

Utilizando **convenção sobre configuração**, ele irá atribuir todas as colunas da tabela polls(plural) ao modelo poll(sigular) como métodos, já com getters & setters

```rb
class Poll < ActiveRecord::Base
  unloadable
end

poll = Poll.new
poll.question = "Question 1"
print poll.question
=> "Question 1"
print poll.yes
=> nil
```
#### Criando um Controller
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

#### Adicionando rota

Como explicado no [Fluxo de Dados](#fluxo-de-dados), quando uma url é chamada, o Rails busca nos arquivos de rota para qual controller#action ele deve direcionar o chamado. Cada plugin do redmine possui seu próprio arquivo de rotas, que fica dentro de _config/_.

Podemos adicionar rotas de duas maneiras no Rails, a primeira no formato

```rb
  http_method url_partner, to: 'controller#action'
```

No nosso exemplo seria:

```rb
  get 'pools', to: 'polls#index'
```

Quando acessácemos 'http://localhost:3000/polls' a action index do controller polls seria executada. Mas isso aparenta ser pouco **convenção sobre configuação** correto? Não parece ser muito rails way de fazer as coisas.

Bom, o rails possui o método _resources(controller_name, opts)_ que define todas as rotas do rest por padrão, então podemos definir:

```rb
  resources :polls
```

O rails possui por padrão uma rake task que lista todas as rotas criadas, vamos roda-la e usar o grep para filtrar somente as rotas do Controller polls

```sh
$ rake routes | grep polls
polls GET          /polls(.:format) polls#index
      POST         /polls(.:format) polls#create
new_poll GET          /polls/new(.:format) polls#new
edit_poll GET          /polls/:id/edit(.:format) polls#edit
 poll GET          /polls/:id(.:format) polls#show
      PUT          /polls/:id(.:format) polls#update
      DELETE       /polls/:id(.:format) polls#destroy
```

Podemos reparar que todas as rotas necessárias para um serviço rest foram criadas. Se você paar para analisar um segundo, verá que a saída tem uma primeira coluna, o que será ela?

Para você não sair escrevendo urls de forma hardcode, o rails cria por padrão dois métodos para cada rota, uma com sufixo path e uma com sufixo url, no nosso caso temos a polls_path e polls_url que tem como retorno /polls e http://localhost:3000/polls respectivamente.

Com isso você não precisa modificar as urls em produção.

> Ok, achei super legal, mas eu somente queria uma rota para index e ele criou várias que eu nem preciso usar.

É verdade, por isso uma das opções que o método aceita é _only_. Podemos passar da seguinte maneira:

```rb
  resources :polls, only: [:index]
```

Sendo agora somente criado a rota para a action index.

As rotas no rails são **MUITO** poderosas e poderia escrever um artigo somente sobre elas, então aconselho a dar uma olhada na guia oficial da linguagem para conhecer essa ferramenta incrível: http://guides.rubyonrails.org/routing.html

#### Adicionando um link no menu

O redmine possui diversos menus que você pode adicionar links para seus controllers, são eles:

* :top_menu - menu superior esquerdo
* :account_menu - menu superior direito, onde ficam os links de login e sair
* :application_menu - menu principal fora de projetos
* :project_menu - menu principal dentro de projetos
* :admin_menu - menu dos administradores

Para adicionar um menu, é necessário editar o arquivo init.rb e dentro do registro do plugin usar o método menu que tem a sintaxe:

```rb
menu(menu_name, item_name, url, options={})
```

No nosso exemplo ficaria:

```rb
  menu :application_menu, :polls, { controller: 'polls', action: 'index' }
```

As opções que podemos passar são:

* :param - the parameter key that is used for the project id (default is :id)
* :if - a Proc that is called before rendering the item, the item is displayed only if it returns true
* :caption - the menu caption that can be:
  * a localized string Symbol
  * a String
  * a Proc that can take the project as argument
* :before, :after - specify where the menu item should be inserted (eg. :after => :activity)
* :first, :last - if set to true, the item will stay at the beginning/end of the menu (eg. :last => true)
* :html - a hash of html options that are passed to link_to when rendering the menu item

Com isso, criaremos um link para a nossa action no menu principal fora dos projetos

#### Internacionalização

A última coisa que falta para o menu, é que nossos clientes são Brasileiros e falam português, eles não querem polls e sim enquetes escrito no menu. Para fazer essa alteração, iremos criar um arquivo chamado pt-BR.yml dentro do config/locates do plugin e preencher com o seguinte conteúdo:

```yml
pt-BR:
  label_polls: Enquetes
```

Caso a opção caption não seja passada, **e ela não deve ser**, o redmine usa o _menu_name_ préfixado com _label_ para fazer a internacionalização do menu.

Para saber mais sobre internacionalização no rails: http://guides.rubyonrails.org/i18n.html

#### Criando uma View

Agora que já temos uma rota, um controller e um botão que o usuário pode clicar para acessar nosso controller, precisamos codificar o que o usuário vai ver quando acessar nossa action. Como falado anteriormente o Rails vai procurar dentro da pasta com o mesmo nome do controller, _polls_, uma view com o mesmo nome da action, _index_.

Então criaremos o arquivo index.html.erb dentro da pasta app/views/polls do plugin e colocaremos o seguinte conteúdo:

```erb
<h1>Polls</h1>
<ul>
  <% @polls.each do |polls| %>
    <li><%= polls.question %></li>
  <% end %>
</ul>
```

> O que diabos é .erb depois do .html?

erb ou Embedded Ruby ou mesmo eRuby é o template padrão do Rails, existem diversos outros e a comunidade é bem dividida nesse ponto. O rails permite adicionar qualquar preprocessador de arquivo estático, sempre lendo as extensões da direita para esquerda. Se tivéssemos um javascript que quiséssemos que antes dele ser enviado para o cliente, rodasse código ruby, poderíamos criar o arquivo _file.js.erb_.

Para entender mais sobre views, que é um dos pontos mais completos do Rails. Ele aceita partials, layouts, content, e várias outras features que facilitam o **dry**, leia o http://guides.rubyonrails.org/layouts_and_rendering.html

#### Assets

Vale ressaltar que view feia não agrada cliente e muitas vezes precisamos escrever css e javascript específicos para um plugin. Para isso o redmine permite adicionar assets da seguinte forma:

```erb
<% content_for :header_tags do %>
  <%= stylesheet_link_tag 'css_name', plugin: 'plugin_name' %>
  <%= javascript_link_tag 'js_name' , plugin: 'plugin_name' %>
<% end %>
```

Se você leu o guia sobre layout como sugerido, saberá que este _content_for_ rodará o bloco de código passado por ele na posição onde tiver um yield(:header_tags), que no nosso caso fica dentro da tag

```html
<header></header>
```

O método _stylesheet_link_tag_, adiciona um link para o css com nome _css_name_ que se encontra dentro do plugin com nome igual _plugin_name_, assim como o _javascript_link_tag_ fará para o javascript.

Com isso você não precisará modificar o código direto dos assets do redmine para fazer uma modificação específica da sua view.

#### Permissões
#### Módulos
#### Hooks
##### Hooks nas Views
##### Hooks nos controllers
#### Fazendo um plugin ser configurável
#### Sobrescrevendo um Modelo
#### Sobrescrevendo um Controller
#### Sobrescrevendo uma View

### Fonte de Estudo

1. http://guides.rubyonrails.org/
2. http://www.redmine.org/projects/redmine/wiki/Plugin_Tutorial
3. http://www.redmine.org/projects/redmine/wiki/Hooks
4. http://www.redmine.org/projects/redmine/wiki/Plugin_Internals

### Futuro
A ideia de desenvolver o curso no github é deixar ele colaborativo e expansível, assim como o Redmine. Gerando assim uma apostila completa sobre o assunto, que se mantenha sempre atualizada.

### Colaboradores
Quem contribuir com esse material, peço que mande um pull request adicionado o seu nome na lista abaixo.
- Victor Lima Campos(victorlcampos)
- Annanda Sousa (annanda)

### Alunos
Quem utilizar esse material para estudo, peço que mande um pull request adicionado o seu nome na lista abaixo.
- Victor Lima Campos (victorlcampos)
- Annanda Sousa (annanda)
- Julio Nascimento (juliocesarnrocha)
- Gabriel Rodrigues (gabrieldesar)
