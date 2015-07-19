![](https://raw.githubusercontent.com/visagio/curso-redmine-univisagio/master/imagens/univisagio.png)

# Curso Redmine - Univisagio
## Ementa
- [O Curso](#o-curso)
- [Ambiente de Desenvolvimento](#ambiente-de-desenvolvimento)
  - [IDEs](#ides)
    - [Sublime](#sublime)
    - [Atom](#atom)
    - [RubyMine](#rubymine)

  - [Ruby](#ruby)
    - [Introdução](#ruby-introducao)
    - [Instalar](#ruby-instalar)

  - [Rails](#rails)
    - [Introdução](#rails-introducao)
    - [Instalar](#rails-instalar)

  - [Redmine](#redmine)
    - [Introdução](#redmine-introducao)
    - [Instalar](#redmine-instalar)

- [Fluxo de Dados](#fluxo-de-dados)
- [Estrutura de Pastas](#estrutura-de-pastas)
- [Estrutura Plugins](#estrutura-plugins)

--------------------------------------------------------------------------------

### O Curso
Neste curso teremos uma introdução do desenvolvimento de plugins para Redmine 2.6. Com foco na teoria e técnicas de programação da framework Rails e como se aplica ao Redmine.

<sub><sup>Não é um curso de Ruby on Rails</sup></sub>

### Ambiente de Desenvolvimento
#### IDES
##### Sublime
[![Sublime Logo](https://raw.githubusercontent.com/visagio/curso-redmine-univisagio/master/imagens/sublime-logo.png) ](http://www.sublimetext.com/)

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

Os dois gerênciadores mais famosos são o rbenv e o rvm. Para esse curso vamos utilizar o rvm.

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
##### Instalar
