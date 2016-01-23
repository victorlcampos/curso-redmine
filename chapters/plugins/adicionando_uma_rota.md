# Adicionando uma rota

Como explicado no [Fluxo de Dados](#fluxo-de-dados), quando uma url é chamada, o Rails busca nos arquivos de rota para qual controller#action ele deve direcionar o chamado. Cada plugin do redmine possui seu próprio arquivo de rotas, que fica dentro de _config/_.

Podemos adicionar rotas de duas maneiras no Rails, a primeira no formato

```rb
  http_method url_partner, to: 'controller#action'
```

No nosso exemplo seria:

```rb
  get 'pools', to: 'polls#index'
```

Quando acessácemos 'http://localhost:3000/polls' a action index do controller polls seria executada. Mas isso aparenta ser pouco **convenção sobre configuação** correto? Não parece ser muito rails way de fazer as coisas.

Bom, o rails possui o método _resources(controller_name, opts)_ que define todas as rotas do rest por padrão, então podemos definir:

```rb
  resources :polls
```

O rails possui por padrão uma rake task que lista todas as rotas criadas, vamos roda-la e usar o grep para filtrar somente as rotas do Controller polls

```sh
$ rake routes | grep polls
polls GET          /polls(.:format) polls#index
      POST         /polls(.:format) polls#create
new_poll GET          /polls/new(.:format) polls#new
edit_poll GET          /polls/:id/edit(.:format) polls#edit
 poll GET          /polls/:id(.:format) polls#show
      PUT          /polls/:id(.:format) polls#update
      DELETE       /polls/:id(.:format) polls#destroy
```

Podemos reparar que todas as rotas necessárias para um serviço rest foram criadas. Se você parar para analisar um segundo, verá que a saída tem uma primeira coluna, o que será ela?

Para você não sair escrevendo urls de forma hardcode, o rails cria por padrão dois métodos para cada rota, uma com sufixo path e uma com sufixo url, no nosso caso temos a polls_path e polls_url que tem como retorno /polls e http://localhost:3000/polls respectivamente.

Com isso você não precisa modificar as urls em produção.

> Ok, achei super legal, mas eu somente queria uma rota para index e ele criou várias que eu nem preciso usar.

É verdade, por isso uma das opções que o método aceita é _only_. Podemos passar da seguinte maneira:

```rb
  resources :polls, only: [:index]
```

Sendo agora somente criado a rota para a action index.

As rotas no rails são **MUITO** poderosas e poderia escrever um artigo somente sobre elas, então aconselho a dar uma olhada na guia oficial da linguagem para conhecer essa ferramenta incrível: http://guides.rubyonrails.org/routing.html