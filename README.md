# **Exploração e análise de dados de crédito com SQL**

## **1 Informação dos dados:** 

Os dados referem-se a informações de clientes de uma instituição financeira e incluem as seguintes colunas:

* idade = idade do cliente
* sexo = sexo do cliente (F ou M)
* dependentes = número de dependentes do cliente
* escolaridade = nível de escolaridade do clientes
* salario_anual = faixa salarial do cliente
* tipo_cartao = tipo de cartao do cliente
* qtd_produtos = quantidade de produtos comprados nos últimos 12 meses
* iteracoes_12m = quantidade de iterações/transacoes nos ultimos 12 meses
* meses_inativo_12m = quantidade de meses que o cliente ficou inativo
* limite_credito = limite de credito do cliente
* valor_transacoes_12m = valor das transações dos ultimos 12 meses
* qtd_transacoes_12m  = quantidade de transacoes dos ultimos 12 meses

A tabela foi criada utilizando dois serviçoes da **Amazon Web Service (AWS)**: **Athena** juntamente com o **S3 Bucket**, utilizando uma versão dos dados disponibilizados em: https://github.com/andre-marcos-perez/ebac-course-utils/tree/main/dataset.


## **2 Exploração de dados:**

### 2.1 Explorando os dados brutos:

**Qual quantidade de linhas temos em nossa base de dados?**

**Query 1:** SELECT count(*) FROM credito
> Reposta: 2564 linhas

***Obs.:*** A base de dados no link fornecido contém mais linhas do que a seleção utilizada aqui. Decidimos filtrar os dados para esse caso, ma você pode utilizar todos os dados disponíveis.

**Vamos visualizar nossos dados e entender como estão distribuídos em uma tabela:** 

**Query 2:** SELECT * FROM credito LIMIT 10;
<p align="center">
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/query1-1.png?raw=true" alt="Dez primeiras linhas do dataset 1" width="62.3%"/>
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/query1-2.png?raw=true" alt="Dez primeiras linhas do dataset 2" width="35.6%"/>
</p>


> Podemos ver que a nossa tabela contém dados faltantes (na). Mais a frente, mostraremos como tratar os dados em relação aos na. Antes disso, vamos dar uma olhada no tipo dos dados em cada coluna. 

**Qual o tipo de dado em cada coluna?**

**Query 3:** DESCRIBE credito

![describe print](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query3.png?raw=true)


Agora que ja entendemos quais são os tipos de dados, vamos olhar mais atentamente para as varíaveis categóricas.

### 2.2 Explorando e visualizando variáveis categóricas:

**Quais as escolaridades do dataset?**

**Query 4:** SELECT escolaridade, COUNT(*) AS quantidade

FROM credito

GROUP BY escolaridade

ORDER BY quantidade DESC;

<p align="center">
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/escolaridade_quantidade.png?raw=true" alt="grafico 1" width="45%"/>
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/query4.png?raw=true" alt="escolaridade x quantidade print" width="50%"/>
</p>

> Os dados mostram diferentes níveis de escolaridade e é possível perceber que temos valores nulos (na) no dataset, vamos tratá-los à frente.

**Quais são os tipos de estado_civil disponíveis no dataset?**

**Query 5:** SELECT estado_civil, COUNT(*) AS quantidade

FROM credito

GROUP BY estado_civil

ORDER BY quantidade DESC;

<p align="center">
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/graficoquery5.png?raw=true" alt="grafico 2" width="45%"/>
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/query5.png?raw=true" alt="estado civil x quantidade print" width="50%"/>
</p>

> Novamente encontramos valores nulos na base de dados.

**Quais são os tipos de salario_anual disponíveis no dataset?**

**Query 6:** SELECT salario_anual, COUNT(*) AS quantidade

FROM credito

GROUP BY salario_anual

ORDER BY quantidade DESC;

<p align="center">
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/graficoquery6.png?raw=true" alt="grafico 3" width="45%"/>
  <img src="https://github.com/lukaasos/ebac-project_2_credit/blob/main/query6.png?raw=true" alt="salario anual x quantidade print" width="50%"/>
</p>

> Aqui temos a faixa salarial dos clientes. Também contém dados faltantes. 


**Quais são os tipos de cartão disponíveis no dataset?**

**Query 7:** SELECT tipo_cartao, COUNT(*) AS quantidade

FROM credito

GROUP BY tipo_cartao

ORDER BY quantidade DESC;

![Tipos de cartão](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query7.png?raw=true)

> Aqui não temos valores nulos, logo não precisaremos tratá-los.


## **3 Análise de dados**

Já conhecemos as informações do nosso banco de dados. Agora vamos analisar essas informações e buscar responder as perguntas:

**Pergunta 1 - Quantos clientes temos em cada faixa salarial?**

**Query 8:** SELECT salario_anual, COUNT(*) AS quantidade  

FROM credito 

GROUP BY salario_anual

ORDER BY quantidade DESC;

![Quantidade para cada faixa salarial](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query8.png?raw=true)

> Percebemos que a maioria dos clientes estão concentrados na faixa salarial menor que 40K anual. Talvez seja interessante criar soluções mais específicas voltadas para esse público (baixa renda). 235 clientes não informaram a faixa salarial ou não consta (na).

**Pergunta 2 - Como está distribuído o sexo dos clientes?**

**Query 9:** SELECT sexo, COUNT(*) AS quantidade 

FROM credito 

GROUP BY sexo

ORDER BY quantidade DESC;


![Quantidade para cada sexo](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query9.png?raw=true)

![Quantidade para cada sexo - gráfico](https://github.com/lukaasos/ebac-project_2_credit/blob/main/graficoquery9.png?raw=true)

> A maioria dos clientes desse banco é homem (~61%)! Podemos ver a proporção na visualização acima.

**Pergunta 3 - E se quisermos focar nosso marketing de acordo com as idades dos clientes?**

**Query 10:** SELECT AVG(idade) AS media_idade, MIN(idade) AS min_idade,

MAX(idade) AS max_idade, sexo

FROM credito

GROUP BY sexo
![Média de idades por sexo](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query10.png?raw=true)

> Os clientes desse banco de dados parecem estar bem distribuídos, com ambos tendo a idade mínima de 26 anos, uma média muito parecida e a idade máxima com uma diferença pequena. 


**Pergunta 4 - Qual a maior e menor transação dos clientes?**

**Query 11:** SELECT MIN(valor_transacoes_12m) AS transacao_minima, 

MAX(valor_transacoes_12m) AS transacao_maxima 

FROM credito
![Valor transacoes](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query11.png?raw=true)

> No banco de dados, os clientes fizeram transações que variam entre 510,16 e 4.476,58.

**Pergunta 5 - Quais as características dos clientes que possuem os maiores creditos?**

**Query 12:** 
SELECT MAX(limite_credito) AS limite_credito, escolaridade, tipo_cartao, 
sexo

FROM credito

WHERE escolaridade != 'na' AND tipo_cartao != 'na' 

GROUP BY  escolaridade, tipo_cartao, sexo 

ORDER BY limite_credito DESC

LIMIT 10

![Valor limite](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query12.png?raw=true)

> Não parece haver um impacto da escolaridade no limite. Talvez seja interessante começar a oferecer tipos diferentes de cartões para os clientes de acordo com o seu limite, visto que o tipo de cartão também não parece acompanhar o padrão de limite.

**Pergunta 6 - Quais as características dos clientes que possuem os menores creditos?**

**Query 13:** 
SELECT MAX(limite_credito) AS limite_credito, escolaridade, tipo_cartao, 
sexo

FROM credito

WHERE escolaridade != 'na' AND tipo_cartao != 'na' 

GROUP BY  escolaridade, tipo_cartao, sexo 

ORDER BY limite_credito ASC

LIMIT 10


![Valor limite](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query13.png?raw=true)

> Dessa vez conseguimos perceber duas coisas: Não há cartões platinum para esses clientes, e a maioria de clientes com limite baixo é mulher, enquanto a maioria com limite alto é homem. Isso nos leva a fazer a próxima pergunta.

**Pergunta 7 - Quem gastam mais?**

**Query 14:** 
SELECT MAX(valor_transacoes_12m) AS maior_valor_gasto, 

AVG(valor_transacoes_12m) AS  media_valor_gasto, MIN(valor_transacoes_12m) 

AS min_valor_gasto, sexo 

FROM credito

GROUP BY sexo 


![Valor transacoes/sexo](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query14.png?raw=true)

> Apesar da diferença nos limites, os gastos de homens e mulheres são similares!

Por fim, 


**Pergunta 8 - O salário impacta no limite?**

**Query 15:** SELECT AVG(qtd_produtos) AS qts_produtos, 

AVG(valor_transacoes_12m) AS media_valor_transacoes, AVG(limite_credito) AS 

media_limite,  sexo, salario_anual FROM credito 

WHERE salario_anual != 'na'

GROUP BY sexo, salario_anual

ORDER BY AVG(valor_transacoes_12m) DESC

![Valor salario_anualLimite](https://github.com/lukaasos/ebac-project_2_credit/blob/main/query15.png?raw=true)

>SIM! As pessoas que tem menor faixa salarial também apresentam menor limite de credito! Percebemos também que as mulheres ganham até 60K, enquanto os homens ganham na faixa acima de 120K.

# **4 Conclusão**
  
Essas foram **algumas** análises extraídas dessa base de dados.

Podemos pensar em alguns insights:

- A maior parte dos clientes possui renda até 40K, e pouquíssimos acima de 120K. Logo, seria interessante focar um marketing específico para esse grupo maior.
- A maior parte dos clientes é masculino, e são eles que possuem os maiores salários e maiores limites.
- A escolaridade não parece influenciar no limite nem no tipo do cartão
- Os clientes com menores limites são em sua maioria mulheres
- Dentre os menores limites não há presença de cartão platinum
- A faixa salarial impacta diretamente no limite de crédito
- Não existem clientes com salário anual acima de 60K do sexo feminino

**Uma análise mais profunda dos dados pode revelar as razões pelas quais as mulheres têm menor acesso a crédito. Além disso, esse pode ser um reflexo de questões culturais que precisam ser repensadas!**









