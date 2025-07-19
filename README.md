# angular-notes

Anotações sobre desenvolvimento utilizando Angular.

### Como você otimizaria o desempenho de um componente grande no Angular?

- O Angular é focado em componentização, não deveria ter um componente muito grande, não é interessante criar um componente com muitas responsabilidades, mas existem algumas estratégias para aplicar como utilizar One push, Change detection e não usar métodos no template, quando você invoca uma função no template, o Angular vai ficar rodando ela toda hora.
