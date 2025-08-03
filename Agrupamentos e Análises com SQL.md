
# Agrupamentos e Análises com SQL

---

## Para que servem?

Esses recursos permitem **resumir dados** de uma tabela ou de uma consulta, como:

- Contar quantos pedidos um cliente fez;
- Calcular a média de vendas por mês;
- Somar valores de vendas por produto;
- Encontrar o maior ou menor valor em grupos;
- E muito mais.

---

##  Funções de agregação mais usadas

| Função | O que faz |
| --- | --- |
| `COUNT()` | Conta o número de linhas |
| `SUM()` | Soma os valores de uma coluna numérica |
| `AVG()` | Calcula a média dos valores |
| `MAX()` | Retorna o maior valor |
| `MIN()` | Retorna o menor valor |

---

##  GROUP BY

### O que faz?

O `GROUP BY` agrupa os dados com base em uma ou mais colunas e permite aplicar funções de agregação em cada grupo.

---

### 🔹 Exemplo 1: Total de pedidos por cliente

Tabelas:

`pedidos`

| id_pedido | cliente_id | valor |
| --- | --- | --- |
| 1 | 1 | 100.00 |
| 2 | 1 | 150.00 |
| 3 | 2 | 200.00 |

Consulta:

```sql
SELECT cliente_id, COUNT(*) AS total_pedidos, SUM(valor) AS total_gasto
FROM pedidos
GROUP BY cliente_id;
```

Resultado:

| cliente_id | total_pedidos | total_gasto |
| --- | --- | --- |
| 1 | 2 | 250.00 |
| 2 | 1 | 200.00 |

---

##  HAVING

### O que faz?

- O `HAVING` é usado **junto com o GROUP BY** para **filtrar grupos** após a agregação (como se fosse um WHERE para grupos).

---

###  Exemplo 2: Mostrar só clientes que gastaram mais de R$200

```sql
SELECT cliente_id, SUM(valor) AS total_gasto
FROM pedidos
GROUP BY cliente_id
HAVING SUM(valor) > 200;
```

Resultado:

| cliente_id | total_gasto |
| --- | --- |
| 1 | 250.00 |

---

##  Exemplo 3: Média de vendas por produto

Tabelas:

`vendas`

| id | produto_id | valor |
| --- | --- | --- |
| 1 | 101 | 50.00 |
| 2 | 101 | 80.00 |
| 3 | 102 | 60.00 |

Consulta:

```sql
SELECT produto_id, AVG(valor) AS media_valor
FROM vendas
GROUP BY produto_id;
```

Resultado:

| produto_id | media_valor |
| --- | --- |
| 101 | 65.00 |
| 102 | 60.00 |

---

##  Dica: ORDER BY com GROUP BY

Você pode ordenar os resultados depois de agrupá-los.

```sql
SELECT cliente_id, SUM(valor) AS total
FROM pedidos
GROUP BY cliente_id
ORDER BY total DESC;
```

---

##  Diferença entre WHERE e HAVING

| WHERE | HAVING |
| --- | --- |
| Filtra **linhas** | Filtra **grupos** |
| Usado **antes** do GROUP BY | Usado **depois** do GROUP BY |
| Não pode usar agregações | Pode usar agregações (`SUM()`, etc.) |

---

##  Resumo

- **GROUP BY**: Agrupa dados por uma ou mais colunas.
- **Funções de agregação**: Usadas com GROUP BY para contar, somar, calcular média, etc.
- **HAVING**: Filtra os resultados agrupados.
- **ORDER BY**: Organiza os dados após o agrupamento.
