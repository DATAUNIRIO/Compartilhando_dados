# Como compartilhar dados com um estatístico. Um resumo das boas práticas

Essa é uma tradução livre [deste site](https://github.com/jtleek/datasharing) para as atividades do Grupo de Apoio Estatístico - GAE

Este é um guia para quem precisa compartilhar dados com um estatístico ou cientista de dados. Os públicos-alvo que tenho em mente são:
1. Colaboradores que precisam de estatísticos ou cientistas de dados para analisar dados para eles
2. Alunos de várias disciplinas à procura de apoio estatístico 
3. Alunos cujo trabalho é agrupar / limpar / lidar com conjuntos de dados

Os objetivos são fornecer algumas instruções sobre a melhor maneira de compartilhar dados para evitar as armadilhas mais comuns na transição da coleta de dados para a análise de dados.  O [Grupo de Apoio Estatístico – GAE]( http://gae.uniriotec.br/) trabalha com um grande número de colaboradores e a variação na velocidade dos resultados é em função da qualidade dos dados quando eles chegam ao GAE. Com base em minhas conversas com os estatísticos do DMQ, isso é uma verdade universal.

Meu sentimento é que os estatísticos devem ser capazes de lidar com os dados em qualquer estado que eles cheguem. É importante ver os dados brutos, entender as etapas do processamento e incorporar fontes ocultas de variabilidade na análise de dados. Por outro lado, para muitos tipos de dados, as etapas de processamento são bem documentadas e padronizadas. Assim, o trabalho de converter os dados da forma bruta para a forma diretamente analisável pode ser executadoaantes de participar de uma reunião com o GAE. Isso pode acelerar drasticamente o tempo de retorno, já que o estatístico não precisa trabalhar primeiro em todas as etapas de pré-processamento.

## O que você deve entregar ao estatístico

Para facilitar a análise mais eficiente e oportuna, esta é a informação que você deve passar para um estatístico:

    1. Os dados brutos.
    2. Um conjunto de dados arrumado.
    3. Um dicionário de dados (livro de códigos) descrevendo cada variável e seus valores no conjunto de dados arrumado.
    4. Como você passou da etapa 1 para as etapas 2 e 3.
Vamos ver cada parte..

### Os dados brutos

É fundamental que você inclua o formato mais bruto dos dados aos quais você tem acesso. Isso garante que a proveniência dos dados possa ser mantida em todo o fluxo de trabalho. Aqui estão alguns exemplos da forma bruta de dados:

  *  O arquivo binário estranho  que a sua máquina de mensuração cospe.
  *  O arquivo do Excel não formatado com 10 planilhas com as quais a empresa contratada o enviou.
  *  Os complicados dados JSON que você obteve da API do Twitter.
  *  Os números escritos à mão que você coletou olhando através de um microscópio.

Você sabe que os dados brutos estão no formato certo se você:

   * Não executou nenhum software nos dados
   * Não modificou nenhum dos valores de dados
   * Você não removeu nenhum dado do conjunto de dados
   * Você não resumiu os dados de maneira alguma

**Se você fez alguma modificação nos dados brutos, não é a forma bruta dos dados.** Relatar dados modificados como dados brutos é uma maneira muito comum de desacelerar o processo de análise, já que o analista geralmente terá que fazer um estudo forense de seus dados para descobrir por que os dados brutos parecem estranhos. (*Imagine também o que aconteceria se novos dados chegassem?*)

### O conjunto de dados arrumado

Os princípios gerais de dados organizados são apresentados por Hadley Wickham neste artigo e neste vídeo. Enquanto o papel e o vídeo descrevem dados arrumados usando R, os princípios são mais geralmente aplicáveis:

   *  Cada variável que você mede deve estar em uma coluna
    * Cada observação diferente dessa variável deve estar em uma linha diferente
    * Deve haver uma tabela para cada "tipo" de variável
    * Se você tiver várias tabelas, elas devem incluir uma coluna na tabela que permita que elas sejam unidas ou mescladas

Embora essas regras sejam rígidas e rápidas, há várias outras coisas que facilitarão o manuseio do seu conjunto de dados. A primeira é incluir uma linha na parte superior de cada tabela / planilha de dados que contenha nomes de linha completos. Portanto, se você medisse a idade no momento do diagnóstico para os pacientes, deveria encabeçar essa coluna com o nome *IdadeDiagnostico* em vez de algo como *IdDia* ou outra abreviação que pode ser difícil para outra pessoa entender.

Aqui está um exemplo de como isso funcionaria a partir da genômica. Suponha que para 30 pessoas você tenha coletado medições de expressão gênica com sequenciamento de RNA. Você também coletou informações demográficas e clínicas sobre os pacientes, incluindo sua idade, tratamento e diagnóstico. Você teria uma tabela / planilha contendo as informações clínicas / demográficas. Ele teria quatro colunas (identificação do paciente, idade, tratamento, diagnóstico) e 31 linhas (uma linha com nomes de variáveis e, em seguida, uma linha para cada paciente). Você também teria uma planilha para os dados genômicos resumidos. Geralmente, esse tipo de dado é resumido no nível do número de contagens. Suponha que você tenha 100.000, então você teria uma tabela / planilha com 31 linhas (uma linha para nomes de genes e uma linha para cada paciente) e 100.001 colunas (uma linha para IDs de pacientes e uma linha para cada tipo de dados).

Se você estiver compartilhando seus dados com o colaborador no Excel, os dados arrumados deverão estar em um arquivo do Excel por tabela. Eles não devem ter várias planilhas, nenhuma macro deve ser aplicada aos dados e nenhuma coluna / célula deve ser destacada. Por favor,** NÃO USE CELULA MESCLADA**. Como alternativa, compartilhe os dados em um arquivo de texto delimitado por CSV ou TAB. (Cuidado, no entanto, que a leitura de arquivos CSV no Excel às vezes pode levar a manipulação não reprodutível de variáveis de data e hora).

### O Dicionário de Dados (livro de códigos)

Para quase todos os conjuntos de dados, as mensurações calculadas precisarão ser descritas com mais detalhes do que você pode ou deve entrar na planilha. O Dicionário de Dados contém esta informação. No mínimo, deve conter:

  *  Informações sobre as variáveis (incluindo as  unidades!) No conjunto de dados não contido nos dados arrumados.
  *  Informações sobre as escolhas que você fez.
  *  Informações sobre o desenho do estudo experimental que você usou.

Em nosso exemplo do genoma, o analista gostaria de saber qual é a unidade de medida para cada variável clínica / demográfica (idade em anos, tratamento por nome / dose, nível de diagnóstico e quão heterogêneo). Eles também gostariam de saber qual estratégia para resumir os dados do genoma (UCSC / Ensembl, etc.). Eles também gostariam de saber qualquer outra informação sobre como você fez a coleta de dados / desenho do estudo. Por exemplo, estes são os primeiros 30 pacientes que entraram na clínica? São 30 pacientes selecionados por alguma característica como a idade? Eles são obtidos por meio de sorteio aleatório?

Um formato comum para este documento é um arquivo do Word. Deve haver uma seção chamada "Desenho do estudo" que tenha uma descrição detalhada de como você coletou os dados. Existe uma seção chamada "Code book" que descreve cada variável e suas unidades.

### O Dicionário de Dados (Como codificar variáveis)

Quando você coloca variáveis em uma planilha, há várias categorias principais nas quais você vai se deparar, dependendo do tipo de dados:
 
*  Contínua
*  Discreta
*  Ordinal
*  Categórico
*  Dado faltante 
*  Dado Censurado

Variáveis contínuas são qualquer coisa medida em uma escala quantitativa que poderia ser qualquer número fracionário. Um exemplo seria algo como peso medido em kg. Dados ordinais são dados que têm um número fixo, pequeno (<100) de níveis, mas são ordenados. Isso poderia ser, por exemplo, respostas a pesquisas em que as escolhas são: ruim, justa, boa. Dados categóricos são dados em que existem várias categorias, mas não são ordenados. Um exemplo seria sexo: masculino ou feminino. Dados ausentes são dados que não são observados e você não conhece o mecanismo. Você deve codificar valores ausentes como NA. Dados censurados são dados em que você conhece o mecanismo de censura em algum nível (exemplo: renda 50.000 ou mais). 
Você **não** deve imputar / inventar / jogar fora as observações perdidas.

Em geral, tente evitar codificar variáveis categóricas ou ordinais como números. Quando você insere o valor para sexo nos dados arrumados, ele deve ser "masculino" ou "feminino". Os valores ordinais no conjunto de dados devem ser "ruim", "regular" e "bom", não 1, 2, 3. Isso evitará misturas potenciais sobre os efeitos de direção e ajudará a identificar erros de codificação.

Sempre codifique cada informação sobre suas observações usando texto. Por exemplo, se você estiver armazenando dados no Excel e usar uma forma de texto colorido ou formatação de plano de fundo de célula para indicar informações sobre uma observação ("entradas variáveis vermelhas foram observadas no experimento 1"), essas informações serão perdidas (não serão exportadas  para o R!) quando os dados são exportados como texto bruto. Cada pedaço de dados deve ser codificado sem sistema de cores.

# A lista de instruções / script

Você pode ter ouvido isso antes, mas a reprodutibilidade é muito importante na ciência. Isso significa que, quando você envia seu artigo, os revisores e o resto do mundo devem ser capazes de replicar exatamente as análises dos dados brutos até os resultados finais. 
Se você estiver tentando ser eficiente, provavelmente executará algumas etapas de resumo / análise de dados antes que os dados possam ser considerados arrumados.

O ideal é criar um script de computador (em R, Python ou qualquer outra coisa) que tome os dados brutos como entrada e produza os dados arrumados que você está compartilhando como saída. Você pode tentar executar seu script algumas vezes e ver se o código produz a mesma saída.

Em muitos casos, a pessoa que coletou os dados tem incentivo para tornar mais fácil para um estatístico acelerar o processo de colaboração. Eles podem não saber como codificar em uma linguagem como o R ou o Python. Nesse caso,  o que você deve fornecer ao estatístico é algo chamado pseudocódigo. Deve ser algo como:

    Etapa 1 - pegue o arquivo bruto , execute a versão 3.1.2 do software resumido com parâmetros a = 1, b = 2, c = 3.
    Etapa 2 - execute o software separadamente para cada amostra.
    Etapa 3 - obtenha a coluna três do outputfile.out para cada amostra e essa é a linha correspondente no conjunto de dados de saída.

### O que você deve esperar do analista (estatístico)

Quando você vira um conjunto de dados devidamente arrumado, diminui drasticamente a carga de trabalho do estatístico. Então, espero que eles voltem a você muito mais cedo. Mas os estatísticos mais cuidadosos verificarão sua receita, farão perguntas sobre as etapas realizadas e tentarão confirmar se podem obter os mesmos dados arrumados que você fez com, no mínimo, verificações no local.

#### Você deve então esperar do estatístico:

  *  Um script de análise que realiza cada uma das análises (não apenas instruções).
  * O código de computador exato que eles usaram para executar a análise.
  * Todos os arquivos / figuras de saída gerados por eles..

Esta é a informação que você usará no suplemento para estabelecer a reprodutibilidade e precisão de seus resultados. Cada uma das etapas da análise deve ser explicada com clareza e você deve fazer perguntas quando não entende o que o analista fez. É responsabilidade do estatístico e do cientista compreender a análise estatística. Talvez você não consiga executar as análises exatas sem o código do estatístico, mas deve ser capaz de explicar por que o estatístico realizou cada etapa com um colega de laboratório / seu investigador principal.
