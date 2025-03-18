## Postgres

Banco de dados Northwind: https://github.com/pthom/northwind_psql

## 1. Criando queries básicas no Postgres
### 1.1 SELECT FROM - Selecionando dados das tabelas

### 1.2. Comentários no PostgreSQL

```sql
-- Comentário

/*
Comentário
*/
```

### 1.3. SELECT AS - Aliasing (renomeando) colunas

```sql
SELECT
	product_id AS "ID_Produto",
	product_name AS nome_produto,
	unit_price AS preco_unitario
FROM
	products AS p;
```

### 1.4. SELECT LIMIT - Limitando a quantidade de linhas de uma query

```sql
SELECT * FROM orders
LIMIT 100;
```

### 1.5. SELECT DISTINCT - Selecionando valores distintos de uma coluna

```sql
SELECT DISTINCT contact_title FROM customers;
```

-------------
## 2. Filtros no Postgres
### 2.1. WHERE - Filtros com textos

```sql
-- 1. Selecione a tabela customers.
-- a) Crie um filtro para que sejam mostrados apenas os clientes com contact_title = 'Owner'.
SELECT * FROM customers
WHERE contact_title = 'Owner';

-- b) Crie um filtro para que sejam mostrados apenas os clientes do pais France.
SELECT * FROM customers
WHERE country = 'France';
```

### 2.2. WHERE - Filtros com números e datas

```sql
-- 2. Selecione a tabela products
-- a) Crie um filtro para que sejam mostrados os produtos com estoque igual a zero (units_in_stock).
SELECT * FROM products
WHERE units_in_stock = 0;

-- b) Crie um filtro para mostrar os produtos com unit_price maior ou igual a 50.
SELECT * FROM products
WHERE unit_price >= 50;

-- 3. Selecione a tabela orders. Crie um filtro para mostrar apenas os pedidos feitos depois do dia '01/01/1998'.
SELECT * FROM orders
WHERE order_date >= '1998-01-01';
```

### 2.3. WHERE - Operadores AND e OR

```sql
-- 1. Selecione a tabela customers.
-- a) Crie um filtro para que sejam mostrados apenas os clientes com contact_title = 'Owner' e do país France.
SELECT * FROM customers
WHERE contact_title = 'Owner' AND country = 'France';

-- b) Crie um filtro para que sejam mostrados apenas os clientes do Mexico ou France.
SELECT * FROM customers
WHERE country = 'Mexico' OR country = 'France';
```

### 2.4. WHERE e LIKE - Filtros especiais com textos

```sql
-- 1. Selecione a tabela products.
-- a) Quais são os produtos medidos em boxes?
SELECT * FROM products
WHERE quantity_per_unit LIKE '%boxes%';

-- b) Quais são os produtos medidos em ml?
SELECT * FROM products
WHERE quantity_per_unit LIKE '%ml%';
```

### 2.5. WHERE e IN - Uma alternativa aos múltiplos ORs

```sql
-- 1. Selecione a tebla customers. Quais clientes são dos países Mexico, UK, Canada?

```

### 2.6. WHERE e BETWEEN - Filtrando intervalos

```sql
-- 1. Selecione a tabela products. Quais produtos tem o unit_price entre 50 e 100?
SELECT * FROM products
WHERE unit_price BETWEEN 50 AND 100;

-- 2. Selecione a tabela orders. Quais pedidos foram feitos entre 01/01/1997 e 31/12/1997?
SELECT * FROM orders
WHERE order_date BETWEEN '1997-01-01' AND '1997-12-31';
```

--------------------
## 3. Funções de Agregação e Agrupamentos no Postgres
### 3.1. COUNT

```sql
-- 1. a) Descubra a quantidade de clientes.
SELECT COUNT(*) FROM customers;

-- b) Descubra a quantidade de produtos.
SELECT COUNT(*) FROM products;
```

### 3.2. SUM

```sql
-- 2. a) Qual é a soma total de estoque de produtos?
SELECT SUM(units_in_stock) FROM products;

-- b) Qual é a soma total de vendas de produtos?
SELECT SUM(quantity) FROM order_details;
```

### 3.3. AVG, MIN, MAX

```sql
-- Descubra a média, minimo e máximo unit_price da tablea products.
SELECT
	AVG(unit_price) AS preco_medio,
	MIN(unit_price) AS menor_preco,
	MAX(unit_price) AS maior_preco
FROM products;
```

### 3.4. GROUP BY - Criando agrupamentos

```sql
-- 1. Faça um agrupamento para descobrir a quantidade total de clientes por country.
SELECT
	country,
	COUNT(*)
FROM customers
GROUP BY country;

-- 2. Faça um agrupamento para descobrir a quantidade total de clientes por title.
SELECT
	contact_title,
	COUNT(*)
FROM customers
GROUP BY contact_title;

-- 3. Faça um agrupamento para descobrir a soma total de estoque por supplier_id.
SELECT
	supplier_id,
	SUM(units_in_stock)
FROM products
GROUP BY supplier_id;

-- 4. Faça um agrupamento para descobrir a média de unit_price por supplier_id.
SELECT
	supplier_id,
	AVG(unit_price)
FROM products
GROUP BY supplier_id;
```

### 3.5. GROUP BY, WHERE e HAVING - Utilizando filtros em agrupamentos

```sql
-- 1. Faça um agrupamento para descobrir a quantidade total de clientes por country. O seu agrupamento deve considerar apenas os clientes de contact_title = 'Owner'.
SELECT
	country,
	COUNT(*)
FROM customers
WHERE contact_title = 'Owner'
GROUP BY country;

-- 2. Faça um agrupamento para descobrir a quantidade total de clientes por country. A query resultante deve conter apenas os países que têm mais de 10 clientes.
SELECT
	country,
	COUNT(*)
FROM customers
GROUP BY country
HAVING COUNT(*) > 10;
```

-----------------
## 4. Joins no Postgres
### 4.3. Exemplo prático 1

```sql
-- Faça um Join entre as tabelas products e order_details. Você deve retornar todas as colunas dessa duas tabelas.
SELECT
	*
FROM order_details
LEFT JOIN products
	ON products.product_id = order_details.product_id;
```

### 4.4. Exemplo prático 2

```sql
-- 1. Avalie como as tabelas products e categories podem se relacionar. Qual coluna as duas têm em comum?
-- Como poderíamos criar uma consulta que retornasse product_id, product_name, category_id, unit_price e category_name?
SELECT
	product_id,
	product_name,
	products.category_id,
	unit_price,
	category_name
FROM products
INNER JOIN categories
	ON products.category_id = categories.category_id;
```

### 4.5. Exemplo prático 3

```sql
-- 2. Avalie como as tabelas customers e orders podem se relacionar. Qual coluna as duas têm em comum? Como poderíamos criar uma consulta que retornasse order_id, customer_id, order_date, contact_name? Será que existem alguns clientes que não fizeram nenhuma compra?
SELECT
	order_id,
	customers.customer_id,
	order_date,
	contact_name
FROM orders
LEFT JOIN customers
	ON orders.customer_id = customers.customer_id;

SELECT DISTINCT customer_id FROM customers; -- 91 clientes
SELECT DISTINCT customer_id FROM orders; -- 89 clientes

-- Faça um join entre as tabelas. Você seria capaz de descobrir qual o nome dos clientes que não fizeram nenhuma compra?
SELECT
	order_id,
	customers.customer_id,
	order_date,
	contact_name
FROM customers
LEFT JOIN orders
	ON orders.customer_id = customers.customer_id
WHERE order_id IS NULL;
```

### 4.6. Join, Group By e Order By - Exemplo prático

```sql
-- 1. Utilize os comandos join, group by e order by para criar um agrupamento que retorne como resultado a quantidade_total (SUM(quantity)) para cada product_name. Ordene o resultado, do maior para o menor, considerando a quantitade_total.
SELECT
	product_name,
	SUM(o.quantity) AS quantidade_total
FROM products AS p
LEFT JOIN order_details AS o
	ON p.product_id = o.product_id
GROUP BY product_name
ORDER BY quantidade_total DESC;
```

### 4.7. Join, Group By, Where e Having - Exemplo prático

```sql
-- Faça o agrupamento da quantidade_total por product_name considerando apenas os produtos da classe Luxo (acima de R$ 80,00).
SELECT
	product_name,
	SUM(o.quantity) AS quantidade_total
FROM products AS p
LEFT JOIN order_details AS o
	ON p.product_id = o.product_id
WHERE p.unit_price >= 80
GROUP BY product_name
ORDER BY quantidade_total DESC;

-- Faça o agrupamento de quantidade_total por product_name considerando apenas os produtos que tiveram uma quantidade_total maior ou igual 1000.

```
