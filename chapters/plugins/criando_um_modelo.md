# Criando um modelo

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