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

```
