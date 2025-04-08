<h1 align="center"> MVP1_PUCRIO_CANDIDATOS </h1> 
<h2 align="center">Repo for MVP of Data Science and Analytics at PUC RIO MBA</h2> 


## Resumo do Projeto
Esse projeto tem como função cumprir etapa obrigatória da disciplina de Engenharia de Dados do Curso de MBA em Data Science e Analytics oferecido pela Puc Rio.

## Objetivo
Criar um pipeline que envolva a busca, coleta, modelagem, carga e análise dos dados.
Nos próximos passos descreverei o projeto e seus passos.

### Objetivo do projeto:
A ideia inicial seria um mapeamento do público de candidatos para oferta de uma consultoria de gestão de patrimônio, considerando supostamente que candidatos possuem grande patrimônio e pouca disponibilidade de tempo para gestão e planejamento de evolução patrimonial.

Como desdobramento do objetivo, analisar a evolução patrimonial dos candidatos recorrentes na eleição de 2022, para isso utilizarei os dados disponibilizados pelo [Portal de Dados Abertos do TSE](https://dadosabertos.tse.jus.br/), pegando dois tipos de arquivos, os arquivos de candidatos e os de bens declarados dos candidatos dos seguintes anos:
1. 2022 - Presidenciais [Candidatos](https://dadosabertos.tse.jus.br/dataset/candidatos-2022/resource/435145fd-bc9d-446a-ac9d-273f585a0bb9) [Bens](https://dadosabertos.tse.jus.br/dataset/candidatos-2022/resource/fac824ef-8519-4c75-b634-378e6fcc717f)

2. 2020 - Municipais [Candidatos](https://dadosabertos.tse.jus.br/dataset/candidatos-2020-subtemas/resource/8187b1aa-5026-4908-a15a-0bf777ee6701) [Bens](https://dadosabertos.tse.jus.br/dataset/candidatos-2020-subtemas/resource/4b5e016e-feed-4ff6-bf86-78217927709a)

3. 2018 - Presidenciais [Candidatos](https://dadosabertos.tse.jus.br/dataset/candidatos-2018/resource/d9cb832e-fa52-4b62-8ee3-8d68d5620116) [Bens](https://dadosabertos.tse.jus.br/dataset/candidatos-2018/resource/84475557-764f-4457-9277-92b58fbb5f80)

Esses dados estão sob licença de uso aberto para uso, manipulação e divulgação.

Vale ressaltar que este projeto não possui cunho político ou partidário, apenas a satisfação de um aprofundamento sob dados aplamente divulgados pelos orgãos competentes.



### Proposta Inicial
A Proposta inicial era avaliar todos os períodos eleitorais, construir bases por candidatos, os anos em que concorreram e todo o descritivo de informações, entretanto por questões de prazo para entrega do projeto, optei por focar nas eleições de 2022 e analisar os candidatos recorrentes nas duas eleições anteriores.<br>

A escolha de 2022 não foi por acaso, a princípio seriam as eleições ocorridas em 2024, entretanto no mesmo ano uma normativa fez com que os CPF's dos candidatos fossem tratados como informações não divulgável, o que dificultaria e muito as relações a serem feitas entre os anos de eleição. No projeto dou uma sujestão para contornar esse problema (usando nome, data de nascimento e UF de nascimento), entretanto a garantia de uma equivalencia fica fraca e sem uma forma plausível de validação.<br>

Tinha o intuito de construir um Data Dash em Power BI, entretanto até o presente momento não consegui(apesar de muita busca), vincular as bases do DataBricks Community à ferramenta, provavelmente no futuro tentarei mais.<br>

Toda a carga dos dados seria realizada via código, entretanto por serem arquivos razoavelmente pesados o download + descompactação + transferencias dos mesmos para as pastas, demandou um pouco de mais do processamento disponível do Databricks, optei por manualmente fazer upload dos arquivos .csv já descompactados.

### Metodologia
A seguir levanterai alguns pontos do processo de tratamento e disponibilização dos dados:

Todo projeto foi feito no Databricks Community, os arquivos utilizados estão disponibilizados nesse repo.
O arquivo [Main](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3522802840626448/980842473257537/2192005160190208/latest.html) com todas as etapas e análise e um arquivo de [Reset](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3522802840626448/3568646841543170/2192005160190208/latest.html), já que o cluster é reiniciado e todas as tabelas são deletadas, entretanto o log guarda o local impossibilitanto a criação das mesmas tabelas, sendo necessário a exclusão dos diretórios.

O código intercala uso de py_spark, python e SQL, como o curso oferece suporte ao uso de SQL todas as consultas que pude fiz via o sqlContext, entretanto os comandos de criação de tabelas permanente funcionaram melhor via df do spark.

#### Bronze Layer
Como dito, os dados foram coletados manualmente e feito upload dos .csv's; a partir do .csv ingestados apenas fiz validações simples sobre os arquivos existirem no devido diretório e um loop para criar pastas distintas para cada tipo de arquivo e para inserí-los no hive da maneira que estão criando as respectivas tabelas em bronze layer.

#### Silver Layer
Aqui foi feito todo a compreensão, tratamento e ajuste que os dados necessitavam para a disponibilização.

Os tratamentos se deram em suma pela conversão dos tipos dos campos, já que pelo upload, todas as colunas dos .csv's ao serem lidos vieram em formato string.

Um pequeno tratamento com relação a alguns dados corrompidos no csv de 2022.

Entratas dos .csv's sem a informação dos campos que seriam usados como chaves de relacionamento, nesses casos optei por retirar as entradas, já que não havia tratamento a ser feito.

Houve um problema tratado com relação a coluna "SQ_CANDIDATO" usada como chave primária. Esse campo é único por candidato e por ano de eleição, implicando em, candidatos que disputaram o 2º turno possuem na base duas entradas com o mesmo código, como solução realizei um _join_  filtrando entradas que não tiveram um turno maior, assim os 1º's turnos daqueles que não disputaram essa etapa e os 2º's daqueles que disputaram.

#### Gold Layer
Idealizei as tabelas finais em esquema Star(Estrela) no qual as tabelas teriam as informações necessárias dentro de uma única tabela fato e métricas disponibilizadas para essas métricas.
Como a análise consiste em comparação de fatos em períodos diferentes, disponibilizar uma tabela com colunas necessárias _pivotadas_ por anos tira a necessidade dos _joins_ na análise final.

Ao final foram disponibilizadas 4 tabelas nessa camada, sendo que uma delas (tabela de patrimonio) já a totalização dos dados necessários para responder as perguntas levantas.

#### Análise
A análise foi pautada numa única métrica, a variação de patrimonio entre dois anos analisados, dado pela formula de variação em percentuais: 


```math
\frac{valor \quad recente}{valor \quad antigo} - 1
```

Aplicando essa fórmula a diversas quebras como raça declarada, situação final da eleição anterior, idade e etc. Pude mapear bem quais características discriminam os candidatos com maior variação de patrimônio seja negativa ou positiva.

Alguns gráficos foram gerados para facilitar a compreensão dos dados, sujiro entras no notebook via link publicado para melhor visualização.


### Auto Avaliação
Acredito que quando iniciamos um projeto dessa magnitude temos intenções mirabolantes, claro sem levar em consideração os percaussos a serem encontrados pelo caminho.

Apesar de trabalhar com consumo de informações e tabelas em nuvem, nunca tinha realizado o processo de ETL. Uso no meu cotidiano a linguagem SQL por isso e pelo suporte do curso tentei usar ao máximo a linguagem dentro do código, entretanto algumas ações ficaram muito mais práticas e eficientes em spark/python dada algumas limitações da linguagem.

Gastei um tempo precioso tentando otimizar o descompactamento dos arquivos via própria ferramenta Databricks Community sem sucesso, após tentei integrar com algum serviço da amazon para realizar o download, descompactação e ingestão "automática", também sem sucesso. No fim optei por fazer o upload manual, arcaico porém funcional.

Como comentado no documento de código, os dados são relativamente bem dispostos, necessitando pouco tratamento, o que foi feito foi a compreensão de sua disponibilização e como deixar os dados o melhor possível para seu consumo nas camadas finais, respeitando o máximo possível boas práticas de Engenharia e os requisitos do projeto.

Confesso que senti falta de uma documentação apropriada para planejamento deste processo de criação de um banco de dados em uma plataforma **na Nuvem** end-to-end, isso aumentou muito a curva de aprendizado e a resolução de problemas encontrados com soluções que julguei as mais corretas, mas sem muito fundamento para as mesmas, apenas pautado pelo que tenho de experiência trabalhando na área de dados e o que entendi por ser mais útil para disponibilizar as tabelas de consumo finais.

Ademais agradeço a oportunidade e a proposta do projeto, tenho alguns anos de experiência na área de dados e poucas vezes fui confrontados com os questionamentos de como e por que fazer certas escolhas no processo de criação de um bando de dados

# Autores
| [<img src="https://avatars.githubusercontent.com/u/131409712?v=4"  width=115><br><sub>Juan Lima</sub>](https://github.com/GruveJL)



