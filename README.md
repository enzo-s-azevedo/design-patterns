# Padrões de Projeto

## Criacional

### Builder

- Objetivo do padrão: tem como objetivo através de um processo gradual construir objetos complexos e permitir que um mesmo objetivo tenha variantes tendo o mesmo processo de construção.
- Qual problema resolve: evita que seja criado um número grande e desnecessário de subclasses ou que se crie um construtor que aborde todas as possibilidades, dessa maneira, na maioria dos casos se tem inúmeras chamadas inúteis.
- Solução: Para isso, são criados diversos objetos, chamados de “builder”, que são responsáveis por construir, assim, quando necessita-se construir um objeto, apenas são chamadas as etapas do objeto “builder” que são necessárias de acordo com o interesse. Além disso, também pode haver uma classe chamada de “diretor”, a fim de estabelecer a sequência em que as tarefas dos “builders” serão concretizadas.
- Diagrama UML:

![UML Builder](https://github.com/enzo-s-azevedo/solid-principles/assets/142538641/c1eff34b-2147-41af-9cb9-dec2e3df1423)

`Imagem extraída de: https://refactoring.guru/pt-br/design-patterns/builder`



## Estrutural

### Proxy

- Objetivo do padrão: proporciona um espaço reservado ou um substituto para outro objeto para moderar o acesso a esse objeto.
- Qual problema resolve: evita o alto consumo desnecessário de recursos do sistema e códigos duplicados.
- Solução: cria-se um objeto proxy que imita o objeto real e é responsável por interagir com o cliente. Assim, o objeto proxy atua como um intermediário realizando tarefas adicionais, como o controle ao acesso, e realizando as operações que seriam delegadas ao objeto real.
- Diagrama UML:

![UML Proxy](https://github.com/enzo-s-azevedo/solid-principles/assets/142538641/a7428143-e448-473f-8c80-259dff95732e)

`Imagem extraída de: https://refactoring.guru/pt-br/design-patterns/proxy`

## Comportamental

### Mediator

- Objetivo do padrão: tem como objetivo usar um objeto mediador para evitar comunicações diretas entre os objetos, assim as dependências entre eles são reduzidas.
- Qual problema resolve: impede que haja um código muito complexo, evita um alto acoplamento desnecessário entre os objetos e promove a extensibilidade do código.
- Solução: os objetos ficam dependentes apenas do objeto mediador, o qual é responsável por redirecionar as chamadas para os objetos apropriados. Assim, a dependência entre os objetos é reduzida e o código torna-se mais extensível e menos complexo. 
- Diagrama UML:

![UML Mediator](https://github.com/enzo-s-azevedo/solid-principles/assets/142538641/ca8f725a-a031-411f-9bdf-12590960e390)

`Imagem extraída de: https://refactoring.guru/pt-br/design-patterns/mediator`


##Referências: https://refactoring.guru/pt-br/design-patterns/

