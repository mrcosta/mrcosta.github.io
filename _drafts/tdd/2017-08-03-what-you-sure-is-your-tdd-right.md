Continuous Integration is a buzzword, Continuous Delivery also


reforçar TDD

Pq pra mim é fácil achar que 90% de cobertura é algo normal?!

quem usa code coverage?

code review e ferramentas de metrica ajuda a identificar casos bobos de quando não usa


efeitos da galera que não usa TDD: não cria um teste quando há um bug

solução:

você pensar como um cliente (uma classe que vai usar essa interface)

você exercita boundaries conditions. o que da insumo para discutir a estória e evitar bugs ou cenários não pegos durante o refinamento/elaboração da estória

reforçar TDD

* escrever stubs que não refletem a realidade
* não ver o teste falhando antes de ve-lo verde, trazendo falso positivo

Test size - Large tests are more likely to be flaky

* colocar metodo publico somente para testar a logica dele (metodo publico chamando privados quando deveria testar através do metodo publico)
