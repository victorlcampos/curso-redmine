# Fluxo de Dados

A primeira coisa para se entender como programar utilizando Ruby on Rails é entender o fluxo de dados.

Quando um request chega ele segue o fluxo:
- Tenta encaixar a url do request em algum partner cadastrado nos arquivos de rotas.
  - Podemos verificar todas as rotas cadastradas rodando o comando  "_rake routes_" no terminal
- O arquivo de rotas dirá para o Rails qual o controller ele deve chamar e qual action ele deve executar.
  - Uma action é o nome de um método de um controller
- Uma action pode redirectionar para outra action ou renderizar uma view.
  - Por padrão o Rails renderiza a view com o mesmo nome da action dentro da pasta com o mesmo nome do controller. **(Convenção sobre Configuração)**
- A view enxerga as variáveis de instâncias(@) do controller

Entender e praticar esse fluxo é de extrema importância para saber encontrar o que você deseja modificar no Redmine e saber em qual parte do código está dando erro. Com o tempo você perceberá que as coisas estão onde devem estar.