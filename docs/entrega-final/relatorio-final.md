# Relatório Final

## Introdução

Este trabalho teve como finalidade entender as limitações de escalabilidade e desempenho da plataforma Testify.

Nesta análise, foi possível identificar que a plataforma possui um componente que é especificamente mais complexo em regras de negócio e possui menor desempenho. Este componente é Runner, que é responsável por executar os testes de unidade e integração do código submetido pelo usuário. Ele foi o objeto de estudo deste trabalho.

## Análise de desempenho

A plataforma executa um teste simples, utilizando Javascript em cerca de 4 segundos. Foi identificado que os testes utilizando Docker e Javascript são grandes consumidores de CPU, então a plataforma obtem maior desempenho quando utiliza uma máquina com mais CPUs.

Foi provado que em casos de múltiplas requisições similtâneas (usuários concorrentes), uma instância de servidor com 2 CPUs possue desempenho até 70% superior quando comparado com uma instância de servidor com 1 CPU.

Pode-se definir que a plataforma pode ser escalada verticalmente adicionando mais CPUs a configuração de hardware do servidor para se obter maior desempenho. No caso de uso testado, com Javascript, aumentar a quantidade de memória RAM não resultou em um aumento de desempenho. 512MB de memória RAM se provou ser o suficiente para executar as operações essenciais da plataforma.

Para atender a uma maior quantidade de usuários, a plataforma pode ser escalada horizontalmente, adicionando mais instâncias de servidores. 

## Alternativas potenciais não exploradas

### Utilização de um cluster Docker

Atualmente a aplicação utiliza uma estratégia similar ao _Docker-in-Docker_ (DinD) para executar os testes de unidade e integração. Esta estratégia consiste em executar um container Docker dentro de outro container Docker. Esta estratégia é considerada uma má prática, pois pode causar problemas de segurança e desempenho, pois o mesmo servidor HTTP também é responsável por executar os containeres Docker.

Uma alternativa viável, porém não explorada nas análises é a execução do programa utilizando um cluster Docker ou Kubernetes para executar os containeres. Desta forma é possível obter maior desempenho e segurança, pois os containeres são executados em um servidor dedicado para esta tarefa. A escalabilidade também pode ser melhorada, pois é possível adicionar mais servidores ao cluster para aumentar a capacidade de execução de testes, sem a necessidade de aumentar a quantidade de servidores HTTP.