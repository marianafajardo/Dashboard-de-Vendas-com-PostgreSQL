<h1 align="center">Dashboard de Vendas com PostgreSQL</h1>

<p align="center">
  <img src="https://user-images.githubusercontent.com/102304054/183674773-fa735d7e-b7a1-400e-8e08-aff5dcd977fa.png">
</p>
 
 ## 1. Dashboard

Esse dashboard foi desenvolvido através do curso de **"Power BI Para Data Science, Versão 2.0"**, realizado pela [Data Science Academy](https://www.datascienceacademy.com.br/).Ele pode ser visualizado de forma online e interativa, clicando na imagem abaixo:

<p align="center">
<a href="https://app.powerbi.com/view?r=eyJrIjoiMjE5NWUyMzUtZjBkNS00ZWFhLTg3NmItOGNkOGM0YzVmNGIwIiwidCI6IjhlNDJlNTBlLTNkMWEtNDAzYy04ZWZmLTU4OGJkOGQxMjk5ZiJ9"><img src="https://user-images.githubusercontent.com/102304054/183627296-4be6b9d5-fb10-4571-8994-3db12da6c6e1.png"></a>
</p>

## 2. Estudo de Caso

Você é Analista de Dados na empresa PowerMasterLearning, uma rede de varejo que vende produtos eletrônicos e eletrodomésticos com lojas espalhadas por diversas cidades do Brasil. A empresa começou sua operação no Brasil em 2012 e atua nos quatro estados da região sudeste mais os estados do Paraná e Bahia.

A empresa está montando a estratégia de vendas para o próximo ano e precisa saber qual dos fabricantes dos produtos vendidos, apresenta melhor desempenho nas vendas. O objetivo é descartar os fabricantes cujos produtos possuem poucas vendas e tentar negociar melhores condições com os principais fabricantes.

Em paralelo a isso, a empresa gostaria de ter diferentes visões das vendas realizadas nos últimos 4 anos (período de 2012 a 2015). Deve ser possível segmentar os relatórios de vendas por diferentes informações e por diferentes ângulos. Estas informações irão suportar as estratégias da empresa para o próximo ano.

Os dados foram extraídos de diferentes tabelas de um banco de dados transacional e como a empresa vai usar os relatórios todos os meses, alguns consultores recomendaram o uso de um Data Warehouse. Aqui as colunas disponíveis nos arquivos txt:

<img src="https://user-images.githubusercontent.com/102304054/183627390-2d5f77c6-13de-41ac-a2e2-1fd9ade52d0f.png"/><a>
</p>

Haverá diversas reuniões para definição da estratégia de vendas e os relatórios poderão ser extraídos sob demanda, de acordo com a necessidade dos gestores. Por conta disso, você deve criar um modelo de dados que permita a extração de relatórios a qualquer momento e que permita extrair dados por diferentes visões e ângulos. Os dados devem ser alimentados em um banco de dados consolidado, que será atualizado mensalmente com novos dados.

## 3. Indicadores

Após implementação de um Data Warehouse, carregamento dos dados no Power BI e conclusão do processo de *ETL - Extract, Transform e Load*, foi possível dar início no desenvolvimento das visualizações, conforme as informações solicitadas pela área de negócio.

**1. Qual é o total do valor de venda por categoria de produto?**

<img src="https://user-images.githubusercontent.com/102304054/183627451-ceebbf12-8c8b-4b1c-8d44-dfbd9edc182b.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado um *Gráfico de Colunas Empilhadas*, trazendo para o eixo X, a coluna **"Categoria"**, e para o eixo Y, a coluna de **"valor"** de vendas, correspondente a soma dessa coluna (valor total).

**2. Qual é o total do valor de venda por ano?**

<img src="https://user-images.githubusercontent.com/102304054/183627469-4480e541-cfef-48be-a7f7-31d487e46613.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado um *Gráfico de Barras Clusterizado*, trazendo para o eixo X, a coluna **"Valor"** de venda, e para o eixo Y, a coluna de **"Ano_Cond"**. Além disso, uma *linha tracejada* mostrando a *Média do Valor de Venda*, foi adicionada ao gráfico. Concluindo-se que os anos de 2015 e 2013, tiveram vendas a cima da média, enquanto que os anos de 2014 e 2021, tiveram vendas abaixo da média.

**Observação:** No momento de construir esse gráfico, identificou-se que a coluna **"Ano"** estava vazia, e em contato com a área de área de negócio, informaram que a fonte de dados estava sendo modificada, sendo assim, disseram que por ora, essa tabela no banco de dados, podia ser carregada apenas com a coluna de **"Data_Completa"** preenchida, deixando as outras três colunas vazias, já que no futuro elas também serão preenchidas. Porém, o gerente da área de vendas, solicitou que essa informação fosse apresentada o quanto antes no relatório, sendo assim, esse problema teve que ser resolvido de forma provisória.

<img src="https://user-images.githubusercontent.com/102304054/183627483-b5c221c7-d9c7-4673-8f95-63f52017bbd4.png"/><a>
</p>

Sabe-se que é possível criar uma coluna apenas com a informação de **"Ano"**, alimentando-a usando um critério de extração da coluna de **"Data_Completa"** (*Split*), ou seja, fazendo a divisão dessa coluna, e tudo que estiver depois da segunda barra, adiciona-se na coluna de **"Ano"**, porém, é necessário lembrar que esses dados são de um banco de dados, então, provavelmente, essa tabela será modificada daqui algum tempo, gerando problemas quando os dados das colunas **"Dia"**, **"Mês"** e **"Ano"** estiverem disponíveis.

Como forma alternativa, foi criado uma *coluna condicional*, essa coluna será alimentada sempre que houver mudança na coluna original. Para criação da *coluna condicional*, é preciso clicar em Página Inicial, Transformar Dados, Adicionar Coluna e por último em Coluna Condicional.

<img src="https://user-images.githubusercontent.com/102304054/183627501-247585fd-75c8-4275-81a4-2d53fbcacb92.png"/><a>
</p>

No final, na opção *"Else"*, foi adicionado um NA, indicando que nenhum dos anos configurados a cima foram encontrados, sendo possível atualizar essa informação em algum momento futuro.

<img src="https://user-images.githubusercontent.com/102304054/183627510-971c2eb5-6efd-434a-977a-bc7cf367a0b2.png"/><a>
</p>

Se amanhã, a coluna **"DATA_COMPLETA"** for atualizada no banco de dados, é possível dar um *"Refresh"* e automaticamente a coluna **"ANO_Cond"** será atualizada.

**3. Qual é o total do valor de venda por marca?**

<img src="https://user-images.githubusercontent.com/102304054/183627597-193ff34d-6949-452f-8007-f9e1140c0efb.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado um *Gráfico de Área*, trazendo para o eixo X, a coluna **"Marca"**, e para o eixo Y, a coluna **"Valor"** de Venda. 

**4. Qual é a média do valor de venda por segmento?**

<img src="https://user-images.githubusercontent.com/102304054/183627607-95c33e76-c136-43a8-8f9d-f7321f0520ac.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado um *Gráfico de cascata*, trazendo para a opção de categoria, a coluna **"Segmento"** e para o eixo Y, a coluna **"Valor"** de Venda, porém alterando o valor correspondente a essa coluna, de *Soma* para *Média*.

**5. Qual é o total do valor de venda?**

<img src="https://user-images.githubusercontent.com/102304054/183627619-593e13bb-15b0-48d1-af3d-eaed8890b431.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado um *cartão*, trazendo o total da coluna **"Valor"** de venda.

**6. Qual é a média do valor de venda?**

<img src="https://user-images.githubusercontent.com/102304054/183627635-1b034df0-34d1-4428-bab6-77de923c774d.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado um *cartão*, trazendo a coluna **"Valor"** de venda, porém alterando o valor correspondente a essa coluna, de *Soma* para *Média*.

**7. Quais são os principais influenciadores?**

<img src="https://user-images.githubusercontent.com/102304054/183627660-9d81c627-c75f-470d-8900-213996906a8f.png"/><a>
</p>

Para responder a essa pergunta, foi utilizado a visualização de *Principais influenciadores*, colocando a coluna **"Valor"** de venda em *Analisar* e a coluna **"Nome_Produto"** em *Explicar por*.




