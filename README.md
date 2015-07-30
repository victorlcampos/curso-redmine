# Curso Redmine
## Ementa
- [O Curso](#o-curso)
- [Ambiente de Desenvolvimento](#ambiente-de-desenvolvimento)
  - [IDEs](#ides)
    - [Sublime](#sublime)
    - [Atom](#atom)
    - [RubyMine](#rubymine)
  - [Ruby](#ruby)
    - [Introdução](#introdução)
    - [Instalar](#instalar)
  - [Rails](#rails)
    - [Introdução](#introdução-1)
    - [Instalar](#instalar-1)
  - [Redmine](#redmine)
    - [Introdução](#introdução-2)
    - [Instalar](#instalar-2)
- [Fluxo de Dados](#fluxo-de-dados)
- [Estrutura de Pastas](#estrutura-de-pastas)
  - [Subpastas importantes](#subpastas-importantes)
    - [App](#app)
    - [Public](#public)
- [Plugins](#estrutura-plugins)
  - [Quando devemos desenvolver um plugin?](#quando-devemos-desenvolver-um-plugin)
  - [Como criar um plugin](#como-criar-um-plugin)
  - [Criando um modelo](#criando-um-modelo)
  - [Criando um Controller](#criando-um-controller)
  - [Adicionando rota](#adicionando-rota)
  - [Adicionando um link no menu](#adicionando-um-link-no-menu)
  - [Internacionalização](#internacionalização)
  - [Criando uma View](#criando-uma-view)
  - [Assets](#assets)
  - [Permissões](#permissões)
  - [Módulos](#módulos)
  - [Hooks](#hooks)
    - [Hooks nas Views](#hooks-nas-views)
    - [Hooks nos Controllers](#hooks-nos-controllers)
    - [Fazendo um plugin ser configurável](#fazendo-um-plugin-ser-configurável)
    - [Sobrescrevendo um Modelo](#sobrescrevendo-um-modelo)
    - [Sobrescrevendo um Controller](#sobrescrevendo-um-controller)
    - [Sobrescrevendo uma View](#sobrescrevendo-um-modelo)
- [Futuro](#futuro)
- [Colaboradores](#colaboradores)
- [Alunos](#alunos)

--------------------------------------------------------------------------------

### O Curso
Neste curso veremos como desenvolver plugins para Redmine 2.6. Com foco na teoria e técnicas de programação da framework Rails e como se aplica ao Redmine.

<sub><sup>Não é um curso de Ruby on Rails</sup></sub>

### Ambiente de Desenvolvimento
#### IDES
##### Sublime
[![Sublime Logo](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/sublime-logo.png) ](http://www.sublimetext.com/)

O Sublime é um editor de texto poderoso, rápido e multiplataforma. Atualmente está na sua versão 3.0, custando 70 obamas.

##### Atom
[![Atom s2s2](https://cloud.githubusercontent.com/assets/72919/2874231/3af1db48-d3dd-11e3-98dc-6066f8bc766f.png) ](https://atom.io/)

Atom é sem dúvida a minha recomendação para quem quer seguir com desenvolvimento ruby. Um editor estável, open source e desenvolvido/mantido pelo próprio GitHub.

Poderia passar horas falando o porque eu gosto do atom, mas não é o foco do curso.

##### RubyMine
[![Ruby Mine $$](https://confluence.jetbrains.com/download/attachments/20238/RM_logo.gif?version=1&modificationDate=1373554086000)](https://www.jetbrains.com/ruby/)

Como escrito no próprio site da JetBrain, a IDE de ruby mais inteligente. Porque não uso? Primeiro, porque acredito que IDE traga mais distrações do que o benefícios, segundo porque demora mais tempo para iniciar que o Atom, terceiro porque custa 199 obamas no primeiro ano e 99 para renovar a licença. E não acho que esse valor se pague.

Mas se você é desenvolvedor Java e não consegue viver sem autocomplete, vale a pena testar os 30 dias de trial.

<sub><sup>PS: Não citei o eclipse porque era bem ruim quando usei para ruby(a uns 2 anos atrás) e nunca mais voltei, mas se você quiser testar, fique a vontade.</sup></sub>

#### Ruby
##### Introdução
![](http://iwanttolearnruby.com/images/ruby-style-guide.gif)

Ruby é uma linguagem de programação de 1995 onde <sub><sup>quase</sup></sub> tudo é um objeto. Uma linguagem moderna, possuindo tipos dinâmicos, lambda function e altamente influenciada por programação funcional.

Diferente do Java onde o tipo é explícito, em Ruby a tipagem é conhecida como Duck Typing, se um argumento tem todos os métodos que o método precisa para usá-lo, o método vai aceita-lo como argumento. O que não significa que a variável não tenha tipo, todo objeto tem o método .class, que retorna a classe que ele pertence.

Outra diferença com o Java é que as classes em Ruby são abertas, mas o que isso significa? Significa que após declarar uma classe, você pode abri-la novamente e altera-la. Continuou sem entender? Vamos para o Exemplo:

```ruby
class A
  def a
    print 'a'
  end
end

obj = A.new
obj.a
=> a

class A
  def b
    print 'b'
  end
end

obj = A.new
obj.a
=> a
obj.b
=> b
```

Depois de declarar a classe A pela segunda vez, quando iniciei um novo objeto dessa classe, ele passou a ter ambos os métodos. Mas o que ocorreria se eu tivesse declarado o mesmo método novamente?

```ruby
class A
  def a
    print 'novo a'
  end
end

obj = A.new
obj.a
=> novo a
obj.b
=> b
```

Se o mesmo método for declarado duas vezes, a última declaração passa a valer. Essa característica da linguagem, evita as milhões de classe Utils que criamos no Java e facilita a criação de plugins.

E como ruby é altamente influenciada por programação funcinal, toda função tem um retorno, não existe função void em Ruby.

##### Instalar
![](https://rvm.io/images/rvm-logo-all-happy.png)

Para instalar o Ruby no Linux **(Usem LINUX)**, vamos utilizar um gerenciador de versão do Ruby para conseguirmos ter mais de uma versão rodando na mesma máquina.

Os dois gerenciadores mais famosos são o rbenv e o rvm. Para esse curso vamos utilizar o rvm.

<sub><sup>Escolha baseada em gosto pessoal, se quiserem se aventurar no rbenv ele também é muito bom.</sup></sub>

```sh
$ apt-get update
$ apt-get install -y subversion git telnet
$ apt-get install apt-get install -y libmysqlclient-dev freetds-dev imagemagick libmagickcore-dev libmagickwand-dev libcurl4-openssl-dev apache2-threaded-dev libapr1-dev libaprutil1-dev curl

$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

$ \curl -sSL https://get.rvm.io | bash -s stable

$ rvm requirements
$ rvm install 1.9.3
$ rvm use 1.9.3 --default
```

O Redmine 2.6 usa o ruby 1.9.3, por isso instalamos ele.

<sub><sub>Enquanto escrevia esse curso, verifiquei que na vesão [2.6.6](http://www.redmine.org/issues/19652) o Redmine passou a suportar o Ruby 2.2, mas como ainda não tive tempo de testar, vou seguir com a 1.9.3</sub><sub>

#### Rails
##### Introdução
![](http://rubyonrails.org/images/rails.png)

O Rails é uma framework MVC baseado em dois princípios básicos que você deve **SEMPRE** seguir, Convenção sobre Configuração e o Dry(Don't repeat yourself).

Atualmente se encontra na versão 4.2.3, mas no redmine 2.6 utilizaremos a 3.2.

##### Instalar
![](https://rubygems.org/favicon.ico)

Podemos instalar o rails utilizando o RubyGem, uma gem seria o equivalente do jar no Java. O RubyGem é um instalador de _gems_ que já é instalado junto com o ruby pelo rvm, ele funciona parecido com o apt-get do ububtu.

```sh
$ sudo gem install rails -v 3.2.x
```

Vamos instalar a versão 3.2 do rails pois é a última compatível com o redmine 2.6

#### Redmine
##### Introdução
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

##### Instalar
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

### Fluxo de Dados
A primeira coisa para se entender como programar utilizando Ruby on Rails é entender o fluxo de dados.

Quando um request chega ele segue o fluxo:
- Tenta encaixar a url do request em algum partner cadastrado nos arquivos de rotas.
  - Podemos verificar todas as rotas cadastradas rodando o comando  "_rake routes_" no terminal
- O arquivo de rotas dirá para o Rails qual o controller ele deve chamar e qual action ele deve executar.
  - Uma action é o nome de um método de um controller
- Uma action pode redirectionar para outra action ou renderizar uma view.
  - Por padrão o Rails renderiza a view com o mesmo nome da action dentro da pasta com o mesmo nome do controller. **(Convenção sobre Configuração)**
- A view enxerga as variáveis de instâncias(@) do controller

Entender e praticar esse fluxo é de extrema importância para saber encontrar o que você deseja modificar no Redmine e saber em qual parte do código está dando erro. Com o tempo você perceberá que as coisas estão onde devem estar.

### Estrutura de Pastas
![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-pastas.png)

- app
  - Pasta onde ficam os arquivos da aplicação como models, controllers, views, etc
- config
  - Pasta onde ficam os arquivos de configuração de banco, ambiente, **rotas**, locales
- db
  - Pasta onde se encontra as migrações do banco
- doc
  - Pasta para guardar as documentações da aplicação
- extra
  - Pasta com exemplo de plugin
- files
  - Pasta onde o Redmine guarda os arquivos anexados
- lib
  - Pasta onde ficam as bibliotecas de código desenvolvida, como rake taskes, patches, etc
- plugins
  - Pasta onde ficam guardados os plugins desenvolvidos para o Redmine, é aqui onde faremos 90% do desenvolvimento
- public
  - Pasta onde ficam os arquivos estáticos servidos pelo webserver
- script
  - Contêm o script para inicialização do rails
- test
  - Pasta com os testes automatizados do Redmine

#### Subpastas importantes
##### App
![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-app.png)

Como mencionado a pasta app contêm os controllers, models e views, mas em qual subpasta eles ficam? Bom, vou deixar você, caro leitor, descobrir sozinho.

Descobriu? Ótimo, agora vamos olhar para as views. As views possuem também subpastas, cada uma delas com o mesmo nome do controller. Assim o rails sabe qual view renderizar quando uma action for chamada. Ele vai dentro da **pasta view > nome_controller > nome_action.{html, js, xml, etcc}.erb**.

Caso ele não encontre o arquivo correspondentes, ele vai buscar na pasta com o mesmo nome da superclasse do controller e assim sucessivamente. Caso ele por fim não encontre, a página 404 do public é renderizada.

##### Public
![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-public.png)

Na pasta public ficam as imagens, javascript e css(pasta stylesheets), nela também ficam os temas do redmine.

O Redmine aceita temas customizados, um tema consiste em um conjunto de css/javascripts e dentro da configuração do Redmine é possível escolher qual tema o usuário vai ver.

O Redmine carrega automaticamente todas as pastas que se encontram dentro da pasta theme e disponibiliza para o administrador escolher qual utilizar.

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/7daaf9dff1b3ed0c4f7178d7ec93f97343e3f5a3/imagens/themas.png)

O Redmine possui uma lista de themas feitos pela comunidade para você não partir do zero.

[Lista de temas da Comunidade](http://www.redmine.org/projects/redmine/wiki/Theme_List)

### Plugins
#### Quando devemos desenvolver um plugin?

Devemos desenvolver um plugin quando:

1. O redmine não faz o que queremos que ele faça **E**
2. Não existe um plugin Open Source que faça o que a gente gostaria que o Redmine fizesse.

Se existir um plugin open source que faça parecido, faça um fork do plugin e contribua com ele. Assim todos ganham =).

#### Como criar um plugin
O Redmine provê um generator para criação da estrutura padrão de um plugin

```sh
$ rails generate redmine_plugin <plugin_name>
```

Rodando por exemplo:

```sh
$ rails generate redmine_plugin polls
```

Ele irá criar dentro da pasta plugins:

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/pasta-plugins.png)

Reparem, ele criou uma estrutura muito parecida com a estrutura do próprio redmine, de diferente temos:

* assets
  * Nessa pasta ficarão as imagens, javascript e css do plugin. O Redmine ao iniciar irá pegar esses arquivos e colocar na pasta plugin_assets dentro da pasta public

Editando o init.rb veremos

```rb
  Redmine::Plugin.register :polls do
    name 'Polls plugin'
    author 'Author name'
    description 'This is a plugin for Redmine'
    version '0.0.1'
    url 'http://example.com/path/to/plugin'
    author_url 'http://example.com/about'
  end
```

O init.rb é o arquivo que o redmine chama ao carregar o plugin, iremos utilizar mais ele no futuro. Mas por enquanto podemos somente modificar as informações do plugin.

Essas informações irão aparecer da seguinte forma no redmine:

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/informacoes-plugins.png)

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
