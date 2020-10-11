# IBD_TP_LOJA_VIRTUAL
Modelagem ER - Banco de Dados - Loja Virtual

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1vZ0fRxiSPFuvCjG4Hm_1_MRpO4xVDNWf#scrollTo=7k8FQmOEstmc)

<p align="center">O objetivo deste projeto é registrar as etapas do Trabalho Prático 2 da disciplina Introdução a Banco de dados 01/2020. O trabalho consiste na construção de um modelo de Banco de Dados do tipo Entidade Relacionamento.</p>

### Resumo

O projeto foi desenvolvido nas ferramentas Lucid Charts e Diagrams para a criação de diagramas e esquemas dos dados nas fases conceitual e logica. A parte da implementação Fisica foi desenvolvida na plataforma Google Colab utilizando a liguagem Python. Os nomes de Homens Mulheres e Produtos foram extraidos da web com os pacotes bs4 e requests. Os dados foram simulados com os pacotes numpy e random, armazenados em dataframes do pacote pandas e convertidos para SQL com o pacote sqlalchemy. Finalmente a função %timeit da liguagem Python foi utilizada para mensurar o desempenho das consultas.


## 1. Mini Mundo

O mini mundo escolhido foi a simplificação de um comercio eletrônico de venda de produtos que pode ser caracterizado por:
* Clientes que possuem uma identificação única, um Nome e Sobrenome e um endereço eletrônico.
* Ordens de pedido dos clientes que possuem um identificador único da ordem, a informação sobre o cliente que colocou a ordem, a informação dos produtos da ordem e a informação da data da colocação da ordem de compra. 
* Produtos disponíveis no estoque da loja com um identificador único do código do produto, o identificador do fornecedor do produto e o nome do Produto.
* Fornecedores de produtos com código de identificador único para cada fornecedor, Nome do fornecedor e endereço eletrônico do fornecedor.
* Entregas dos produtos com código único de entrega, código identificador do Produto e Data da Entrega.

## 2. Análise de Requisitos

Requisitos dos atributos das 5 entidades do mini mundo
* Cliente (ID_Cliente(nn), Nome, Email)
* Ordem (ID_Ordem(nn), ID_Produto(nn), Data)
* Produto (ID_Produto(nn), ID_Fornecedor(nn), Nome)
* Fornecedor (ID_Fornecedor(nn), Nome, Email)
* Entrega (ID_Entrega(nn), ID_Produto(nn), Data)

Restrições de Chaves:

![RI1](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Restricao_Integridade_1.png)

Restrições de Integridade Referencial:

![RI2](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Restricao_Integridade_2.png)

## 3. Projeto Conceitual

Mapa do Modelo Entidade Relacionamento:

![ER](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Modelo_ER_1.png)

## 4. Projeto Lógico

Mapa do Modelo Lógico

![Modelo Logico](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/Modelo_Logico.png)

Restrições das Relações Projeto Lógico

![Restrições Lógicas](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelagem/restricoes_projeto_logico.png)

## 5. Projeto Físico

O sistema de SGBD escolhido para alocar fisicamente os dados foi o mysql. Para criar o motor de buscas foi utilizada a função create_engine da biblioteca sqlalchemy da linguagem Python.

* Gera Data Frames no Pandas

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
                      'Data' : Data_Ordem})

# Remove Horario da Coluna Data tablea Ordem
Ordem['Data'] = [str(i) for i in pd.to_datetime(Ordem['Data']).dt.date]

# Declara dataframe de Produto com 10 produtos
Produto = pd.DataFrame({'ID_Produto' : range(21),
                        'ID_Fornecedor' : np.random.choice(range(9), 21).tolist(),
                        'Nome' : Produtos })

# Declara data frame de entrega com 5000 entregas
Entrega = pd.DataFrame({'ID_Entrega' : range(5000),
                        'ID_Produto' : Ordem.ID_Produto,
                        'Data' : Data_Entrega})

# Remove Horario da Coluna Data tabela Entrega
Entrega['Data'] = [str(i) for i in pd.to_datetime(Entrega['Data']).dt.date]

# Declara Data Frame Sobrenome
Sobrenome = pd.DataFrame({'ID_Cliente' : range(90),
                          'Sobrenome' : Sobrenomes})		  
```

* Define Função para_sql e gera motor SGBD

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

* Gera tabela de Clientes

```python
# Converte para SQL
sql = para_sql(Cliente,"Cliente")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Cliente;")
  
# Gera tabela 
engine.execute("CREATE TABLE Cliente ( \
                ID_Cliente mediumint(8) NOT NULL,\
                Nome varchar(255) default NULL,\
                Email varchar(255) default NULL,\
                PRIMARY KEY (ID_Cliente) \
                );")

# Popula tabela
engine.execute(sql)
```

* Gera tabela de Sobrenomes

```python
# Converte para SQL
sql = para_sql(Sobrenome,"Sobrenome")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Sobrenome;")
  
# Gera tabela 
engine.execute("CREATE TABLE Sobrenome ( \
                ID_Cliente mediumint(8) NOT NULL, \
                Sobrenome varchar(255) default NULL, \
                PRIMARY KEY (ID_Cliente) \
                );")

# Popula tabela
engine.execute(sql)
```

* Gera tabela de Fornecedor

```python# Converte para SQL
sql = para_sql(Fornecedor,"Fornecedor")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Fornecedor;")
  
# show the complete data 
engine.execute("CREATE TABLE Fornecedor ( \
                ID_Fornecedor  mediumint(8) NOT NULL, \
                Nome varchar(255) default NULL, \
                Email varchar(255) default NULL, \
                PRIMARY KEY (ID_Fornecedor) \
              );")

# Popula tabela
engine.execute(sql)
```

* Gera tabela de Produtos

```python
# Converte para SQL
sql = para_sql(Produto,"Produto")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Produto;")
  
# show the complete data 
engine.execute("CREATE TABLE Produto ( \
                ID_Produto mediumint(8) NOT NULL, \
                ID_Fornecedor mediumint, \
                Nome varchar(255) default NULL, \
                PRIMARY KEY (ID_Produto) \
              );")

# Popula tabela
engine.execute(sql)
```

* Gera tabela de Ordens

```python
# Converte para SQL
sql = para_sql(Ordem,"Ordem")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Ordem;")
  
# show the complete data 
engine.execute("CREATE TABLE Ordem ( \
                ID_Ordem  mediumint(8) NOT NULL, \
                ID_Cliente mediumint, \
                ID_Produto mediumint, \
                Data varchar(255) default NULL, \
                PRIMARY KEY (ID_Ordem) \
                );")

# Popula tabela
engine.execute(sql)
```

* Gera tabela de Entregas

```python
# Converte para SQL
sql = para_sql(Entrega,"Entrega")

# Apaga tabela se já existir
engine.execute("DROP TABLE IF EXISTS Entrega;")
  
# show the complete data 
engine.execute("CREATE TABLE Entrega ( \
                ID_Entrega mediumint(8) NOT NULL, \
                ID_Produto mediumint, \
                Data varchar(255), \
                PRIMARY KEY (ID_Entrega) \
                );")

# Popula tabela
engine.execute(sql)
```

## 6. Consultas

Foram Consideradas 10 consultas em SQL para avaliar a consistência e tempo gasto na execução de cada consulta. Para avaliar o tempo das consultas foi utilizado o comando %timeit da linguagem Python. Esse comando avalia a query fornecendo o tempo médio de resposta para 3 consultas.

* Consulta 1

```python
# π(IDCliente,   Nome,   Email) (Cliente)
%timeit pd.read_sql('SELECT * FROM Cliente', con = engine)
```

![Consulta 1](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q1.png)

* Consulta 2

![Consulta 2](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q2.png)

* Consulta 3

![Consulta 3](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q3.png)

* Consulta 4

![Consulta 4](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q4.png)

* Consulta 5

![Consulta 5](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q5.png)

* Consulta 6

![Consulta 6](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q6.png)

* Consulta 7

![Consulta 7](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q7.png)

* Consulta 8

![Consulta 8](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q8.png)

* Consulta 9

![Consulta 9](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consultas/q9.png)

* Consulta 10

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

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1vZ0fRxiSPFuvCjG4Hm_1_MRpO4xVDNWf#scrollTo=7k8FQmOEstmc)

<h4 align="center"> 
	🚧  IBD_TP_LOJA_VIRTUAL 🚀 Em construção...  🚧
</h4>
