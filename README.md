# angular-notes

Anotações sobre desenvolvimento utilizando Angular.

### Como você otimizaria o desempenho de um componente grande no Angular?

- O Angular é focado em componentização, não deveria ter um componente muito grande, não é interessante criar um componente com muitas responsabilidades, mas existem algumas estratégias para aplicar como utilizar On push e não usar métodos no template, quando você invoca uma função no template, o Angular vai ficar rodando ela toda hora.
   - A estratégia ```OnPush``` é uma otimização do ```Change Detection```. Quando você configura um componente com ```changeDetection: ChangeDetectionStrategy.OnPush```, o Angular só verifica mudanças nesse componente em duas        situações:
      - Quando as referências dos inputs do componente mudam (ou seja, o valor de um ```@Input``` é atualizado com uma nova referência, não apenas modificando o conteúdo de um objeto existente).
      - Quando um evento gerado pelo próprio componente ou seus filhos ocorre (como um clique ou input no componente).

``` bash
@Component({
  selector: 'app-meu-componente',
  template: `<p>{{ dados }}</p>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class MeuComponente {
  @Input() dados: string;
}
```
- Aqui, o Angular só atualiza a UI se o @Input dados receber uma nova referência ou se o componente disparar um evento interno.

### Como você organizaria os estilos SCSS da aplicação?

- 
