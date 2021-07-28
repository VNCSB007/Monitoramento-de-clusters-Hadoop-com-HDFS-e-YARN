# TEORIA E PRÁTICA HDFS

## Configuração Inicial da VM

No virtual Machine, na parte de config, mantenha da maneira que parecer melhor, de acordo com as capacidades de CPU e armazenamento (aloque 50% dos recursos disponíveis no seu computador para o cluster)

um detalhe importante: alunos mostraram dificuldade em utilizar o cluster alocando apenas 4GB

Caso haja problemas em Startar o VM, é possível que no download tenha se perdido pacotes: caso este erro persista, recomenda-se fazer um redownload

O terminal do Linux não é o melhor ambiente para se trabalhar com Hadoop, recomenda-se  a ide "moba" ou "put (?)"

Acaso comandos simples do sudo aparecerem como faltantes ou não existentes, recomenda-se fazer o download pelo terminal: "$ sudo yum install nettools -y"

## Hadoop-Distributed File System

"**Big Data** é um processo de **análise e interpretação** de um **grande volume de dados** armazenados remotamente. (...) O Big Data pode **integrar** qualquer dado coletado sobre um **assunto ou uma empresa,** como os registros de compra e venda e mesmo os canais de interação não digital (telemarketing e call center). **Onde há um registro feito, a tecnologia o alcança**" - FIA, 2018 

Processamento tradicional (escalabilidade vertical) x processamento distribuído (escalabilidade horizontal)

Escalamento elástico: manter necessidade computacional de acordo com a demanda do serviço, ou seja, quando for preciso (aquele problema do pai com os 17 mil que processou 2 bilhões)

#### Cluster

Cluster: são computadores conectados por uma rede para a promoção conjunta de armazenamento, processamento e gerenciamento de recursos

Nó: forma de se referir a um computador individual inserido em um cluster. Existe um Nó Master (conhecido como _driver_) que gerencia a distribuição de trabalho para os Nós Comuns (_worker_)

daemon: um programa (conhecido como _serviço_) rodando dentro de um nó. 

Ou seja, cada nó worker e driver tem um serviço específico  dentro do Cluster

#### Hadoop

- conhecido como s.o. de Big Data 
- Open-source escrito em Java

- Escalável, confiável, processamento distribuído, tolerância a falhas que acontecem em hardware ou software ou rede
- Permite utilizar hardware comum, que seja com poucas características fora do padrão de mercado (_commodity cluster computing_)
- Framework para computação distribuída, com filesystem distribuído (HDFS)

##### Qual o core do Hadoop: 

Processamento:

- Spark
- MapReduce

Administração de Recursos:

- YARN

Armazenamento:

- HDFS

#### HDFS

- Baseado no Google FS (FileSystem) 
- Escalável e tolerante a falhas (imagine que um node tenha caído, e agora? o arquivo presente naquela máquina está perfeitamente presente no cluster, distribuído por todas as outras máquinas conectadas, de forma que o arquivo continue disponível na rede)
- '.txt', sequence file, Parquet, AVRO, ORC, ...
- Tamanho mínimo de bloco (padrão: 128 MB, ou seja, ele repartirá os arquivos na base de 128 MB [265 => 128 + 128 + 9])
- Fator de replicação (padrão: 3)

NameNode: gerencia o namespace e "armazena" os metadados (responsável pelos meta dados, que abstrai a noção do que temos sobre Dado)

DataNode: aonde armazena os metadados

Secondary NameNode: ajuda o NameNode caso ele sofra algum problema ou caia do sistema, e assim troca de papel para NameNode, e o antigo NameNode passa a ser Secondary NameNode

##### PUT / GET 

(nesse caso, o arquivo 'file_teste', aparentemente apenas um placeholder, existe também dentro do cluster)

- **COPIAR ARQUIVOS** HDFS para a máquina local: 

$ hdfs dfs -get /temp/file_teste.txt

- **INGESTÃO MANUAL** para o HDFS:

$ hdfs dfs -put file_teste.txt /user/pasta/

## Utilizando o Cluster 

Antes de começar a utilizar o cluster, é preciso iniciar esses programas:

1. $ sudo service hadoop-hdfs-namenode start
2. $ sudo service hadoop-hdfs-secondarynamenode start (com a função ll [será que é ls?], encontrará uma pasta 'script_apoio'. faça então: "sh script_apoio/start_all_service.sh")
3. $ sudo service hadoop-hdfs-datanode start
4. $ sudo service hadoop-mapreduce-historyserver start
5. $ sudo service hadoop-yarn-resourcemanager start
6. $ sudo service hadoop-yarn-nodemanager start

Caso você precise reiniciar o sistema ainda na fase de downloads, também considere fazer um: shutdown -r 0 

para restartar a máquina, esperar e voltar o serviço para tentar novamente

