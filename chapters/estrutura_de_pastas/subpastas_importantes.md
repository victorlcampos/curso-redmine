# Subpastas importantes

## App

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-app.png)

Como mencionado a pasta app contêm os controllers, models e views, mas em qual subpasta eles ficam? Bom, vou deixar você, caro leitor, descobrir sozinho.

Descobriu? Ótimo, agora vamos olhar para as views. As views possuem também subpastas, cada uma delas com o mesmo nome do controller. Assim o rails sabe qual view renderizar quando uma action for chamada. Ele vai dentro da **pasta view > nome_controller > nome_action.{html, js, xml, etcc}.erb**.

Caso ele não encontre o arquivo correspondentes, ele vai buscar na pasta com o mesmo nome da superclasse do controller e assim sucessivamente. Caso ele por fim não encontre, a página 404 do public é renderizada.


## Public

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-public.png)

Na pasta public ficam as imagens, javascript e css(pasta stylesheets), nela também ficam os temas do redmine.

O Redmine aceita temas customizados, um tema consiste em um conjunto de css/javascripts e dentro da configuração do Redmine é possível escolher qual tema o usuário vai ver.

O Redmine carrega automaticamente todas as pastas que se encontram dentro da pasta theme e disponibiliza para o administrador escolher qual utilizar.

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/7daaf9dff1b3ed0c4f7178d7ec93f97343e3f5a3/imagens/themas.png)

O Redmine possui uma lista de themas feitos pela comunidade para você não partir do zero.
