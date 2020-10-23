# SQL - Question Junior

41

The HAVING clause is evaluated before the SELECT - so the server doesn't yet know about that alias.

1) First, the product of all tables in the FROM clause is formed.

2) The WHERE clause is then evaluated to eliminate rows that do not satisfy the search_condition.

3) Next, the rows are grouped using the columns in the GROUP BY clause.

4) Then, groups that do not satisfy the search_condition in the HAVING clause are eliminated.

5) Next, the expressions in the SELECT statement target list are evaluated.

6) If the DISTINCT keyword in present in the select clause, duplicate rows are now eliminated.

7) The UNION is taken after each sub-select is evaluated.

8) Finally, the resulting rows are sorted according to the columns specified in the ORDER BY clause.

9) TOP clause is executed.



1) Determine the number of duplicates in a table.

```sql
SELECT COLLUN1, COLLUN2, COUNT(*)
FROM TABLE
GROUP BY COLLUN1, COLLUN2
HAVING COUNT(*) > 1;
```

2) Find all unique combination of two rows.

```sql
SELECT COLLUN1, COLLUN2, COUNT(*)
FROM TABLE
GROUP BY COLLUN1, COLLUN2
HAVING COUNT(*) = 1;
```

3) Count the number of non-null entries in a column

```sql
SELECT COUNT(column_name)
FROM table_name
WHERE column_name is NOT NULL;
```

4) When to use group by vs. distinct.
Group by é usado de duas formas ou quando é passado uma função de agregação no SELECT e uma coluna, então necessita agrupar a coluna. E, quando é utilizado para ver o resultado agrupado.

https://pt.stackoverflow.com/questions/228294/distinct-e-group-by-qual-a-diferen%C3%A7a-entre-ambas-as-declara%C3%A7%C3%B5es

OBS.:
# 3.2 Quais produtos cada vendedor vendeu?
Essa requisição envolve o uso de duas colunas: NomeVendedor e ProdutoVendido.

-- código #5
SELECT NomeVendedor, ProdutoVendido FROM VENDAS;
No resultado percebe-se que o par {João, Macarrão} aparece mais de uma vez. Eis outra aplicação típica do uso de DISTINCT, mas agora atuando sobre duas colunas.

-- código #5a
SELECT DISTINCT NomeVendedor, ProdutoVendido FROM VENDAS;
# Importante: DISTINCT atua simultaneamente nas colunas NomeVendedor e ProdutoVendido. Considera as duas colunas para eliminar as repetições.

# 3.5 Quantos produtos diferentes foram vendidos por cada vendedor?
Para contar quantos produtos diferentes foram vendidos por cada vendedor a função de agregação COUNT é a ideal

-- código #8
SELECT NomeVendedor, Count(ProdutoVendido)
FROM Vendas
GROUP BY NomeVendedor;

Antonio 1
João 3
José 1
Maria 1
Ao consultar o resultado, e comparar com o conteúdo da tabela de Vendas, percebemos que para o Vendedor João foram contabilizados 3 produtos quando ele somente vendeu dois tipos de produtos: Macarrão e Molho de tomate. Mas ele fez duas vendas de macarrão. Como fazer para que a função de agregação COUNT somente some uma vez cada produto? A resposta está no uso de DISTINCTdentro do parâmetro da função, conforme

COUNT ( { [ [ ALL | DISTINCT ] expression ] | * } )


-- código #8a
SELECT NomeVendedor, COUNT(DISTINCT ProdutoVendido)
FROM Vendas
GROUP BY NomeVendedor;

Antonio 1
João 2
José 1
Maria 1


5) Why use coalesce vs case statements?

Função SQL COALESCE vs CASE e ISNULL
 


    A função Coalesce está presente no SQL Server desde a versão 2008. Funciona como a instrução CASE e a função ISNULL . Ele avalia os argumentos na ordem e retorna o valor atual da primeira expressão que inicialmente não tem valor NULL.

COALESCE (expressão, [, expressões])

A função Coalesce retorna a primeira expressão na lista de expressões que não é NULL. Vejamos o exemplo a seguir para entendê-lo melhor. Tudo o que estamos dizendo aqui é, retorne o valor de RetailPrice se não for nulo, se RetailsPrice for nulo, então retorne InvoicePrice se seu valor não for nulo. Se isso também falhar, retorne 0.



Podemos converter o exemplo COALESCE anterior para a instrução CASE e ele funcionará da mesma maneira. O COALESCE é, na verdade , um atalho de instruções CASE .


        E sobre a função ISNULL , ela faz algo muito semelhante ao exemplo também. A única diferença é que a   função ISNULL testa apenas 1 valor. Se o valor de teste for NULL, ele retorna o que você decidir que ele retorne. Se o preço de varejo for nulo, ele retornará 0





      Tenho certeza de que você está se perguntando agora. Qual é melhor ou qual é mais rápido que o outro? Existem muitos blogs comparando o desempenho dessas funções. Na minha experiência, não há diferença significativa de desempenho entre eles. Se você precisar comparar apenas um valor, eu escolheria a função ISNULL (). Se você precisa comparar mais de uma expressão, pode usar COALESCE, pois precisa escrever menos código. Também não há nada de errado em usar a instrução CASE.



6)  When would you choose a left join over an inner join?

```sql

```

7) In what cases is a subquery a bad idea?

Adaptive Server Enterprise 15.7 ESD #2 > Transact-SQL Users Guide > Subqueries: Using Queries Within Other Queries > How subqueries work
How subqueries work  Example of using a subquery

Chapter 5: Subqueries: Using Queries Within Other Queries
Subquery restrictions
A subquery is subject to these restrictions:

The subquery_select_list can consist of only one column name, except in the exists subquery, where an (*) is usually used in place of the single column name. You can use an asterisk (*) in a nested select statement that is not an exists subquery.

Do not specify more than one column name. Qualify column names with table or view names if there is ambiguity about the table or view to which they belong.

Subqueries can be nested inside the where or having clause of an outer select, insert, update, or delete statement, inside another subquery, or in a select list. Alternatively, you can write many statements that contain subqueries as joins; Adaptive Server processes such statements as joins.

In Transact-SQL, a subquery can appear almost anywhere an expression can be used, if it returns a single value. SQL derived tables can be used in the from clause of a subquery wherever the subquery is used

You cannot use subqueries in an order by, group by, or compute by list.

You cannot include a for browse clause in a subquery.

You cannot include a union clause in a subquery unless it is part of a derived table expression within the subquery. For more information on using SQL derived tables, see Chapter 9, “SQL-Derived Tables.”

The select list of an inner subquery introduced with a comparison operator can include only one expression or column name, and the subquery must return a single value. The column you name in the where clause of the outer statement must be join-compatible with the column you name in the subquery select list.

You cannot include text, unitext, or image datatypes in subqueries.

Subqueries cannot manipulate their results internally, that is, a subquery cannot include the order by clause, the compute clause, or the into keyword.

Correlated (repeating) subqueries are not allowed in the select clause of an updatable cursor defined by declare cursor.

There is a limit of 50 nesting levels.

The maximum number of subqueries on each side of a union is 50.

The where clause of a subquery can contain an aggregate function only if the subquery is in a having clause of an outer query and the aggregate value is a column from a table in the from clause of the outer query.

The result expression from a subquery is subject to the same limits as for any expression. The maximum length of an expression is 16K. See Chapter 4, “Expressions, Identifiers, and Wildcard “Characters,” in the Reference Manual: Building Blocks.



8) Why is a temp table a good idea? Bad idea?

https://sqlperformance.com/2016/04/t-sql-queries/knee-jerk-temporary-tables

9) How to filter using a join.

https://mode.com/sql-tutorial/sql-joins-where-vs-on/

10) 
 
https://blog.getdbt.com/one-analysts-guide-for-going-from-good-to-great/#:~:text=The%20single%20most%20important%20way,the%20middle%20of%20your%20query.

https://stackoverflow.com/questions/11169550/is-there-a-performance-difference-between-cte-sub-query-temporary-table-or-ta/11169910
