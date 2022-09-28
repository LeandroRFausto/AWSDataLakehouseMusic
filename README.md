# AWSDataLakehouseMusic

Constrói um data lakehouse na AWS. O provisionamento e o job inicial no Glue se dão por arquivos Terraform de forma automatizada. Ao rodar o Glue script, os dados semi-estruturados são copiados da pasta landing zone para a pasta bronze. Já nesta transformação são gerados arquivos parquet no formato delta. O crawler das pastas é feito manualmente, podendo também ser feito via Terraform. A finalização deste processo cria uma database que é utilizada como elemento centralizador no catálogo e no data lakehouse. Por fim, o Athena é utilizado para realizar query serverless, com os dados já estruturados. Não foi objeto do projeto realizar transformações, agregações e insights.

## Arquitetura
<p align="center">
<img src="https://github.com/LeandroRFausto/AWSDataLakehouseMusic/blob/master/prints/arq.png"/>
</p>

## Recursos utilizados:
* Terraform, como elemento que provisionou a estrutura.
* bucket s3 para armazenamento.
* IAM para administrar as permissões.
* VPC para criação de ENDPOINTS e GATEWAYS.
* Glue para as tranformações, processamentos iniciais, delta lake e catálogo dos dados.
* Amazon Athena para query serverless.

## Preparo preliminar do ambiente
* Criar uma conta AWS.
* Instalar o Terraform.
* Construir o script python que será executado no Glue.
* Criar as polices e roles em json que serão usadas no Glue.
* Construir os arquivos de extensão tf para: glue, iam, storage e variable.
* Criar um usuário IAM com as devidas permissões.
* Montar arquivo para versionamento.

## Provisionamento

Na IDE de sua escolha, navegar até a pasta terraform que contém os arquivos se extensão tl. Com os comandos terraform as estrutura será montada conforme a seguir:

<p align="center">
<img src="https://github.com/LeandroRFausto/AWSDataLakehouseMusic/blob/master/prints/datalake.png"/>
</p>

Para a construção são necessários os seguintes comandos: "terraform init", que inicia o programa; "terraform plan", que mostra a estrutura a ser montada e "terraform apply", que executa a construção. 

<p align="center">
<img src="https://github.com/LeandroRFausto/AWSDataLakehouseMusic/blob/master/prints/terraform_apply.png"/>
</p>

## Processamento
A inserção dos dados semi-estruturados ocorre manualmente na pasta landing zone. O script python e o arquivo delta core, responsável para geração dos arquivos na extensão delta, são inseridos na pasta glue scripts. Agora, o job do glue inserido pelo terraform já pode ser iniciado. Ao fim do processo, os dados são copiados da pasta landing zone para a pasta bronze. Nesta transformação são gerados arquivos parquet no formato delta.

<p align="center">
<img src="https://github.com/LeandroRFausto/AWSDataLakehouseMusic/blob/master/prints/delta_raw.png"/>
</p>

O crawler com ajuda do data source irá catalogar os dados centralizando a informação. Necessário informar que os dados serão "raspados" no formato delta. Para uma execução bem sucedida será necessário criar conexões, endpoints e gateways. No fim do processo uma database será criada com as tables e dados já estruturados.


## Query
Para consultas das tabelas criadas, é utilizado o Athena, um serviço de consultas interativas que facilita a análise de dados no Amazon S3 usando SQL padrão. O Athena não precisa de servidor.

<p align="center">
<img src="https://github.com/LeandroRFausto/AWSDataLakehouseMusic/blob/master/prints/athena.png"/>
</p>
