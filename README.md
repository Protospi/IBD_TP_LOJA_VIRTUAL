# LOJA_VIRTUAL
#### Modelagem ER - Banco de Dados - Loja Virtual
#### Introdução a Bancos de Dados-UFMG
#### Aluno: Pedro Ladeira Loes

### Resumo
<p align="justify">
O objetivo deste projeto foi desenvolver um Modelo de Entidade Relacionamento, um Modelo Lógico e um SGBD com implementação física e simulação dos dados para a aplicação Loja Virtual considerando diretrizes de integridade referencial e lógica. O projeto foi desenvolvido nas ferramentas Lucid Charts e Diagrams para a criação de diagramas e esquemas dos dados nas fases conceitual e lógica. A parte da implementação Física foi desenvolvida na ferramenta Google Colab utilizando a liguagem Python. Os nomes de Homens, Mulheres, Sobrenomes e Produtos foram extraidos da web com os pacotes bs4 e requests. Os dados foram simulados com os pacotes numpy e random, armazenados em dataframes do pacote pandas e convertidos para SQL com o pacote sqlalchemy. Finalmente a função %timeit da liguagem Python foi utilizada para mensurar a performance das consultas.
</p>

[![Abrir em Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1vZ0fRxiSPFuvCjG4Hm_1_MRpO4xVDNWf?usp=sharing)

## 1. Mini Mundo

O mini mundo escolhido foi a simplificação de um comercio eletrônico de venda de produtos que pode ser caracterizado por:
* Clientes que possuem uma identificação única, um Nome e Sobrenome e um endereço eletrônico.
* Ordens de pedido dos clientes que possuem um identificador único da ordem, a informação sobre o cliente que colocou a ordem, a informação dos produtos da ordem e a informação da data da colocação da ordem de compra. 
* Produtos disponíveis no estoque da loja com um identificador único do código do produto, o identificador do fornecedor do produto e o nome do Produto.
* Fornecedores de produtos com código de identificador único para cada fornecedor, Nome do fornecedor e endereço eletrônico do fornecedor.
* Entregas dos produtos com código único de entrega, código identificador do Produto e Data da Entrega.

## 2. Análise de Requisitos

* Requisitos dos atributos das 5 entidades do mini mundo
	* Cliente (ID_Cliente(nn), Nome, Email)
	* Ordem (ID_Ordem(nn), ID_Produto(nn), Data)
	* Produto (ID_Produto(nn), ID_Fornecedor(nn), Nome)
	* Fornecedor (ID_Fornecedor(nn), Nome, Email)
	* Entrega (ID_Entrega(nn), ID_Produto(nn), Data)

* __Restrições de Chaves:__

![RI1](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Restricao_Integridade_1.png)

* __Restrições de Integridade Referencial:__

![RI2](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Restricao_Integridade_2.png)

## 3. Projeto Conceitual

Mapa do Modelo Entidade Relacionamento:

![ER](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Modelo_ER_1.png)

## 4. Projeto Lógico

Mapa do Modelo Lógico

![Modelo Logico](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Modelo_Logico.png)

* __Restrições das Relações Projeto Lógico__

![Restrições Lógicas](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/restricoes_projeto_logico.png)

## 5. Projeto Físico

O sistema de SGBD escolhido para alocar fisicamente os dados foi o mysql. Para criar o motor de buscas foi utilizada a função create_engine da biblioteca sqlalchemy da linguagem Python. Os nomes de Homens, Mulheres, Sobrenomes e Produtos foram extraídos da web com o pacote bs4 e requests utilizando a função BeautifulSoup. Os emails, data da ordem e da entrega foram gerados com funções padrão da linguagem python e o pacote datetime. O pacote random foi utilizado para randomizar os nomes de Clientes e Produtos nas Ordens. As tabelas foram compostas de 89 clientes, 9 fornecedores, 21 produtos, 5000 ordens e 5000 entregas. Os dados foram gerados como data frames do pacote pandas e transformados em strings no formato adequado para criação de tabelas em SQL.

* __Pacotes Utilizados__

```python

# Carrega Pacotes
import requests
from bs4 import BeautifulSoup
import numpy as np
import pandas as pd
import datetime
import random
from unidecode import unidecode
from sqlalchemy import create_engine 

```

* __Extração de Sobrenomes__

```python
# Define Url de Sobrenomes
url = 'https://www.procob.com/os-sobrenomes-mais-comuns-do-brasil/'
 
# Conecta com URL
reqs = requests.get(url)
 
# Extrai informações de lista ordenada
soup = BeautifulSoup(reqs.text, 'lxml')
 
# Define Sobrenomes
Sobrenomes = np.random.choice([tag.text for tag in soup.find_all("ol")][0].split(), 90)
```

* __Extração do Nome de Homens__

```python

# Define Url de Homens
url2 = 'https://www.minhavida.com.br/familia/materias/35919-100-nomes-para-meninos-mais-comuns-confira-lista'

# Conecta com URL
reqs = requests.get(url2)

# Extrai informações de lista ordenada
soup = BeautifulSoup(reqs.text, 'lxml')

# Define Sobrenomes
Homens = [tag.text for tag in soup.find_all("ul")][1].split()

```

* __Extração do Nome de Mulheres__

```python

# Define Url de Mulheres
url3 = 'https://www.minhavida.com.br/familia/materias/35925-100-nomes-para-meninas-mais-comuns-confira-lista'

# Conecta com URL
reqs = requests.get(url3)

# Extrai informações de lista ordenada
soup = BeautifulSoup(reqs.text, 'lxml')

# Define Sobrenomes
Mulheres = [tag.text for tag in soup.find_all("ul")][1].split()

```

* __Extração do Nome de Produtos__

```python

# Define Url de Melhores 10 celulares
url4 = 'https://mobizoo.com.br/opiniao/celulares-mais-vendidos/'

# Conecta com URL
reqs = requests.get(url4)

# Extrai informações da url
soup = BeautifulSoup(reqs.text, 'lxml')

# Define Produtos de Celulares
Produtos_Celulares = [tag.text for tag in soup.find_all("th")][5::4]

# Define Url de Melhores Desktops
url5 = 'https://spy.com/articles/gadgets/electronics/best-desktop-computers-reviews-247841/'

# Conecta com URL
reqs = requests.get(url5)

# Extrai informações da url
soup = BeautifulSoup(reqs.text, 'lxml')

# Define Variavel auxiliar com titulos completos com categoria
Produtos_Desktop = [tag.text for tag in soup.find_all("h2")][:11]

# Define Produtos Desktops 
Produtos_Desktop = list(map(lambda w: w.split(". ", 1)[1], Produtos_Desktop))

# Concatena Produtos
Produtos = Produtos_Celulares + Produtos_Desktop

```

* __Gera Emails e Datas de Ordem e Entrega__

```python

# Define Encoding do texto
encoding = "utf-8"

# Declara email de Clientes
email_Cliente = [unidecode(i) + j for i, j in zip(list(Homens[0:89]), np.random.choice(["@gmail.com","@yahoo.com","@bh.com"], 90).tolist())] 

# Email de Fornecedor
email_Fornecedor = [unidecode(i) + j for i, j in zip(list(Homens[90:99]), np.random.choice(["@gmail.com","@yahoo.com","@bh.com"], 90).tolist())]

# Data de Inicio
inicio = datetime.date(2020, 1, 1)
fim = datetime.date(2020, 10, 1)

# Calcula Data
tempo_entre_datas = fim - inicio
dias_entre_datas = tempo_entre_datas.days
dias_aleatorios = random.randrange(dias_entre_datas)
data_aleatoria = inicio + datetime.timedelta(days=dias_aleatorios)

# Data Ordem
Data_Ordem = pd.date_range(start = '2020-01-01', end = '2020-10-01', periods=5000)

# Data Entrega
Data_Entrega = [data + datetime.timedelta(days=random.randrange(dias_entre_datas)) for data in Data_Ordem]
```

* __Gera Data Frames do Pandas__

```python
# Declara data frame de Clientes com 500 clientes
Cliente = pd.DataFrame({'ID_Cliente' : range(89),
                       'Nome' : np.random.choice(Homens + Mulheres, 89).tolist(),
                       'Email' : email_Cliente})


# Declara data frame de Fornecedor com 50 Fornecedores
Fornecedor = pd.DataFrame({'ID_Fornecedor' : range(9),
                           'Nome' : Homens[90:99],
                           'Email' : email_Fornecedor})

# Declara data frame de Ordem com 5000 ordens
Ordem = pd.DataFrame({'ID_Ordem' : range(5000),
                      'ID_Cliente' : np.random.choice(range(500), 5000).tolist(),
                      'ID_Produto' : np.random.choice(range(20), 5000).tolist(),
                      'Data' : Data_Ordem,
                      'Nota_Fiscal' :  range(1000,6000)
                       })

# Remove Horario da Coluna Data tablea Ordem
Ordem['Data'] = [str(i) for i in pd.to_datetime(Ordem['Data']).dt.date]

# Declara dataframe de Produto com 10 produtos
Produto = pd.DataFrame({'ID_Produto' : range(21),
                        'ID_Fornecedor' : np.random.choice(range(9), 21).tolist(),
                        'Nome' : Produtos,
                        'Tipo' : ("Celular "*10).split() + ("Desktop "*11).split()
                       })

# Declara data frame de entrega com 5000 entregas
Entrega = pd.DataFrame({'ID_Entrega' : range(5000),
                        'ID_Produto' : Ordem.ID_Produto,
                        'Data' : Data_Entrega,
                        'Tipo' : np.random.choice(["PAC","SEDEX"],5000)
                       })

# Remove Horario da Coluna Data tabela Entrega
Entrega['Data'] = [str(i) for i in pd.to_datetime(Entrega['Data']).dt.date]

# Declara Data Frame Sobrenome
Sobrenome = pd.DataFrame({'ID_Cliente' : range(90),
                          'Sobrenome' : Sobrenomes
                         })
```

* __Define Função para_sql e gera motor SGBD__

```python

# Define Função
def para_sql(df, nome):
  rows = df.to_records(index=False)
  values = ', '.join(map(str, rows))
  sql = "INSERT INTO "+ nome + " VALUES {}".format(values)
  return sql.replace("""\'""", """\"""")

# Cria Motor de SQL 
engine = create_engine('sqlite:///ibdtp.db', echo = False)

```

* __Gera tabela de Clientes__

```python
# Converte para SQL
sql = para_sql(Cliente,"Cliente")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Cliente;")
  
# Gera tabela de Clientes
engine.execute("CREATE TABLE Cliente ( \
                ID_Cliente mediumint(8) NOT NULL,\
                Nome varchar(255) default NULL,\
                Email varchar(255) default NULL,\
                PRIMARY KEY (ID_Cliente) \
                );")

# Popula tabela
engine.execute(sql)
```

* __Gera tabela de Sobrenomes__

```python
# Converte para SQL
sql = para_sql(Sobrenome,"Sobrenome")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Sobrenome;")
  
# Gera tabela de Sobrenomes
engine.execute("CREATE TABLE Sobrenome ( \
                ID_Cliente mediumint(8) NOT NULL, \
                Sobrenome varchar(255) default NULL, \
                PRIMARY KEY (ID_Cliente) \
                );")

# Popula tabela
engine.execute(sql)
```

* __Gera tabela de Fornecedores__

```python# Converte para SQL
sql = para_sql(Fornecedor,"Fornecedor")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Fornecedor;")
  
# Gera tabela de Fornecedor
engine.execute("CREATE TABLE Fornecedor ( \
                ID_Fornecedor  mediumint(8) NOT NULL, \
                Nome varchar(255) default NULL, \
                Email varchar(255) default NULL, \
                PRIMARY KEY (ID_Fornecedor) \
              );")

# Popula tabela
engine.execute(sql)
```

* __Gera tabela de Produtos__

```python
# Converte para SQL
sql = para_sql(Produto,"Produto")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Produto;")
  
# Gera tabela de Produtos 
engine.execute("CREATE TABLE Produto ( \
                ID_Produto mediumint(8) NOT NULL, \
                ID_Fornecedor mediumint, \
                Nome varchar(255) default NULL, \
                Tipo varchar(255) default NULL, \
                PRIMARY KEY (ID_Produto) \
              );")

# Popula tabela
engine.execute(sql)
```

* __Gera tabela de Ordens__

```python
# Converte para SQL
# Converte para SQL
sql = para_sql(Ordem,"Ordem")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Ordem;")
  
# Gera tabela de Ordens 
engine.execute("CREATE TABLE Ordem ( \
                ID_Ordem  mediumint(8) NOT NULL, \
                ID_Cliente mediumint, \
                ID_Produto mediumint, \
                Data varchar(255) default NULL, \
                Nota_Fiscal mediumint default NULL, \
                PRIMARY KEY (ID_Ordem) \
                );")

# Popula tabela
engine.execute(sql)
```

* __Gera tabela de Entregas__

```python
# Converte para SQL
sql = para_sql(Entrega,"Entrega")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Entrega;")
  
# Gera tabelas de Entregas 
engine.execute("CREATE TABLE Entrega ( \
                ID_Entrega mediumint(8) NOT NULL, \
                ID_Produto mediumint, \
                Data varchar(255), \
                Tipo varchar(255) default NULL, \
                PRIMARY KEY (ID_Entrega) \
                );")

# Popula tabela
engine.execute(sql)
```

## 6. Consultas

Foram Consideradas 10 consultas em SQL para avaliar a consistência e tempo gasto na execução de cada consulta. Para avaliar o tempo das consultas foi utilizado o comando %timeit da linguagem Python. Esse comando avalia a query fornecendo o melhor tempo de 3.

* __Consulta 1: π(IDCliente,  Nome,   Email) (Cliente)__

```python 
# Executa Consulta 1 e avalia tempo
%timeit pd.read_sql('SELECT * FROM Cliente', con = engine)
```

![Consulta 1](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q1.png)

* __Consulta 2: π(IDProduto,   IDFornecedor,   Nome) (Produto)__

```python
# Executa Consulta 2 e avalia tempo
%timeit pd.read_sql('SELECT * FROM Produto', con = engine)
```

![Consulta 2](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q2.png)

* __Consulta 3: Cliente  ⋈ (IDCliente=IDCliente)  Ordem__

```python
# Executa Consulta 3 e avalia tempo
q3 = "SELECT c.ID_Cliente, c.Nome, c.Email, o.ID_Ordem, o.ID_Produto, o.Data \
      FROM Cliente as c \
      JOIN Ordem as o ON c.ID_Cliente = o.ID_Cliente"

# Avalia tempo gasto na consulta para projetar a jução de Cliente e Ordem
%timeit pd.read_sql(q3, con = engine)
```

![Consulta 3](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q3.png)

* __Consulta 4: Produto  ⋈ (IDProduto=IDProduto)  Ordem__

```python
# Executa Consulta 4 e avalia tempo
q4 = "SELECT p.ID_Produto, p.ID_Fornecedor, p.Nome as Nome_Produto, o.ID_Ordem, o.ID_Cliente, o.Data \
      FROM Produto as p \
      JOIN Ordem as o ON p.ID_Produto = o.ID_Produto"

# Avalia tempo gasto Consulta para projetar a jução de Produto e Ordem
%timeit pd.read_sql(q4, con = engine)
```

![Consulta 4](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q4.png)

* __Consulta 5: Fornecedor  ⋈ (IDFornecedor=IDFornecedor)  Produto__

```python
# Executa Consulta 5 e avalia tempo
q5 = "SELECT f.ID_Fornecedor, f.Nome, f.Email, p.ID_Produto, p.Nome as Nome_Produto  \
      FROM Fornecedor as f \
      JOIN Produto as p ON f.ID_Fornecedor = p.ID_Fornecedor"

# Avalia tempo gasto na consulta para projetar a junção de Fornecedor e Produto
%timeit pd.read_sql(q5, con = engine)
```

![Consulta 5](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q5.png)

* __Consulta 6: (Fornecedor  ⋈ (IDFornecedor=IDFornecedor)  Produto)⋈ (IDProduto=IDProduto)  Ordem__

```python
# Executa Consulta 6 e avalia tempo
q6 = "SELECT f.ID_Fornecedor, f.Nome, f.Email, p.ID_Produto, p.Nome as Nome_Produto, o.ID_Cliente, o.Data \
      FROM Fornecedor as f \
      JOIN Produto as p ON f.ID_Fornecedor = p.ID_Fornecedor \
      JOIN Ordem as o ON p.ID_Produto = o.ID_Produto"

# Consulta para projetar a junção de Fornecedor, Produto e Ordem
%timeit pd.read_sql(q6, con = engine)
```

![Consulta 6](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q6.png)

* __Consulta 7: (Produto  ⋈ (IDProduto=IDProduto)  Ordem)  ⋈ (IDCliente=IDCliente)  Cliente__

```python
# Executa Consulta 6 e avalia tempo
q7 = "SELECT p.ID_Produto, p.ID_Fornecedor, c.Nome as Nome_Cliente, o.ID_Ordem, o.ID_Cliente, o.Data \
      FROM Produto as p \
      JOIN Ordem as o ON p.ID_Produto = o.ID_Produto \
      JOIN Cliente as c ON o.ID_Cliente = c.ID_Cliente"

# Avalia tempo gasto na consulta para projetar a jução de Produto, Ordem e Cliente
%timeit pd.read_sql(q4, con = engine)
```

![Consulta 7](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q7.png)

* __Consulta 8: (Produto  ⋈ (IDProduto=IDProduto)  Ordem)  ⋈ (IDFornecedor=IDFornecedor)  Fornecedor__

```python
# Executa Consulta 7 e avalia tempo
q8 = "SELECT p.ID_Produto, p.ID_Fornecedor, f.Nome as Nome_Fornecedor, o.ID_Ordem, o.ID_Cliente, o.Data \
      FROM Produto as p \
      JOIN Ordem as o ON p.ID_Produto = o.ID_Produto \
      JOIN Fornecedor as f ON p.ID_Fornecedor = p.ID_Fornecedor"

# Avalia tempo gasto na consulta para projetar a jução de Produto, Ordem e Fornecedor
%timeit pd.read_sql(q8, con = engine)
```

![Consulta 8](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q8.png)

* __Consulta 9:  J (COUNT IDCliente) (Cliente  ⋈ (IDCliente=IDCliente)  Ordem)__ 

```python
# Executa Consulta 9 e avalia tempo
q9 = "SELECT c.ID_Cliente, c.Nome, COUNT(o.ID_Ordem) as Quantidade_de_Ordens  \
                         FROM Cliente as c \
                         JOIN Ordem as o ON c.ID_Cliente = o.ID_Cliente \
                         GROUP BY c.ID_Cliente \
                         ORDER BY Quantidade_de_Ordens DESC"

# Avalia tempo gasto na consulta para calcular a quantidade de ordens por cliente
%timeit pd.read_sql(q9, con = engine)
```

![Consulta 9](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q9.png)

* __Consulta 10: J (COUNT IDProduto) (Fornecedor  ⋈ (IDFornecedor=IDFornecedor)  Produto)__

```python
# Executa Consulta 10 e avalia tempo
q10 = "SELECT f.ID_Fornecedor, f.Nome as Nome_Fornecedor, COUNT(p.ID_Produto) as Quantidade_de_Produtos \
       FROM Fornecedor as f \
       JOIN Produto as p ON f.ID_Fornecedor = p.ID_Fornecedor \
       GROUP BY f.ID_Fornecedor \
       ORDER BY Quantidade_de_Produtos DESC"

# Consulta para Calcular a quantidade de produtos por fornecedor
%timeit pd.read_sql(q10, con = engine)
```

![Consulta 10](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q10.png)


## 7. Tecnologias

Tecnologias utilizadas:
* <a href = "https://colab.research.google.com/drive/1PeIUI6mdR7XZz9lMiVm0VKuIou6FKXZE#scrollTo=1bgkBvAGpI4U&uniqifier=1"> Colab .ipynb </a>
* <a href = "https://lucid.app/lucidchart/"> LucidCharts </a>
* <a href = "https://app.diagrams.net/"> Diagrams </a>

## 8. Bibliografia

* Aulas da Disciplina Introdução a Bancos de Dados DCC-UFMG
* Slides da Disciplina Introdução a Bancos de Dados DCC-UFMG
* Livro Texto Sistemas de Banco de Dados - Elmasri Navathe - 4ª-Edicao

[![Abrir em Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1vZ0fRxiSPFuvCjG4Hm_1_MRpO4xVDNWf?usp=sharing)

<h4 align="center"> 
	🚧  IBD_TP_LOJA_VIRTUAL 🚀 Em construção...  🚧
</h4>
