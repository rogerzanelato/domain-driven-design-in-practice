# Curso Domain-Driven-Design in Practice (DDD)

Repositório com o projeto prático feito no decorrer do curso e anotações pessoais referente ao assunto.

[Curso de estudo prático sobre o Domain Driven Design]( https://app.pluralsight.com/library/courses/domain-driven-design-in-practice)

[Professor Vladimir Khorikov](http://enterprisecraftsmanship.com/)

## Introduction
Lembre-se que o DDD não é uma bala de prata (que alias, não existem), DDD é aplicável apenas a uma gama de projetos específicos.

Todo Software possuí alguns atributos em comum:
- Quantidade de dados
- Performance
- Complexidade da Regra de Negócios
- Complexidade técnica (ex: sistemas que envolvem trabalhar diretamente com Hardwares embutidos)

A intenção do DDD é lidar APENAS com Softwares focados em **Regras de Negócio**. A aplicação do DDD não deverá ajudar em problemas relacionados à Big Data, Performance ou Sistemas Embutidos, sua aplicação principal, está no escopo de `Enterprise Systems` onde o maior problema à ser resolvido, é a complicada regra de negócios da empresa.

O objetivo do DDD é produzir sistemas fortemente orientado a negócios que sejam altamente estáveis e fáceis de manter.

**Atenção:**
- DDD foca em simplificar o problema, extraí-lo e deixá-lo CLARO no código.
- Queremos quebrar o problema em pedaços pequenos, para chegar num ponto onde ele não é mais difícil de entender ou implementar.
- Ele é um completo ao **YAGNI** (You are not gonna need it) e ao **KISS** (Keep it simple stupid).
- DDD deve ser a solução no seu projeto e não o problema!

### Main concepts of domain driven design
- `Ubiquitous Language`: É uma linguagem que deve ser comum entre o "Cliente" e o "Desenvolvedor". Seu código deve expressar as terminologias do do cliente, para que os desenvolvedores e o cliente falem a mesma lingua.
- `Bounded Context`: Criar delimitações específicas entre cada parte do sistema. Uma classe Produto não tem o mesmo sentido num contexto de "Suporte" do que num contexto de "Venda". Essa "Separação" de sentido entre contextos DEVE SER CLARA!
- `Core Domain`: Foca no principal problema à ser resolvido. Detalhes menores, podem ser delegados ou integrados com soluções de terceiros que já estão prontas. O importante é FOCAR SEMPRE NO NEGÓCIO.

### DDD Is Not Only About Writing Code
Os desenvolvedores devem estar em constante contato com o negócio e evoluírem constatamente no entendimento sobre o domínio.
Todos os desenvolvedores devem estar cientes do ponto de vista do Domain Expert e do que estão cumprindo.

### Application Architecture
Arquitetura DDD pode ser visualizada como uma Cebola (see Onion Architecture), é uma arquitetura composta por Camadas.

![](https://i.imgur.com/9kqcdqP.png)

A regra é que cada "Bloco" pode ter conhecimento dos outros blocos da mesma camada, e das camadas inferiores. **Mas nunca das SUPERIORES.**

Obs: O banco de dados estará sempre na camada `Repository`, se você vai utilizar uma ORM ou efetuar as query diretamente, não importa! Mas as interações com o banco ocorrerão por lá.

**E porque essa separação é importante?**
Por causa da separação de preocupações.

As regras de negócios estarão (se não toda, a maioria esmagadora) nos objetos centrais, em especial, as `Entity` e `Value Object`. Esses objetos não devem ter CONHECIMENTO NENHUM, de como vão ser persistidos, construídos ou mapeados no banco de dados. Tudo que eles devem saber é o **Domínio** que representam.

Lembre: Quanto mais limpo e claro você manter seu modelo, mais fácil será entendê-lo mais tarde.

A falta de separação de responsabilidades, é a principal razão que bases de códigos se tornam gigantes ~~e uma porcaria~~.

### Modeling Best Practices
Como melhores práticas, tente sempre começar o código pelo `Core Domain` e testes´ unitários. Deixe a camada de apresentação e de banco de dados **por último**.

Lembre: **DDD == Foco no domínio**.

#### Seja pragmático!

Ter um código com 100% Coverage em testes é algo lindo, mas custoso e muitas vezes não necessário.
Isso varia de projeto para projeto, mas na maioria das "Enterprise-level Application", o **valor** que o teste agrega ao projeto cresce junto com o esforço para criá-lo e mantê-lo, dessa forma, quando o valor agregado não justifica mais o esforço necessário, é a hora de parar de criar o Coverage.

![](https://i.imgur.com/gOPibwT.png)

Na prática, queremos ter nossa camada de negócios (primeira camada na arquitetura acima) com 100% Code Coverage. Quanto as camadas superiores, ao invés de focar nos testes unitário, o ideal é aplicarmos testes de Integração, para garantir que as peças estão se "encaixando".

### Introducing the Problem
Neste curso, será programado utilizando conceitos de DDD uma "Máquina de Lanches" e uma máquina "ATM" (Caixa eletrônico) capaz de vender produtos diferentes, com valores diferentes, diferentes formas de pagamento, capaz de armazenar uma "carteira" do cash sendo inserido, devolver troco e realizar transações.

![](https://i.imgur.com/GBL1zCs.png)


## Starting With de First Bounded Context

**Vocabulário:**
- **Problem Domain** == **Domain**: O propósito que o sistema está sendo criado, o que queremos resolver.
	- **Core Domain**: A principal parte do problema. A parte única que não há como terceirizarmos.
- **Business Logic** == **Business Rules** == **Domain Logic** == **Domain Knowledge** == **Domain Model**: Codificação/solução do domínio
- **Domain Classes**: Classes domínios irão se referir as camadas Repositories, Domain Services, Factories, Entities, Value Objects, Domain Events e Aggregates.

![](https://i.imgur.com/82rwFgv.png)

### Dicas
- Comece sempre com o `Core Domain`
- Não introduza vários `Bounded Contexts` logo de cara. Sim, isso mesmo! Não crie as delimitações logo de cara. Comece sempre com apenas um Bounded Context tentando encapsular toda a regra de negócios da aplicação. E porque? Embora separar a aplicação em contextos ajude a diminuir a complexidade do código, sua aplicação só é justificada quando a base de código já está suficientemente grande. Do contrário, vamos apenas introduzir mais complexidade desnecessária!
- Sempre revise o código e procure por "abstrações ocultas" , e se sentir que o código está "estranho", teste abordagens diferentes para ver se são mais adequadas para nosso problema.

### Types of Equality
Tipos de comparação:
- **Identifier Equality**: Possuem um ID ou algo semelhante
- **Reference Equality**: Variáveis referenciando o mesmo objeto
- **Structural Equality**: Todos os atributos são iguais

### Entities vs Value Objects

*"One picture is more than one thousand words".*

![](https://i.imgur.com/XHpuOUE.png)

Em resumo, uma `Entity` possuí uma identidade única, pense numa classe "Snack Machine", eu posso ter várias máquinas, mas elas não são a mesma coisa! Cada uma possuí produtos diferentes, quantidades diferentes, valores diferentes, etc, são únicas, não podemos simplesmente trocar uma pela outra! **E são MUTÁVEIS.**

`Value Objects` não possuem uma identidade, eles podem ser trocados! Pense numa classe "Money" por exemplo, ela não tem uma identidade própria, ela pode ser trocada por qualquer outra classe que tenha a mesma composição. **E são IMUTÁVEIS.**

- Value objects não podem viver por si só, eles precisam estar associados com uma Entidade.
- Não possuem tabela no banco de dados

### Diferenciando

Diferenciar uma Entity de um Value Object nem sempre é algo fácil, e não se preocupe, isso é normal. Mas caso em determinado momento perceba que sua Entity se parece mais com um Value Object, não existe em refatorar seu Domain Model.

**Dê preferência à Value Objects!**

Eles são leves, imutáveis. Tente colocar a maior parte de sua regra de negócio neles, e deixe as Entidades funcionar como Wrappers.

Um "macete" para identificá-los é compará-los com Integers. Seu Domain Model se parece com um Integer? Então ele é provavelmente um Value Object!

![](https://i.imgur.com/nOTus9x.png)

Nós nos importamos se o "5" de um método, é o mesmo "5" de outro? Não! Todos os integers da aplicação são o mesmo, indiferente de como foram instanciados. Então, são essencialmentem `Values Objects`.

## Extending the Bounded Context with Aggregates

`Aggregate` é um Design Pattern dentro do DDD que busca simplificar as responsabilidades da Entidade. Ele irá conter uma ou mais `Entity` e será responsável por efetuar as validações e aplicar regras.

Um `Aggregate` deve sempre possuir uma Entidade "Raíz", que representa o Domínio ao qual aquela regra pertence. Como bom senso, um Aggregate não deve ter internamente mais do que 3 entidades. Caso tenha, será um indício de que o `Domain Model` deve ser repensado, e quebrado em partes menores, ou dividido em mais `Aggregates`.

Mesmo que um `Aggregate` possua mais que uma Entidade, apenas sua entidade raíz pode ser acessada diretamente. Ou seja, se temos um `Aggregate` da Entidade `Snack Machine` que possuí `Slots`, não podemos acessar Slots diretamente. Para acessá-los, teríamos que acessá-lo através do `Snack Machine`.

![](https://i.imgur.com/Cxm2UDE.png)

### Aggregate ou Entity?

Embora a explicação acima não deixe claro, o Aggregate é um substituto da "simples" entidade. Ou seja, se temos supostamente a Entidade `Car`, não vamos ter algo como `CarAggregate` na mesma base.

Para definir se nosso Modelo é uma `Entidade` ou `Aggregate`, devemos pensar, "Essa entidade, faz sentido sozinha?". Caso a resposta seja **Não** então temos espaço para um `Aggregate`.

Lembre-se: DDD significa `Domain Driven Design` e não `Database Driven Design`. O `Domain Model` não deve representar à como os dados estão armazenados, devem representar nossa lógica de negócio.

Caso crie o `Aggregate`, evite deixar que suas entidades (fora a principal) sejam acessadas diretamente.

## Introducing Repositories

Repositório é um padrão que visa encapsular toda comunicação com o banco de dados. O foco desse padrão, é abstrair a lógica de capturação dos dados e retornar as entidades modelo como se elas já estivessem alí, prontas para serem capturadas.

O ideal é que exista no máximo um `Repository` por `Aggregate`.

## Introducing the Second Bounded Context

O `Bounded Context` não é demonstrado nas Layers da arquitetura, pois como o objetivo dele delimitar e separar as áreas de atuação do negócio, cada Bounded Context teria suas próprias Entities/Repository/Application Services etc.. (até mesmo UI)

### Sub Domains

Sub domínio é parecido com o `Bounded Context` no sentido de que é uma divisão do problema. Porém, enquanto o Bounded Context está relacionado ao Escopo da **Solução**, o Sub-Domínio está relacionado ao Escopo do **Problema**.

Idealmente, cada sub-domínio terá um, e apenas um, bounded context.

![](https://i.imgur.com/eQqa368.png)

Embora essa regra possa ser quebrada, não é recomendado. Ter vários `Bounded Context` atacando o mesmo problema pode terminar em muita confusão.

### Identificando

Como vimos acima, o bounded context possuí uma relação de 1-1 com o sub-domínio. Portanto, a forma mais fácil de identificar que você precisará de um segundo (ou terceiro, quarto...) Bounded Context, é identificar os sub-domínios.

E como identificá-lo?
**Fale com os experts das Regras de Negócio!**

Apenas eles conseguirão identificar e informatar como  a área de atuação está divida.

### Single Bounded Context?

Muitas vezes, após ter identificado os múltiplos sub-domínios, o problema poderá ter sido resolvido com pouco código e então será tentador deixar as soluções em apenas um `Bounded Context`.

Apesar disso, é recomendado que essa prática seja evitada.

Utilizar um único Bounded Context para atacar múltiplos sub-domínios irá causar problemas à longo prazo. Em pouco tempo terá uma base de código macarrão de difícil manutenção.

### Considerações

Antes de fazer a separação, é importante considerar o quão grande o projeto está se tornando e a equipe em geral.

Se a base de código estiver crescendo demasiada complexa, ou o contexto por si só for demasiado grande, poderá ser necessário alocar um time de desenvolvedores para lidar com cada Bounded Context. O que em muitos casos é o ideal.

Portanto, sempre reavalie o projeto e leve em consideração todo o contexto.

É melhor **uma equipe trabalhando em múltiplos Bounded Context**, do que **duas equipes trabalhando no mesmo Bounded Context**.

![](https://i.imgur.com/S9WntvP.png)

### Tipos de Isolação entre Bounded Context

**1. No mesmo Assembly:**
	- Maior dificuldade em Isolar corretamente
	- Projeto pode crescer demasiado grande
	- Mais fácil de se trabalhar e efetuar deploy

![](https://i.imgur.com/a0aoxZh.png)

**2. Em Assemblies separados:**
	- Projeto pode crescer demasiado grande
	- Mais fácil de se trabalhar e efetuar deploy
	
![](https://i.imgur.com/2MBipbJ.png)

**3. Em deploys separados** ( **MicroServiços** \o/ )
	- Um grupo de projeto para cada `Bounded Context`
	- Fácil de manter as regras de isolação
	- Possívelmente, maior sobrecarga de manutenção e maior dificuldade em deploys

A dica de ouro é: **Seja Pragmático!**

Comece com o tipo 1 e só vá para o próximo formato se for necessário.

**Implicações no formato de comunicação entre cada tipo:**
![](https://i.imgur.com/dvBaAjI.png)

### Finalizando

Para finalizar sobre os `Bounded Context`. Evite reutilizar o código entre eles. O ideal é que cada processo seja único e independente.

Caso hajam **Utility Classes**, que agreguem valor se compartilhada entre os times, prefira criar uma base separada externa ao projeto. Mas lembre-se dos riscos, se algo quebrar, todas equipe serão impactadas, portanto, faça isso apenas se o valor agregado realmente justifica o risco.

## Working With Domains Events

Ao separar a aplicação entre diferentes `Bounded Context` é normal ter contextos nos quais uma ação será efetuada, após determinada ação ocorrer dentro de outro domínio.

Para esses casos, é recomendado utilizar de `Domain Events` para efetuar a comunicação entre os contextos. Injetar a classe principal e utilizá-la é Má Prática, pois deixará ambos contextos fortemente acoplados.

Conforme foto acima, caso estejamos usando **MicroServiços**, teremos que efetuar as chamadas por Http ou disparar eventos por Messages Queue (como Rabbit, Kafka, SQS, etc..).

### Consideração ao Utilizar Domains Events

Ao criar uma estrutura de evento, não permita que o `Domínio` (primeira camada) dispare os eventos diretamente.

Lembre que essa camada é destinada a negócios e não possuí conhecimento das superiores, portanto, se disparar um evento diretamente a partir dela, além da questão da violação na separação de responsabilidades, não teremos garantia de que a ação foi definitivamente Comitada no repositório.

**Formato recomendado:** Enfilere os eventos na `Entidade` ou `AggregateRoot`, mas deixe para o `Repositório` disparar o Evento ao concluir suas alterações com sucesso.

## Looking Forward to Further Enhancements

### Factories

`Factory` não é um pattern diretamente ligado ao DDD, mas é extramemente útil para retirar a reponsabilidade de inicialização da própria classe e deve ser usado quando necessário.

### Validação

Em alguns casos, criamos métodos de validação e deixamos para que o `Repositório` o chame antes de persistir a `Entidade`. Embora isso possa ser aplicável, quando o novo desenvolvedor for escrever a persistência, deve SEMPRE lembrar de chamar a validação.

Recomendado: Aplicar a validação diretamente na classe, para garantir a integridade dos dados.

### Domain Services vs Application Services

**Domain Service:**
- Fica dentro da camada de domínio
- Possuí regra de negócio
- Não se comunica com o mundo Exterior

**Application Service:**
- Fica fora da camada de domínio
- Não possuí regra de negócio
- Se comunica com o mundo exterior

### Anemic Domains Model Anti-Pattern

Evite criar `Model Domains` anêmicos.

[AnemicDomainModel - Martin Fowler](https://martinfowler.com/bliki/AnemicDomainModel.html)

### Fat Entitites Anti-Pattern

Ao contrário de entidades anêmicas, `Fat Entities` são entidades que possuem lógica demais ou responsabilidades não naturais. Reavalie sempre se a lógica que está aplicando está sendo feita no local adequado.

Caso ela esteja muito extensa, pode ser um sinal de que deve quebrá-la em partes menores.

Lembre-se: Seu Domain Model não é um espelho do Banco de Dados.

### Repository Anti-Patterns

**Não retorne entidades parcialmente carregadas!**

Embora essa prática diminua o uso de memória e tenha ganho de performance, ao retornar entidades que não estão corretamente populadas, estaremos retornando entidades que não são válidas no contexto do negócio.

Para esses casos, utilize uma classe **DTO** `Data Transfer Object` e retorne apenas os dados que precisa.

### Machanical Approach to DDD

Domain Driven Design não é um pattern que te dá um modelo pronto. Cada negócio é especifico e portanto cada modelagem também é única.

Se o DDD não possuí modelos pronto, então não faz sentido utilizar ferramentas de geração de código. Cada modelagem deve ser feita com pensamento e na mão, principalmente para que você aprenda o Domínio.

![](https://i.imgur.com/BHZUv68.png)

## Lista de Recursos Adicionais

![](https://i.imgur.com/wDdnMeg.png)

## Extras

### Onde coloco minha validação?

Essa pergunta deu origem a diferente escolas de pensamento dentro do escopoe de DDD.
[Validation in Domain-Driven Design (DDD)
](http://gorodinski.com/blog/2012/05/19/validation-in-domain-driven-design-ddd/)

Mas basicamente, o ideal (segundo muitos) é que sua `Domain Layer` faça as validações das entidades dentro delas mesmo, sem utilização de frameworks globais. Dessa forma, suas entidades terão por todo o sistema um estado de "Sempre válido".

Após isso, Frameworks de Validação podem ser utilizadas na `Application Layer`, para ter mensagens de erro mais adequada, mesmo que isso cause de certa forma duplicação nas validação.

Lembre também que algumas validação de negócio, podem ser feita apenas por `Domain Classes`. Por exemplo, "Cliente A pode receber X limite de crédito?", "Usuário B já existe no banco de dados?".