## Proposta de projeto MAC-6931

## Hussani Silva de Oliveira

# Avaliação de extensibilidade e escalabilidade da plataforma Testify

## Motivação

A [Testify](http://testify.surge.sh/) é uma plataforma voltada para o ensino de testes de software baseada no conteúdo do livro Effective Software Testing, de Maurício Aniche. A plataforma é composta por um conjunto de exercícios, que são apresentados aos alunos de forma incremental, e um conjunto de ferramentas que auxiliam os alunos a resolver os exercícios. A plataforma foi criada em 2022 por Vinicius Pereira como parte de seu [projeto de conclusão](https://www.linux.ime.usp.br/~viniciusgp/) de curso de Ciência da Computação no IME-USP.

A plataforma foi criada com a premissa de que outras pessoas possam contribuir com a adição de novos exercícios, e que os exercícios possam ser registrados e validados com diferentes linguagens de programação e frameworks de testes.

Para que a Testify cresça e possa ser adotada como uma ferramenta de referência para o ensino é necessário que ela seja de fácil extensão e que ela suporte um grande volume de acessos. A motivação deste projeto é avaliar a extensibilidade e escalabilidade da plataforma Testify, e identificar possíveis melhorias para que a plataforma possa ser utilizada em larga escala por estudantes de testes de software.

## Questões de Pesquisa
- A plataforma é escalável? Se não, como ela pode ser aparfeiçoada para ser escalável?
- A plataforma é extensível? Se não, quais são as limitações e como elas podem ser superadas?

## Metodologia
Por meio de dados quantitativos coletados utilizando ferramentas de análise de desempenho e performance, pretendo avaliar como a plataforma se comporta com diferentes volumes de usuários. Nesta análise será considerado o tempo de resposta do servidor, o tempo de resposta do cliente e carga suportada pelo servidor.

Para avaliar a extensibilidade da plataforma, pretendo analisar o código fonte dos componentes, e identificar os principais desafios para a adição de novos exercícios, novas linguagens de programação e frameworks de testes. Esta análise levará em consideração a arquitetura da plataforma, testes, acoplamento e modularização dos componentes.
Neste trabalho não será feita uma análise qualitativa com base na experiência dos usuários.

O acompanhamento do trabalho poderá ser feito através do repositório [hussani/testify-evaluation-project](https://github.com/hussani/testify-evaluation-project) no Github.

## Cronograma

### Entrega 1 (02/04)
- Visão geral do livro Effective Software Testing, de Maurício Aniche
- Visão geral da plataforma Testify e seus principais componentes

### Entrega 2 (16/04)
- Análise de código para avaliar a extensibilidade da plataforma
- Relatório parcial sobre a extensibilidade da plataforma e seus desafios

### Entrega 3 (30/04)
- Coleta de dados de desempenho geral da plataforma
- Testes de estresse para avaliar o desempenho do software em situações extremas

### Entrega 4 (14/05)
- Análise de desempenho isolada do componente cliente (frontend) da plataforma

### Entrega 5 (28/05)
-  Análise de desempenho isolada do componente servidor (backend) da plataforma

### Entrega 6 (11/06)
- Análise de desempenho isolada do componente executor de testes da plataforma

### Entrega 7 (25/06)
- Análise do impacto do crescimento do número de exercícios e do número de usuários no desempenho da plataforma

### Entrega 8 (02/07)
- Conclusão do projeto e redação do relatório final

## Resultados esperados
Espero com este trabalho certificar que a plataforma Testify pode ser utilizada em larga escala por estudantes de testes de software, e com desempenho aceitável. Espero também documentar as como a plataforma pode suportar novas linguagens de programação e frameworks de testes, identificar os principais desafios referentes à escalabilidade e manutenibilidade, e propor soluções para estes desafios em trabalhos futuros.
