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

### Como você organizaria os estilos SCSS/estilos da aplicação?

- Organizo os estilos SCSS em uma estrutura modular com pastas como ```base``` (para resets), ```components``` (para estilos de botões, cards), e ```utils``` (para variáveis e mixins). Uso um arquivo ```main.scss``` com ```@use``` para importar tudo. Apesar de o Angular encapsular bem CSS é interessante ter essa arquitetura dos estilos, para a manutenibilidade a medida que o projeto crescer.

``` bash
styles/
├── base/_reset.scss
├── components/_button.scss
├── utils/_variables.scss
├── main.scss
```

### Como você faria para refatorar um componente que é grande, inchado sem necessidade, de 2 mil linhas por exemplo, cheio de regras de negócio, que está fazendo muitas coisas?

- Para refatorar um componente de 2 mil linhas cheio de regras de negócio, eu começaria analisando o código e separando responsabilidades com comentários, identificaria que componentes filhos eu poderia criar, moveria partes de lógica para um service e escreveria testes unitários para garantir que nada quebrou, executando-os após cada mudança.
