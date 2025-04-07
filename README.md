<h1 align="center"> MVP1_PUCRIO_CANDIDATOS </h1> 
<h2 align="center">Repo for MVP of Data Science and Analytics at PUC RIO MBA</h2> 


## Resumo do Projeto
Esse projeto tem como função cumprir etapa obrigatória da disciplina de Engenharia de Dados do Curso de MBA em Data Science e Analytics oferecido pela Puc Rio.

## Objetivo
Criar um pipeline que envolva a busca, coleta, modelagem, carga e análise dos dados.
Nos próximos passos descreverei o projeto e seus passos.

### Objetivo do projeto:
A ideia inicial seria um mapeamento do público de candidatos para oferta de uma consultoria de gestão de patrimônio, considerando que candidatos possuem grande patrimônio e pouca disponibilidade de tempo para gestão e planejamento de evolução patrimonial.

Como desdobramento do objetivo, analisar a evolução patrimonial dos candidatos recorrentes na eleição de 2022, para isso utilizarei os dados disponibilizados pelo [Portal de Dados Abertos do TSE](https://dadosabertos.tse.jus.br/), pegando dois tipos de arquivos, os arquivos de candidatos e os de bens declarados dos candidatos dos seguintes anos:
1. 2022 - Presidenciais [Candidatos](https://dadosabertos.tse.jus.br/dataset/candidatos-2022/resource/435145fd-bc9d-446a-ac9d-273f585a0bb9) [Bens](https://dadosabertos.tse.jus.br/dataset/candidatos-2022/resource/fac824ef-8519-4c75-b634-378e6fcc717f)

2. 2020 - Municipais [Candidatos](https://dadosabertos.tse.jus.br/dataset/candidatos-2020-subtemas/resource/8187b1aa-5026-4908-a15a-0bf777ee6701) [Bens](https://dadosabertos.tse.jus.br/dataset/candidatos-2020-subtemas/resource/4b5e016e-feed-4ff6-bf86-78217927709a)

3. 2018 - Presidenciais [Candidatos](https://dadosabertos.tse.jus.br/dataset/candidatos-2018/resource/d9cb832e-fa52-4b62-8ee3-8d68d5620116) [Bens](https://dadosabertos.tse.jus.br/dataset/candidatos-2018/resource/84475557-764f-4457-9277-92b58fbb5f80)

Esses dados estão sob licença de uso aberto para uso, manipulação de divulgação.

Vale ressaltar que este projeto não possui cunho político ou partidário, apenas a satisfação de um aprofundamento sob dados aplamente divulgados pelos orgãos competentes.



### Proposta Inicial
A Proposta inicial era avaliar todos os períodos eleitorais, construir bases por candidatos, os anos em que concorreram e todo o descritivo de informações, entretanto por questões de prazo para entrega do projeto, optei por focar nas eleições de 2022 e analisar os candidatos recorrentes nas duas eleições anteriores.<br>

A escolha de 2022 não foi por acaso, a princípio seriam as eleições ocorridas em 2024, entretanto no mesmo ano uma normativa fez com que os CPF's dos candidatos fossem tratados como informações não divulgável, o que dificultaria e muito as relações a serem feitas entre os anos de eleição. No projeto dou uma sujestão para contornar esse problema (usando nome, data de nascimento e UF de nascimento), entretanto a garantia de uma equivalencia fica fraca e sem uma forma plausível de validação.<br>

Tinha o intuito de construir um Data Dash em Power BI, entretanto até o presente momento não consegui(apesar de muita busca), vincular as bases do DataBricks Community à ferramenta, provavelmente no futuro tentarei mais.<br>

Toda a carga dos dados seria realizada via código, entretanto por serem arquivos razoavelmente pesados o download + descompactação + transferencias dos mesmos para as pastas, demandou um pouco de mais do processamento disponível do Databricks, optei por manualmente fazer upload dos arquivos .csv já descompactados.

### Metodologia
A seguir levanterai alguns pontos do processo de tratamento e disponibilização dos dados:

Todo projeto foi feito no Databricks Community, os arquivos utilizados estão disponibilizados nesse repo.
O arquivo [Main](https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/3522802840626448/980842473257537/2192005160190208/latest.html) com todas as etapas e análise e um arquivo de reset, já que o cluster é reiniciado e todas as tabelas são deletadas, entretanto o log guarda o local impossibilitanto a criação das mesmas tabelas, sendo necessário a exclusão dos diretórios.

O código intercala uso de py_spark, python e SQL, como o curso oferece suporte ao uso de SQL todas as consultas que pude fiz via o sqlContext, entretanto os comandos de criação de tabelas permanente funcionaram melhor via df do spark.

#### Bronze Layer

#### Silver Layer

#### Gold Layer

#### Análise
$\frac{3x-1}{2}$


```math
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```
#### Conclusão

### Auto




