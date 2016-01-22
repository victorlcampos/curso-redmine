# Estrutura das Pastas

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

## Subpastas importantes

### App

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-app.png)

Como mencionado a pasta app contêm os controllers, models e views, mas em qual subpasta eles ficam? Bom, vou deixar você, caro leitor, descobrir sozinho.

Descobriu? Ótimo, agora vamos olhar para as views. As views possuem também subpastas, cada uma delas com o mesmo nome do controller. Assim o rails sabe qual view renderizar quando uma action for chamada. Ele vai dentro da **pasta view > nome_controller > nome_action.{html, js, xml, etcc}.erb**.

Caso ele não encontre o arquivo correspondentes, ele vai buscar na pasta com o mesmo nome da superclasse do controller e assim sucessivamente. Caso ele por fim não encontre, a página 404 do public é renderizada.


