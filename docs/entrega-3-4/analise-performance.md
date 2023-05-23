## Análise de Performance

### Introdução

Neste documento é apresentada a análise de performance da plataforma Testify. Antes de apresentar os resultados, introduzirei os conceitos de um teste de performance.

De acordo com a [ISO/IEC 25010](https://iso25000.com/index.php/en/iso-25000-standards/iso-25010), a performance de um software é uma característica de qualidade, que é relativa a quantidade de recursos utilizados por um software para realizar uma determinada função. Ela possui as seguintes subcaracterísticas: tempo de resposta, utilização de recursos e capacidade.

Um teste de performance é um tipo de teste de software que tem como objetivo avaliar a performance de um software. Teste de performance é um termo bastante abrangente, e possui diferentes estratégias e objetivos. Nesta etapa do trabalho, pretendo executar um teste de carga nos componentes backend da plataforma Testify, com o objetivo de avaliar a capacidade de suportar um grande volume de acessos simultâneos.

### Recursos utilizados

Os testes foram executados em um computador com a configuração abaixo:

| Componente        | Especificação                                     |
|-------------------|--------------------------------------------------|
| Processador       | Intel Core i7 de 6 núcleos de 9ª geração          |
| Velocidade do processador | 2,6GHz base, até 4,5GHz com Turbo Boost     |
| Memória RAM       | 16GB DDR4                                        |
| Armazenamento     | 512GB SSD  |
| Sistema operacional | macOS 12.6 (Monterey)                                      |

Os dois componentes testados foram o Runner e o Exercises. Os dois foram executados com Python 3.10.8.
A versão do Docker utilizada pelo Runner foi a 23.0.5. O Docker foi configurado para utilizar até 6 CPUs e até 2GB de memória RAM.

A análise de performance foi feita utilizando o [JMeter](https://jmeter.apache.org/), uma ferramenta de código aberto para análise de desempenho de aplicações web.

### Análise de desempenho do componente Runner utilizando o JMeter

Abaixo é apresentado o resultado da análise de desempenho do componente Runner utilizando o JMeter. O JMeter foi configurado para simular 30 usuários acessando a plataforma Testify ao mesmo tempo, com 10 repetições.

| Label       | #Samples | FAIL | Error % | Average (ms) | Min (ms)  | Max (ms)  | Median (ms)  | Transactions/s |
|--------------|----------|------|---------|----------|------|------|---------|----------------|
| Total        | 300      | 49   | 16.33%  | 21323.37 | 2567 | 26168| 21973.50| 1.32           |
| HTTP Request | 300      | 49   | 16.33%  | 21323.37 | 2567 | 26168| 21973.50| 1.32           |



Como apresentado na tabela acima, 16.33% das requisições falharam. As falhas apresentadas foram devido a um problema de concorrencia na criação de arquivos temporários.
Em detalhe, o problema ocorre quando dois enviam testes para um mesmo exercício. Cada teste enviado é salvo em um arquivo temporário, e os nomes desses arquivos são pré-definidos pela configuração do teste. Quando dois usuários enviam testes para um mesmo exercício, os nomes dos arquivos temporários são iguais, e o segundo usuário a enviar o teste sobrescreve o arquivo temporário do primeiro usuário. Para resolver este problema, é necessário que o nome dos arquivos temporários sejam gerados de forma aleatória, ou que o envio de testes seja feito de forma sequencial.

#### Ajustes e resultados obtidos

Para resolver o problema de concorrência na criação de arquivos temporários, foi feita uma alteração no código fonte do componente Runner. O nome dos arquivos temporários passou a ser gerado dentro de uma pasta com nome aleatório. Com esta alteração, o problema de concorrência foi resolvido, e o número de falhas foi reduzido para 0%.

Duas mudanças foram enviadas para o projeto original do componente Runner:
1. [Alteração no código fonte para gerar nomes de arquivos temporários de forma aleatória](https://github.com/testify-tcc/runner/pull/2)
2. [Correção nos testes unitários para garantir que a mudança não altere o comportamento do componente](https://github.com/testify-tcc/runner/pull/1)

As mudanças ainda aguardam aprovação do mantenedor do projeto.

A tabela abaixo apresenta os resultados obtidos após a correção do problema de concorrência na criação de arquivos temporários.

| Label         | #Samples | FAIL | Error % | Average (ms) | Min (ms) | Max (ms) | Median (ms) | Transactions/s |
|---------------|----------|------|---------|--------------|----------|----------|-------------|----------------|
| Total         | 300      | 0    | 0.00%   | 24917.62     | 15197    | 30460    | 25689.00    | 1.14           |
| HTTP Request  | 300      | 0    | 0.00%   | 24917.62     | 15197    | 30460    | 25689.00    | 1.14           |


Como é possível notar, a mudança no código fonte do componente Runner não alterou consideralvemente desempenho do componente. Uma pequena mudança no tempo médio das requisições foi observada, e possivelmente é relacionada com a carga adicional que o componente passou a processar dado que não havia falha de execução de testes.

### Análise de desempenho do componente Exercises utilizando o JMeter

Abaixo é apresentado o resultado da análise de desempenho do componente Exercises utilizando o JMeter. O JMeter foi configurado para simular 200 usuários acessando a plataforma Testify ao mesmo tempo, com 50 repetições, e um tempo limite de 120 segundos.

| Label         | #Samples | FAIL | Error % | Average (ms) | Min (ms) | Max (ms) | Median (ms) | Transactions/s |
|-------------- |---------:|-----:|--------:|-------------:|----:|----:|-------:|----------------:|
| Total         |     2500 |    0 |   0.00% |       183.67 |   8 | 398 |  193.00 |          29800.78 |
| HTTP Request  |     2500 |    0 |   0.00% |       183.67 |   8 | 398 |  193.00 |          29800.78 |

Como é possível notar, o componente Exercises não apresentou falhas de execução de testes. A carga de usuários acessando simultaneamente a aplicação é consideravelmente maior do que o componente Runner. Isto ocorre porque a requisição executada não demanda de recursos externos.

### Considerações finais

Após a análise inicial e correções pontuais, foi possível verificar que o componente agora pode ser utilizado em produção pode diversos usuários. O componente Runner, quando utilizado com carga de concorrência considerável apresenta lentidão que pode comprometer a experiência do usuário. O componente Exercises, por outro lado, apresentou bom desempenho mesmo com carga de concorrência alta. Os testes foram executados com recursos superdimensionados, e portanto, é possível que o desempenho do componente seja menor quando utilizado em produção.

Nas próximas entregas serão executados testes de resistência com cargas e recursos mais próximos da realidade. Estes testes serão executados em um ambiente virtualizado ou em um ambiente em núvem pública.