
---

### O que é JOIN?

O **JOIN** é uma operação em SQL usada para combinar dados de duas ou mais tabelas, relacionando as linhas entre elas através de colunas comuns. É essencial quando queremos consultar dados espalhados em tabelas diferentes que possuem relação.

---

### Para que serve o JOIN?

- Trazer informações relacionadas de tabelas diferentes;
- Evitar duplicação de dados;
- Montar relatórios e consultas mais completas e úteis.

---

### Tipos mais comuns de JOIN

### 1. INNER JOIN

- Retorna só os registros que têm correspondência em **ambas** as tabelas.

**Exemplo:**

Tabelas:

`clientes`

| id_cliente | nome |
| --- | --- |
| 1 | Ana |
| 2 | Bruno |
| 3 | Carla |

`pedidos`

| id_pedido | id_cliente | produto |
| --- | --- | --- |
| 101 | 1 | Teclado |
| 102 | 2 | Monitor |
| 103 | 4 | Mouse |

Consulta:

```sql
SELECT clientes.nome, pedidos.produto
FROM clientes
INNER JOIN pedidos ON clientes.id_cliente = pedidos.id_cliente;
```

Resultado:

| nome | produto |
| --- | --- |
| Ana | Teclado |
| Bruno | Monitor |

---

### 2. LEFT JOIN (LEFT OUTER JOIN)

- Retorna **todos os registros da tabela da esquerda** e os correspondentes da tabela da direita.
- Se não houver correspondência, os campos da direita aparecem como NULL.

**Exemplo:**

```sql
SELECT clientes.nome, pedidos.produto
FROM clientes
LEFT JOIN pedidos ON clientes.id_cliente = pedidos.id_cliente;
```

Resultado:

| nome | produto |
| --- | --- |
| Ana | Teclado |
| Bruno | Monitor |
| Carla | NULL |

---

### 3. RIGHT JOIN (RIGHT OUTER JOIN)

- Retorna **todos os registros da tabela da direita** e os correspondentes da tabela da esquerda.
- Se não houver correspondência, os campos da esquerda aparecem como NULL.

---

### 4. FULL JOIN (FULL OUTER JOIN)

- Retorna **todos os registros de ambas as tabelas**.
- Onde não há correspondência, mostra NULL nos campos da outra tabela.
- Nem todos os bancos suportam FULL JOIN diretamente.

---

### 5. CROSS JOIN

- Retorna o **produto cartesiano** entre as duas tabelas, ou seja, todas as combinações possíveis entre as linhas da primeira e da segunda tabela.
- O número de registros resultantes é o número de linhas da tabela 1 multiplicado pelo número da tabela 2.

**Exemplo:**

Tabelas:

`tamanhos`

| tamanho |
| --- |
| P |
| M |
| G |

`cores`

| cor |
| --- |
| Azul |
| Verde |

Consulta:

```sql
SELECT tamanhos.tamanho, cores.cor
FROM tamanhos
CROSS JOIN cores;
```

Resultado (3 x 2 = 6 linhas):

| tamanho | cor |
| --- | --- |
| P | Azul |
| P | Verde |
| M | Azul |
| M | Verde |
| G | Azul |
| G | Verde |

---

### Diferença importante

- **INNER JOIN** e os outros JOINs combinam tabelas com base em uma condição de relacionamento (via `ON`).
- **CROSS JOIN** não usa condição, combina tudo com tudo, gerando todas as combinações possíveis.

---

### Resumo dos JOINs

| Tipo JOIN | O que retorna? |
| --- | --- |
| INNER JOIN | Só registros com correspondência em ambas tabelas |
| LEFT JOIN | Todos da esquerda + correspondentes da direita |
| RIGHT JOIN | Todos da direita + correspondentes da esquerda |
| FULL JOIN | Todos registros de ambas tabelas |
| CROSS JOIN | Produto cartesiano: todas as combinações possíveis |
