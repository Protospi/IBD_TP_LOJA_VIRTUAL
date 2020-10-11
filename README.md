# IBD_TP_LOJA_VIRTUAL
Modelagem ER - Banco de Dados - Loja Virtual

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1PeIUI6mdR7XZz9lMiVm0VKuIou6FKXZE#scrollTo=1bgkBvAGpI4U&uniqifier=1)

<p align="center">O objetivo deste projeto é registrar as etapas do Trabalho Prático 2 da disciplina Introdução a Banco de dados 01/2020. O trabalho consiste na construção de um modelo de Banco de Dados do tipo Entidade Relacionamento.</p>


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

![RI1](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Restricao_Integridade_1.png)

Restrições de Integridade Referencial:

![RI2](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Restricao_Integridade_2.png)

## 3. Projeto Conceitual

Mapa do Modelo Entidade Relacionamento:

![ER](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelo_ER_1.png)

## 4. Projeto Lógico

Mapa do Modelo Lógico

![Modelo Logico](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Modelo_Logico.png)

## 5. Projeto Físico

O sistema de SGBD escolhido para alocar fisicamente os dados o mysql. Para criar o motor de buscas foi utilizada a função create_engine da biblioteca sqlalchemy.

## 6. Consultas

Foram Consideradas 10 consultas em SQL para avaliar a consistência e tempo gasto na execução de cada consulta.

Exemplo de Consulta

![Consulta 10](https://github.com/Protospi/IBD_TP_LOJA_VIRTUAL/blob/main/Consulta_10.png)

Para avaliar o tempo das consultas foi utilizado o comando %timeit da linguagem Python. Esse comando avalia a query fornecendo o tempo médio de resposta para 3 consultas.

## 7. Tecnologias

As tecnologias utilizadas foram:
* <a href = "https://colab.research.google.com/drive/1PeIUI6mdR7XZz9lMiVm0VKuIou6FKXZE#scrollTo=1bgkBvAGpI4U&uniqifier=1"> Colab .ipynb </a>
* <a href = "https://lucid.app/lucidchart/"> LucidCharts </a>
* <a href = "https://app.diagrams.net/"> Diagrams </a>

## 8. Bibliografia

* Aulas da Disciplina Introdução a Bancos de Dados DCC-UFMG
* Slides da Disciplina Introdução a Bancos de Dados DCC-UFMG
* Livro Texto Sistemas de Banco de Dados - Elmasri Navathe - 4ª-Edicao

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1PeIUI6mdR7XZz9lMiVm0VKuIou6FKXZE#scrollTo=1bgkBvAGpI4U&uniqifier=1)

<h4 align="center"> 
	🚧  IBD_TP_LOJA_VIRTUAL 🚀 Em construção...  🚧
</h4>
