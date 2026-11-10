# Projeto Prático: Loja de Meias com Vue.js (Componentização)
---

##  Como Executar o Projeto
1. Clone este repositório ou baixe os arquivos em sua máquina.
2. Abra a pasta do projeto no VS Code.
3. Inicie o projeto utilizando a extensão Live Server.
---

## Respostas às Perguntas Teóricas

### 1. O que retornam `Vue.createApp` e `app.mount` e onde cada um é utilizado?
- `Vue.createApp`: Retorna uma instância da aplicação Vue, como um obejto do app. Ele é utilizado no arquivo main.js para inicializar a aplicação passando o objeto de configuração (com data, methods, etc.).
- `app.mount`: Retorna a instância do componente raiz montada no elemento DOM especificado. Ele é utilizado no final do arquivo index.html (ex: app.mount('#app')), basicamente ele é utilizado para atualizar o DOM da página sem recarrega-lá.

### 2. Quais dados saíram da raiz e foram para `product-display`? Por que `cart` permaneceu no pai?
- Apenas a propriedade premium (via props). Todos os dados específicos do produto (como nome, marca, variantes e detalhes) ficaram encapsulados dentro do próprio componente do produto.
- O carrinho (cart) ficou no componente raiz (pai) porque ele atua como o controle central da aplicação. Isso centraliza as informações compartilhadas, permitindo que o estado do carrinho seja gerenciado e fique disponível para a aplicação como um todo.

### 3. Como `selectedVariant` determina a imagem e a disponibilidade, sem atribuir valores diretamente a `image` e `inStock`?
Através do uso de propriedades computadas (`computed`). As funções `image` e `inStock` monitoram reativamente o índice armazenado em `selectedVariant`. Quando o usuário interage com as opções (passando o mouse por cima de um círculo de cor), `selectedVariant` muda de índice, fazendo com que as propriedades computadas recalculem e retornem automaticamente a imagem e a quantidade em estoque correspondentes à variante ativa no array `variants`.

### 4. Como `premium` chega ao produto e como seu valor altera o resultado de shipping?
- Como chega: O dado `premium` (booleano) é definido na raiz e passado para o componente filho através de uma **prop** (`:premium="premium"` na tag `<product-display>`). O componente declara essa prop na opção `props`.
- Como altera o `shipping`: O componente possui uma propriedade computada chamada `shipping`. Ela avalia a condição `this.premium`: se for `true`, retorna a string `'Free'`; se for `false`, retorna o valor numérico `2.99`.

### 5. Que alteração comprova a independência entre duas instâncias? Quais foram os resultados dos testes de cores e frete?
- Alteração: A duplicação temporária da tag do componente no `index.html` (uma recebendo `:premium="true"` e outra `:premium="false"`).
- Resultados: Ficou comprovado que cada instância do componente gerencia seu próprio estado local (`selectedVariant`). Ao passar o mouse ou selecionar variantes diferentes em um componente, o outro não sofreu alterações nas imagens ou no estoque. Além disso, cada um exibiu corretamente o valor de frete correspondente à sua respectiva prop (`Free` ou `2.99`).

### 6. Por que `this.cart += 1` no filho não atualiza o contador do pai? Qual comportamento provisório permitiu verificar a componentização antes da correção do botão?
- Por que não atualiza: Devido ao fluxo de dados unidirecional do Vue. Os componentes filhos não devem modificar diretamente o estado do componente pai (propriedades ou dados do pai). Alterar o carrinho diretamente no filho violaria essa regra de arquitetura e isolamento de escopo.
- Comportamento provisório: Antes da emissão de eventos (`$emit`), utilizou-se um `console.log` no método `addToCart` do filho para registrar o ID da variante selecionada no console do navegador, permitindo validar o clique e a lógica interna da variante antes de implementar a comunicação correta com a raiz.