## Visão geral sobre a plataforma Testify

### Introdução
A plataforma Testify é uma plataforma de ensino de testes de software baseada no livro Effective Software Testing, de Maurício Aniche. A plataforma é composta por um conjunto de exercícios, que são apresentados aos alunos de forma incremental, e um conjunto de ferramentas que auxiliam os alunos a resolver os exercícios. A plataforma foi criada em 2022 por Vinicius Pereira como parte de seu [projeto de conclusão](https://www.linux.ime.usp.br/~viniciusgp/) de curso de Ciência da Computação no IME-USP.

### Componentes da plataforma

A plataforma Testify é composta por 3 componentes principais:

- **Client**: O componente Client é responsável por gerenciar a interface do usuário.
- **Exercises**: O componente Exercises é responsável por gerenciar os exercícios da plataforma.
- **Runner**: O componente Runner é responsável por executar os testes dos exercícios.

#### Client

O componente *Client* é responsável por gerenciar a interface do usuário. Nele os exercícios são apresentados aos usuários, e os usuários podem interagir com os exercícios, como por exemplo, editar os arquivos do exercício e executar os testes para serem validados. O código do componente está disponível no repositório [testify-tcc-client](https://github.com/testify-tcc/client).

O componente *Client* foi feito em React, e utiliza o framework Chackra UI para a construção da interface do usuário. O Client se comunica por requisições HTTP tanto com o componente Exercises quanto com o componente Runner. 

Com o componente *Exercises*, o *Client* solicita todas as configurações de exercício pelo endpoint `/` no componente *Exercises*. É responsabilidade do *Client* renderizar os exercícios e o menu de acordo com as configurações recebidas. 

Com o componente *Runner*, o *Client* solicita a execução dos testes de um exercício pelo endpoint `/`. O *Client* envia os arquivos do exercício para o *Runner* no corpo da requisição POST, e espera como resultado o *output* da execução dos testes, e a informação se os testes foram executados com sucesso ou não. 

#### Exercises

O componente Exercises é responsável por gerenciar os exercícios da plataforma. Nele as definições exercícios dos são registrada, processadas, e servidas no formato JSON para serem consumidas pelo componente Client. O código do componente está disponível no repositório [testify-tcc-exercises](https://github.com/testify-tcc/exercises).

O componente foi feito em Python, e utiliza o framework FastAPI para a construção da API.

##### Armazenamento de exercícios
O armazenamento dos exercícios deve ser organizado dentro da pasta `definitions`. Abaixo está um exemplo real de como o exercício `basket` está organizado dentro da pasta `definitions`:

```
definitions
├── exercises
│   ├── basket
│   │   ├── definition.json
│   │   ├── descriptions
│   │   │   └── description.md
│   │   ├── files
│   │   │   ├── basket.test.ts
│   │   │   ├── basket.ts
│   │   │   └── utils.ts
│   │   └── solutions
│   │       └── solution.md
```

O arquivo `description.md` contém a descrição do exercício, e o arquivo `solution.md` contém a solução do exercício. O conteúdo destes dois arquivos em Markdown tem a sua formatação preservada quando o Client renderiza os exercícios.

Os arquivos contidos na pasta `files` são os arquivos que serão apresentados como editáveis pelo Client, e serão servidos para o componente Runner. O componente Runner utiliza estes arquivos para executar os testes dos exercícios.

O arquivo `definition.json` contém a configuração geral do exercício. A configuração agrega tanto a configuração de execução do exercício, quanto a configuração de apresentação do exercício.

Abaixo está um exemplo de como o arquivo `definition.json` do exercício `basket` está organizado:

```json
{
  "id": "basket",
  "panelLabel": "Basket",
  "panelPosition": 2,
  "testEnvironments": ["typescript-jest"],
  "fileSchemasMap": {
    "typescript-jest": [
      {
        "fileName": "basket.ts",
        "type": "CODE"
      },
      {
        "fileName": "utils.ts",
        "type": "CODE"
      },
      {
        "fileName": "basket.test.ts",
        "type": "TEST"
      }
    ]
  },
  "testCommandsMap": {
    "typescript-jest": "jest"
  },
  "description": "description.md",
  "solution": "solution.md"
}
```

##### Servindo exercícios

Como visto acima, a configuração de um exercício é armazenada em diversos arquivos, porém o Client recebe a configuração de um exercício em uma única resposta JSON. Para isso, o componente Exercises é responsável por ler os arquivos de configuração de um exercício, processá-los, e servir a configuração de forma unificada.

Um comando executado antes da inicialização do servidor gera o arquivo JSON, que é carregado e retornado sempre que o endpoint `/` é requisitado pelo Client. 

#### Runner

O Runner é responsável por executar os testes dos exercícios. Ele recebe os arquivos do exercício, qual `test environment` deve executar o teste, e então executa os testes dos arquivos de teste e retorna o *output* da execução. O código do componente está disponível no repositório [testify-tcc-runner](https://github.com/testify-tcc/runner).

O componente foi feito em Python, e utiliza o framework FastAPI para a construção da API, que possui somente um endpoint, `/`, que recebe os arquivos do exercício no corpo da requisição POST, e retorna o *output* da execução dos testes.

##### Execução de testes

A execução dos testes é feita utilizando o Docker. O componente Runner executa um container Docker com a imagem referente `test environment` enviado pelo Client. O container Docker é executado com o volume montado para a pasta `files` do exercício, e então o comando de execução dos testes é executado dentro do container Docker.

Os ambientes de testes estão definidos no repositório [testify-tcc/environments](https://github.com/testify-tcc/environments) e publicados no Docker Hub no namespace [vinigpereira](https://hub.docker.com/u/vinigpereira).

#### Segurança

A execução de código enviado pelo usuário é um desafio de segurança. Para mitigar este problema, o componente Runner executa os testes em um container Docker, que promove uma camada de isolamento entre o código enviado pelo usuário e o sistema operacional do servidor. Outros mecanismos de segurança são adicionais são utilizados, como por exemplo, a limitação de memória e CPU do container Docker.
