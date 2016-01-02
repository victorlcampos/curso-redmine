# Adicionando um link ao menu

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
