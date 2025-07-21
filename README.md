# angular-notes

Anotações sobre desenvolvimento utilizando Angular.

### Qual a diferença entre Angular e Angular Js?

- AngularJS é a versão antiga, usa JavaScript e controladores. Angular é moderno, usa TypeScript, componentes e é mais rápido e organizado.

### O que são módulos e services?

- Módulo é uma caixa que organiza componentes e serviços, define o que a aplicação precisa para uma funcionalidade específica, por exemplo um módulo para gerenciar tarefas. Service é um gerente que cuida de dados e lógica, compartilhando-os com os componentes, exemplo: O TaskService busca tarefas de uma API REST e as fornece aos componentes, garantindo que todos usem a mesma lista.
  
### O que são diretivas?

- Diretivas são instruções que adicionam comportamentos ou modificam o HTML para tornar a interface mais dinâmica. Elas controlam como os elementos aparecem ou funcionam na tela.
   - Permitem mostrar, esconder ou repetir elementos (ex.: mostrar uma lista de tarefas).
   - Podem mudar a aparência de elementos (ex.: adicionar uma cor a uma tarefa).
   - Permitem criar comportamentos personalizados (ex.: destacar uma tarefa ao clicar).

### Quais são os tipos de diretiva no Angular?

- Estruturais: Modificam a estrutura do HTML, adicionando ou removendo elementos.
   - Exemplo: *ngFor repete elementos, como listar tarefas. *ngIf mostra ou esconde algo, como uma mensagem "Sem tarefas".
- De Atributo: Alteram a aparência ou comportamento de um elemento existente.
   - Exemplo: ngClass muda a cor de uma tarefa se estiver concluída. ngModel sincroniza um formulário com os dados.
- Componentes: São diretivas com um template (interface) e lógica própria, como um pedaço reutilizável da aplicação.
   - Exemplo: Um TaskListComponent exibe a lista de tarefas com sua própria lógica e visual.
 
### O que são Pipes?

- È uma funcionalidade esepcífica para transformação de dados, é uma má prática utilziar métodos no template, logo, o correto é você utilizar pipes.

### O que são Pipes puros e Pipes impuros?

- Pipes Puros:
   - Só são executados quando os dados de entrada mudam (imutabilidade). Isso os torna mais rápidos, pois evitam cálculos desnecessários, melhorando a performance.
      - Exemplo: O pipe date formata uma data (ex.: {{ task.date | date:'dd/MM/yyyy' }}). Só recalcula se a data mudar.
    
- Pipes Impuros:
   - São executados toda vez que o Angular verifica mudanças, mesmo que os dados não mudem. São úteis para dados que mudam com frequência, mas podem ser mais lentos, impactando na performance.
      - Exemplo: Um pipe personalizado que filtra tarefas em tempo real (ex.: {{ tasks | filter:'concluídas' }}) precisa ser impuro para atualizar a lista sempre que o filtro muda.
    
### O que é Pipe async?

- O pipe async mostra dados assíncronos (como Observables) no HTML e gerencia a inscrição automaticamente.

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

### Qual a diferença entre Promise, Observable e Signals?

- A promise tem um ciclo de vida, um resultado, acaba depois de entregar. Exemplo: "Quero os dados de um usuário." A Promise te entrega os dados e termina.
- Observable é muito usado no Angular, usado para coisas complexas, como vários resultados, um fluxo contínuo. Exemplo: "Quero a lista de tarefas sempre que ela mudar." O Observable continua enviando as novas listas.
- Signals é novo no Angular, para mudanças simples, mais fácil de usar, usado para gerenciar o estado da interface. Exemplo: atualizar a tela quando uma tarefa é marcada como concluída, se a tarefa mudar de 'pendente' para 'concluída', a tela atualiza sozinha.

### Qual a diferença de swicthMap e mergeMap?

- SwitchMap cancela a operação anterior e usa apenas a mais recente quando um novo valor chega, mergeMap
executa todas as operações ao mesmo tempo, combinando os resultados, sem cancelar as anteriores.
  - Exemplo: Se o usuário digita "tarefa1", "tarefa2", "tarefa3" rápido, switchMap busca apenas "tarefa3" na API, cancelando as anteriores. mergeMap busca todas, retornando resultados para "tarefa1", "tarefa2" e "tarefa3".

### Como você faria gerenciamento de estado?

- Uso um service com Observables, ou service com Signals. O service guarda uma tarefa e uso Observables ou Signals para avisar a tela quando algo muda.
   - Observables: (do RxJS) para avisar os componentes quando algo muda, como ao adicionar uma tarefa.
   - Signals: para mudanças simples, como marcar uma tarefa como concluída, atualizando a tela automaticamente.
 
### Como você lida com erros de maneira global?

- Uso ErrorHandler para erros gerais, HttpInterceptor para erros de API, e try-catch para erros locais.
  - ErrorHandler do Angular para capturar erros gerais da aplicação, como falhas inesperadas no código e mostro uma mensagem amigável ao usuário, como "Algo deu errado, tente novamente".
  - Para erros em chamadas HTTP, uso um HttpInterceptor, permitindo tratar erros de forma centralizada, como exibir um alerta de "Servidor indisponível".
  - Localmente em componentes ou serviços, trato erros específicos com blocos try-catch ou assinando erros de Observables, verifico se houve erro e mostro uma mensagem como "Nenhuma tarefa encontrada".

### Como você organizaria uma aplicação com vários módulos, várias funcionalidades? Como você organizaria essa arquitetura?

- Dividiria em workspaces, libs talvez, utilizaria Nx. Criaria módulos separados para cada grupo de funcionalidades. Usaria Nx para organizar os módulos, separar os componentes e os serviços.

### Você saber o que é micro front-end? Quais cenários você utilizaria micro front-end?

- Eles são bons no cenário que você tem uma aplicação web legada e quer construir uma nova, sem abrir mão da antiga.
