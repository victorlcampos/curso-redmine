# App

![](https://raw.githubusercontent.com/victorlcampos/curso-redmine/master/imagens/estrutura-app.png)

Como mencionado a pasta app contêm os controllers, models e views, mas em qual subpasta eles ficam? Bom, vou deixar você, caro leitor, descobrir sozinho.

Descobriu? Ótimo, agora vamos olhar para as views. As views possuem também subpastas, cada uma delas com o mesmo nome do controller. Assim o rails sabe qual view renderizar quando uma action for chamada. Ele vai dentro da **pasta view > nome_controller > nome_action.{html, js, xml, etcc}.erb**.

Caso ele não encontre o arquivo correspondentes, ele vai buscar na pasta com o mesmo nome da superclasse do controller e assim sucessivamente. Caso ele por fim não encontre, a página 404 do public é renderizada.
