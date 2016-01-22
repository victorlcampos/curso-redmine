# Ruby

## Introdução

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

