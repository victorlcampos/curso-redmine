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


#### Criando um Controller

#### Adicionando rota



#### Adicionando um link no menu


#### Internacionalização


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
