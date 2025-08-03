

## 🔍 Consultas com SELECT
O comando SELECT permite recuperar os dados de um objeto do banco de dados, como uma tabela, view e, em alguns casos, uma stored procedure.

### Sintaxe básica:

```sql
SELECT <campos>
FROM <tabela>;
```

### Exemplos:

```sql
SELECT CODIGO, NOME FROM CLIENTES;
SELECT * FROM CLIENTES;
```

> ⚠️ `*`O caractere * representa todos os campos. Apesar de prático, ele não é muito utilizado, pois para o SGBD é mais rápido receber o comando com todos os campos explicitados. O uso de * obriga o servidor a consultar quais são os campos antes de efetuar a busca dos dados, criando mais um passo no processo.

---

## 🎯 WHERE – Filtros
A cláusula `WHERE` permite ao comando SQL passar condições de filtragem.

```sql
SELECT CODIGO, NOME FROM CLIENTES WHERE CODIGO = 10;
SELECT CODIGO, NOME FROM CLIENTES WHERE UF = 'RJ';
SELECT CODIGO, NOME FROM CLIENTES WHERE CODIGO >= 100 AND CODIGO <= 500;
SELECT CODIGO, NOME FROM CLIENTES WHERE UF = 'MG' OR UF = 'SP';
```

Com parênteses:

```sql
SELECT CODIGO, NOME FROM CLIENTES
WHERE UF = 'RJ' OR (UF = 'SP' AND ATIVO = 'N');
```

Com nulos:

```sql
SELECT CODIGO, NOME FROM CLIENTES
WHERE ENDERECO IS NULL OR CIDADE IS NULL;
```

---

## 🔠 LIKE – Busca textual
Para busca parcial de string, o SELECT fornece o operador `LIKE`:

```sql
SELECT CODIGO, NOME FROM CLIENTES WHERE NOME LIKE 'MARIA%';
SELECT CODIGO, NOME FROM CLIENTES WHERE NOME LIKE '%MARIA%';
SELECT CODIGO, NOME FROM CLIENTES WHERE UPPER(NOME) LIKE '%MARIA SILVA%';
```

---

## 📊 ORDER BY – Ordenação
A ordenação pode ser feita com `ORDER BY`. Veja os exemplos:

```sql
SELECT CODIGO, NOME FROM CLIENTES ORDER BY NOME;
SELECT CODIGO, NOME FROM CLIENTES ORDER BY UF, NOME;
SELECT CODIGO, NOME FROM CLIENTES ORDER BY NOME DESC;
```

---

## 🔗 Junção entre Tabelas
SELECT permite unir várias tabelas. Exemplo com junção tradicional:

### Sintaxe tradicional:

```sql
SELECT CLIENTES.CODIGO, CLIENTES.NOME, PEDIDOS.DATA
FROM CLIENTES, PEDIDOS
WHERE CLIENTES.CODIGO = PEDIDOS.CODCLIENTE;
```

### Com aliases:

```sql
SELECT A.CODIGO, A.NOME, B.DATA, B.VALOR
FROM CLIENTES A, PEDIDOS B
WHERE A.CODIGO = B.CODCLIENTE;
```

---

## 🔁 JOIN

### INNER JOIN:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO, B.QTD
FROM PRODUTOS A
INNER JOIN COMPONENTES B ON A.CODIGO = B.CODPRODUTO;
```

### LEFT JOIN:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM PRODUTOS A
LEFT OUTER JOIN COMPONENTES B ON A.CODIGO = B.CODPRODUTO;
```

### RIGHT JOIN:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM COMPONENTES A
RIGHT OUTER JOIN PRODUTOS B ON A.CODIGO = B.CODPRODUTO;
```

### FULL OUTER JOIN:

```sql
SELECT A.CODIGO, A.DESCRICAO, B.DESCRICAO
FROM PRODUTOS A
FULL OUTER JOIN COMPONENTES B ON A.CODIGO = B.CODPRODUTO;
```

> Nem todos os bancos suportam FULL OUTER JOIN.

---

## 🔗 UNION

```sql
SELECT CODIGO, NOME FROM CLIENTES
UNION
SELECT CODIGO, NOME FROM FUNCIONARIOS;
```

### UNION ALL:

```sql
SELECT CODIGO, NOME FROM CLIENTES
UNION ALL
SELECT CODIGO, NOME FROM FUNCIONARIOS;
```

---

## 📊 Funções de Agregação

```sql
SELECT AVG(VALOR) FROM PEDIDOS;
SELECT MIN(VALOR) FROM PEDIDOS;
SELECT MAX(VALOR) FROM PEDIDOS;
SELECT SUM(VALOR) FROM PEDIDOS;
SELECT COUNT(*) FROM CLIENTES;
```

---

## 📦 GROUP BY

```sql
SELECT CODCLIENTE, MAX(VALOR)
FROM PEDIDOS
GROUP BY CODCLIENTE;
```

```sql
SELECT CODCLIENTE, COUNT(*)
FROM PEDIDOS
GROUP BY CODCLIENTE;
```

---

## 🧾 HAVING

```sql
SELECT CODCLIENTE, COUNT(*)
FROM PEDIDOS
GROUP BY CODCLIENTE
HAVING COUNT(*) >= 2;
```

Com `WHERE` e `HAVING`:

```sql
SELECT CODCLIENTE, COUNT(*)
FROM PEDIDOS
WHERE DATA > '2002-10-06'
GROUP BY CODCLIENTE
HAVING COUNT(*) >= 2;
```

---

## ✅ Conclusão

Com este guia, você já pode aplicar comandos SQL para:

- Consultar dados com SELECT
- Filtrar e ordenar registros
- Relacionar tabelas com JOIN
- Usar funções de agregação e agrupar resultados
- Controlar permissões e transações

Pratique os exemplos e adapte para suas próprias tabelas! 🚀

---


