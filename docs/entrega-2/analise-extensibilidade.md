## Análise de extensibilidade da plataforma

A extensibilidade de um software é a capacidade do mesmo receber novos componentes. É encontrada na normativa ISO/IEC-25010 a definição de extensibilidade como sendo "o esforço relativo para expandir a capacidade ou desempenho do software, aprimorando as funções atuais ou incluindo novas funções ou dados".

A extensibilidade é também uma das características da modificabilidade, um uma das sub-qualidades do aspecto de qualidade de manutenibilidade. A modificabilidade é definida como "a capacidade de modificar o software para corrigir defeitos, adaptar o software a novas necessidades ou melhorar o desempenho do software" (ISO/IEC-25010).

### Estudos sobre extensibilidade

Existem diversos estudos sobre extensibilidade de software, e como ela pode ser avaliada. A seguir destaco os estudos que foram mais relevantes nesta área.

#### EMSA
EMSA (acrônimo em inglês para Métrica de Extensibilidade para Arquitetura de Software). Definiu 4 fatores da extensibilidade:
- Compreensividade
- Projetabilidade
- Implementabilidade
- Adaptabilidade

Para cada fator a EMSA possui uma métrica relacionada. O conjunto de métricas obtidas individualmente são calculadas com um peso em uma nova fórmula, que resulta em uma pontuação final para a extensibilidade do software.

O modelo teórico proposto não está implementado publicamente em nenhuma biblioteca. Por este motivo, não será utilizado neste trabalho.

#### QMOOD

As métricas QMOOD (acrônimo em inglês para Modelo de Qualidade para Projetos de Orientação a Objeto) são um conjunto de métricas que avaliam a qualidade de um software orientado a objetos. A extensibilidade é avaliada através da métrica QMOOD-Extensibility, que é calculada a partir de 4 sub-métricas relacionadas a orientação a objetos: abstração, acoplamento, herança e polimorfismo.

Este modelo é debatido em diversos artigos. Porém não existe uma implementação de seu algoritmo em uma biblioteca que possa ser utilizada para avaliar a extensibilidade na plataforma Testify.

### Manutenibilidade

Pela dificuldade de avaliar automaticamente e isoladamente a extensibilidade de um software, utilizarei de maneira geral o conceito de manutenibilidade para avaliar o esforço de modificação e manutenção da plataforma.

Para esta análise, utilizarei a métrica de Índice de Manutenibilidade (MI, em inglês Maintainability Index). Este índice é calculado com base em 4 métricas: Complexidade Ciclomática, Comentários, volume de Halstead e número de linhas de código. A fórmula para o cálculo do MI é a seguinte:

$$ MI= 171-5.2^{\ast}\ln(avgV)-0.23^{\ast }avgV(g)-16.2^{\ast}\ln(avgLOC)+avgCM $$

onde, $avgV$, $avgV(g)$, $avgLOC$, e $avgCM$ são a média de esforço, extensão de V(G), média de linhas de código, e número de comentários por sub-módulo (função ou procedure) em um sistema de software. (Coleman, 1994)

O índice de manutenibilidade é um índice que varia de 0 a 100, sendo 100 o valor máximo. O índice de manutenibilidade é calculado para cada sub-módulo do software, e a média dos índices é calculada para obter o índice de manutenibilidade do sistema como um todo.

Para avaliar a manutenabilidade dos componentes Runner e Exercises, que são desenvolvidos em Python, utilizei a biblioteca [wily](https://wily.readthedocs.io/en/latest/).

Não efetuei a análise de manutenibilidade do componente Client, pois este concentra menos regras de negócio do que os demais componentes. Além disto, o componente Client é desenvolvido em JavaScript, e a biblioteca wily não possui suporte para esta linguagem.

#### Exercises

A partir do resultado obtido pelo wily, foi possível calcular o índice de manutenibilidade do componente Exercises. O MI total do projeto é ~92.39, um valor considerado de alta manutenibiliade.
O resultado completo foi o seguinte:

| File                                       | Maintainability Index |
|--------------------------------------------|-----------------------|
| services/exercise_definitions_service.py   | 53.8729               |
| services/section_definitions_service.py    | 56.5037               |
| models/exercise_definition.py              | 60.637                |
| models/section_definition.py               | 66.0123               |
| services                                   | 82.0753               |
| models                                     | 90.8312               |
|                                            | 92.2393               |
| utils/constants.py                         | 100                   |
| services/definition_file_system_service.py | 100                   |
| env.py                                     | 100                   |
| json_keys/section_definition_json_keys.py  | 100                   |
| models/processed_file_schema.py            | 100                   |
| models/definition_type.py                  | 100                   |
| models/file_schema_type.py                 | 100                   |
| models/file_schema.py                      | 100                   |
| services/definitions_json_service.py       | 100                   |
| models/processed_exercise_definition.py    | 100                   |
| services/definitions_service.py            | 100                   |
| json_keys/exercise_definition_json_keys.py | 100                   |
| main.py                                    | 100                   |
| bootstrap.py                               | 100                   |
| generate_definitions.py                    | 100                   |
| models/processed_section_definition.py     | 100                   |
| dependency_container.py                    | 100                   |
| json_keys                                  | 100                   |
| utils                                      | 100                   |
| Total                                      | 92.3912               |

Os arquivos com menor MI são os que possuem maior complexidade, e onde estão concentradas as regras de agregação dos exercícios. Os arquivos com o menor MI também são os com maior complexidade ciclomática, sendo `services/exercise_definitions_service.py` o arquivo com maior complexidade ciclomática, com valor 19, e `services/section_definitions_service.py` o segundo arquivo com maior complexidade ciclomática, com valor 11.

#### Runner

A partir do resultado obtido pelo wily, foi possível calcular o índice de manutenibilidade do componente Runner. O MI total do projeto é ~89.54, um valor considerado de alta manutenibiliade.
O resultado completo foi o seguinte:

| File                                                     |   Maintainability Index |
|----------------------------------------------------------|-------------------------|
| services/runner_service.py                               |                 56.0144 |
| services/docker_service.py                               |                 58.3996 |
| tests/test_javascript_jest_environment.py                |                 67.2867 |
| tests/test_typescript_jest_environment.py                |                 67.2867 |
| tests                                                    |                 67.2867 |
| builders/test_verifier_builder.py                        |                 67.606  |
| services/test_verifiers/javascript_jest_test_verifier.py |                 81.8563 |
| services/test_verifiers/typescript_jest_test_verifier.py |                 81.8563 |
| services/test_verifiers                                  |                 81.8563 |
| services                                                 |                 82.5895 |
| builders                                                 |                 83.803  |
|                                                          |                 89.5384 |
| main.py                                                  |                100      |
| models/run_request.py                                    |                100      |
| services/runner_file_service.py                          |                100      |
| utils/docker_container.py                                |                100      |
| bootstrap.py                                             |                100      |
| builders/docker_container_builder.py                     |                100      |
| dependency_container.py                                  |                100      |
| models/docker_image_name.py                              |                100      |
| env.py                                                   |                100      |
| interfaces/test_verifier_interface.py                    |                100      |
| client_test.py                                           |                100      |
| services/docker_image_map_service.py                     |                100      |
| services/docker_image_name_service.py                    |                100      |
| models/testing_environment.py                            |                100      |
| interfaces                                               |                100      |
| models                                                   |                100      |
| utils                                                    |                100      |
| Total                                                    |                 89.151 |

Assim como observado no componente Exercises, os arquivos com menor MI são os que concentram as regras mais complexas da aplicação. Os arquivos `services/runner_service.py` e `services/docker_service.py` são os que possuem o maior volume de código no componente.

#### Testabilidade

Na definição da ISO/IEC 25010, testabilidade é uma sub-qualidade de manutenibilidade, e é definida como o grau de eficiência e eficácia com que os testes podem ser executados para verificar a conformidade do sistema com os requisitos. A análise de Maintainability Index (MI) não avalia a implementação, ou cobertura de testes do sistema.

Analisando os três componentes que compõe a plataforma, o componente Runner possui a maior cobertura de testes. Não foi notada a presença de nenhuma biblioteca que auxiliasse a execução dos testes unitários, como `pytest`, ou outro similar.

O componente Client possui boa cobertura de testes dos componentes não visuais. Porém, a cobertura de testes dos componentes visuais é considerada baixa, já que apenas dois componentes foram testados.

Não foi evidenciada nenhuma estrutura de testes para o componente Exercises.

O componente Runner possui 3 testes de integração, que testam os dois ambientes de execução de testes disponíveis na plataforma. O teste de integração implementado é no formato caixa-preta, onde uma requisição HTTP é simulada e o resultado da execução é verificado.  Os componentes Client e Exercises não possuem testes de integração.

## Conclusão

Não foi possível analisar isoladamente o critério de extensibilidade do código da plataforma. Todavia, a análise de manutenibilidade apontou que o código da plataforma possui boa manutenibilidade. Por extensibilidade ser uma sub-qualidade de manutenibilidade, existe a hipótese de que o código da plataforma possua boa extensibilidade.

A plataforma possui baixa cobertura de testes, e este pode ser um impecílio para que mudanças sejam implementadas sem que as funcionalidades já existentes sejam comprometidas.

## Referencias bibliográficas

<div class="csl-entry"><a name="1"></a> Mari, M., &#38; Eila, N. (2003). The impact of maintainability on component-based software systems. <i>Conference Proceedings of the EUROMICRO</i>, 25–32. https://doi.org/10.1109/EURMIC.2003.1231563</div>

<div class="csl-entry">Coleman, D., Ash, D., Lowther, B., &#38; Oman, P. (1994). Using metrics to evaluate software system maintainability. <i>Computer</i>, <i>27</i>(8), 44–49. https://doi.org/10.1109/2.303623</div>

<div class="csl-entry">Wani, M. F., &#38; Gandhi, O. P. (1999). Development of maintainability index for mechanical systems. <i>Reliability Engineering and System Safety</i>, <i>65</i>(3), 259–270. https://doi.org/10.1016/S0951-8320(99)00004-6</div>

<div class="csl-entry">Alreffaee, T. R., Dabdawb, M. M. A., &#38; Taha, D. B. (2021). Measure extendibility/extensibility quality attribute using object oriented design metric. <i>TELKOMNIKA (Telecommunication Computing Electronics and Control)</i>, <i>19</i>(5), 1507. https://doi.org/10.12928/telkomnika.v19i5.19278</div>

<div class="csl-entry">Kim, J., Kang, S., Ahn, J., &#38; Lee, S. (2018). EMSA: Extensibility Metric for Software Architecture. <i>International Journal of Software Engineering and Knowledge Engineering</i>, <i>28</i>(03), 371–405. https://doi.org/10.1142/S0218194018500134</div>

<div class="csl-entry">Goyal, P. K., &#38; Joshi, G. (2014). QMOOD metric sets to assess quality of Java program. <i>Proceedings of the 2014 International Conference on Issues and Challenges in Intelligent Computing Techniques, ICICT 2014</i>, 520–533. https://doi.org/10.1109/ICICICT.2014.6781337</div>

<div class="csl-entry">Oliveira Junior, E. A., Maldonado, J. C., &#38; Gimenes, I. M. S. (2010). Empirical Validation of Complexity and Extensibility Metrics for Software Product Line Architectures. <i>2010 Fourth Brazilian Symposium on Software Components, Architectures and Reuse</i>, 31–40. https://doi.org/10.1109/SBCARS.2010.13</div>

<div class="csl-entry">Malhotra, R., &#38; Lata, K. (2020). A systematic literature review on empirical studies towards prediction of software maintainability. <i>Soft Computing</i>, <i>24</i>(21), 16655–16677. https://doi.org/10.1007/S00500-020-05005-4/TABLES/15</div>

<div class="csl-entry"><i>ISO/IEC 25010:2011(en), Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models</i>. (n.d.). Retrieved April 16, 2023, from https://www.iso.org/obp/ui/#iso:std:iso-iec:25010:ed-1:v1:en</div>