### **Guia de consultas para o comando SELECT**

---

### **SELECT simples**

O **comando SELECT** permite recuperar os dados de um objeto do banco de dados, como uma tabela, view e, em alguns casos, uma stored procedure. A sintaxe mais básica do comando é:

```sql
SELECT <lista_de_campos>
FROM <nome_da_tabela>
```

Exemplo:

```sql
SELECT CODIGO, NOME FROM CLIENTES;
SELECT * FROM CLIENTES;
```

O caractere `*` representa todos os campos. Apesar de prático, ele não é muito utilizado, pois para o SGBD é mais rápido receber o comando com todos os campos explicitados. O uso de `*` obriga o servidor a consultar quais são os campos antes de efetuar a busca dos dados, criando mais um passo no processo.

---

### **COMANDO WHERE**

A cláusula `WHERE` permite ao comando SQL passar condições de filtragem.

```sql
SELECT CODIGO, NOME FROM CLIENTES WHERE CODIGO = 10;
SELECT CODIGO, NOME FROM CLIENTES WHERE UF = 'RJ';
SELECT CODIGO, NOME FROM CLIENTES WHERE CODIGO >= 100 AND CODIGO <= 500;
SELECT CODIGO, NOME FROM CLIENTES WHERE UF = 'MG' OR UF = 'SP';
```

Parênteses corretamente utilizados dão mais poder às consultas:

```sql
SELECT CODIGO, NOME FROM CLIENTES
WHERE UF = 'RJ' OR (UF = 'SP' AND ATIVO = 'N');
```

Todos os clientes do Rio de Janeiro e apenas os clientes inativos de São Paulo seriam retornados.

```sql
SELECT CODIGO, NOME FROM CLIENTES
WHERE (ENDERECO IS NULL) OR (CIDADE IS NULL);
```

Clientes sem endereço ou cidade cadastrada serão selecionados.

---

### **FILTRO DE TEXTO**

Para busca parcial de string, o SELECT fornece o operador `LIKE`:

```sql
SELECT CODIGO, NOME FROM CLIENTES
WHERE NOME LIKE 'MARIA%';

SELECT CODIGO, NOME FROM CLIENTES
WHERE NOME LIKE '%MARIA%';
```

O uso de máscara no início e no fim da string fornece maior poder de busca, mas causa perda de performance. Use com critério.

> Em alguns bancos de dados, o caractere `%` pode variar. Consulte a documentação do seu SGBD.

Para eliminar a diferença entre letras maiúsculas e minúsculas, use `UPPER`:

```sql
SELECT CODIGO, NOME FROM CLIENTES
WHERE UPPER(NOME) LIKE '%MARIA SILVA%';
```

---

### **ORDENAÇÃO**

A ordenação pode ser feita com `ORDER BY`. Veja os exemplos:

```sql
SELECT CODIGO, NOME FROM CLIENTES
ORDER BY NOME;

SELECT CODIGO, NOME FROM CLIENTES
ORDER BY UF, NOME;

SELECT CODIGO, NOME FROM CLIENTES
ORDER BY NOME DESC;

SELECT CODIGO, NOME FROM CLIENTES
ORDER BY UF DESC;
```

---

### **JUNÇÃO DE TABELAS**

SELECT permite unir várias tabelas. Exemplo com junção tradicional:

```sql
SELECT CLIENTES.CODIGO, CLIENTES.NOME, PEDIDOS.DATA
FROM CLIENTES, PEDIDOS
WHERE CLIENTES.CODIGO = PEDIDOS.CODCLIENTE;
```

Com aliases:

```sql
SELECT A.CODIGO, A.NOME, B.DATA, B.VALOR
FROM CLIENTES A, PEDIDOS B
WHERE A.CODIGO = B.CODCLIENTE;
```

Com várias tabelas:

```sql
SELECT A.CODIGO, A.NOME, B.DATA, B.VALOR, C.QTD, D.DESCRIC
FROM CLIENTES A, PEDIDOS B, ITENS C, PRODUTOS D
WHERE A.CODIGO = B.CODCLIENTE
AND B.CODIGO = C.CODPEDIDO
AND C.CODPRODUTO = D.CODIGO;
```

Com filtro adicional:

```sql
SELECT A.CODIGO, A.NOME, B.DATA, B.VALOR
FROM CLIENTES A, PEDIDOS B
WHERE A.CODIGO = B.CODCLIENTE
AND A.UF = 'RJ';
```

Caso precise incluir registros sem correspondência, use `JOIN`.

---

### **COMANDO JOIN**

Exemplo com `INNER JOIN`:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO, B.QTD
FROM PRODUTOS A
INNER JOIN COMPONENTES B ON (A.CODIGO = B.CODPRODUTO);
```

Forma resumida:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM PRODUTOS A
JOIN COMPONENTES B ON (A.CODIGO = B.CODPRODUTO);
```

Exemplo com `LEFT OUTER JOIN`:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO, B.QTDE
FROM PRODUTOS A
LEFT OUTER JOIN COMPONENTES B ON (A.CODIGO = B.CODPRODUTO);
```

Inverso com `RIGHT OUTER JOIN`:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM COMPONENTES A
RIGHT OUTER JOIN PRODUTOS B ON (A.CODIGO = B.CODPRODUTO);
```

Com `WHERE` adicional:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO, B.QTDE
FROM PRODUTOS A
JOIN COMPONENTES B ON (A.CODIGO = B.CODPRODUTO)
WHERE A.CATEGORIA = 1;
```

Com `ORDER BY`:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM PRODUTOS A
JOIN COMPONENTES B ON (A.CODIGO = B.CODPRODUTO)
WHERE A.CATEGORIA = 1 OR A.CATEGORIA = 2
ORDER BY A.CATEGORIA, A.DESCRICAO;
```

---

### **FULL OUTER JOIN**

Une todos os registros das duas tabelas, com ou sem correspondência:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM PRODUTOS A
FULL OUTER JOIN COMPONENTES B ON (A.CODIGO = B.CODPRODUTO);
```

> Nem todos os bancos suportam FULL OUTER JOIN diretamente.

---

### **UNION**

Permite combinar o resultado de duas ou mais `SELECTs` verticalmente.

```sql
SELECT CODIGO, NOME FROM CLIENTES
UNION
SELECT CODIGO, NOME FROM FUNCIONARIOS;
```

Para incluir duplicatas:

```sql
SELECT CODIGO, NOME FROM CLIENTES
UNION ALL
SELECT CODIGO, NOME FROM FUNCIONARIOS;
```

---

### **FUNÇÕES DE AGRUPAMENTO**

Funções de agregação comuns:

```sql
SELECT AVG(VALOR) FROM PEDIDOS;
SELECT MIN(VALOR) FROM PEDIDOS;
SELECT MAX(VALOR) FROM PEDIDOS;
SELECT SUM(VALOR) FROM PEDIDOS;
SELECT COUNT(*) FROM CLIENTES;
```

---

### **AGRUPAMENTO COM GROUP BY**

```sql
SELECT CODCLIENTE, MAX(VALOR)
FROM PEDIDOS
GROUP BY CODCLIENTE;

SELECT CODCLIENTE, COUNT(*)
FROM PEDIDOS
GROUP BY CODCLIENTE;
```

---

### **HAVING**

Usado para filtrar resultados após agrupamento:

```sql
SELECT CODCLIENTE, COUNT(*)
FROM PEDIDOS
GROUP BY CODCLIENTE
HAVING COUNT(*) >= 2;
```

Com filtro na `WHERE` antes do agrupamento:

```sql
SELECT CODCLIENTE, COUNT(*)
FROM PEDIDOS
WHERE DATA > '2002-10-06'
GROUP BY CODCLIENTE
HAVING COUNT(*) >= 2;
```

---
