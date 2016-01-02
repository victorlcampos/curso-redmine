# Internacionalização

A última coisa que falta para o menu, é que nossos clientes são Brasileiros e falam português, eles não querem polls e sim enquetes escrito no menu. Para fazer essa alteração, iremos criar um arquivo chamado pt-BR.yml dentro do config/locates do plugin e preencher com o seguinte conteúdo:

```yml
pt-BR:
  label_polls: Enquetes
```

Caso a opção caption não seja passada, **e ela não deve ser**, o redmine usa o _menu_name_ préfixado com _label_ para fazer a internacionalização do menu.

Para saber mais sobre internacionalização no rails: http://guides.rubyonrails.org/i18n.html
