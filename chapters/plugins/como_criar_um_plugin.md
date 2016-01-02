# Como criar um plugin

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
