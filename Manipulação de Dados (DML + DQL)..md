# 🧑‍💻 Comandos SQL na Prática

---

## 📌 DDL – Estrutura de Tabelas

DDL (Data Definition Language) é o conjunto de comandos SQL usados para definir e modificar a estrutura de objetos do banco de dados, como tabelas, colunas e índices.

### ➕ Criar Tabela

```sql
CREATE TABLE produtos (
  id INT PRIMARY KEY,
  nome VARCHAR(100),
  preco DECIMAL(10,2)
);
```

### 🛠️ Alterar Tabela

```sql
ALTER TABLE produtos ADD estoque INT;
```

### ❌ Deletar Tabela

```sql
DROP TABLE produtos;
```

---

## 🔑 Chaves, Integridade e Índices

### PRIMARY KEY
- Identifica de forma única cada linha de uma tabela.
- Não pode conter valores nulos.
- Cria um índice automaticamente.

### FOREIGN KEY
- Cria um vínculo entre duas tabelas.
- Garante que o valor inserido exista na tabela referenciada.

### Integridade Referencial
- Impede a inserção de dados órfãos.
- Garante consistência nas relações entre tabelas.

### Índices
- Aceleram as consultas (`SELECT`) em colunas específicas.
- São úteis em colunas muito consultadas ou com buscas frequentes.

#### 📌 Exemplo de uso:

```sql
-- Tabela de usuários com chave primária
CREATE TABLE usuarios (
  id INT PRIMARY KEY,
  nome VARCHAR(100)
);

-- Tabela de pedidos com chave estrangeira
CREATE TABLE pedidos (
  id INT PRIMARY KEY,
  usuario_id INT,
  FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);

-- Criar índice para otimizar buscas pelo nome
CREATE INDEX idx_nome ON usuarios(nome);
```

---

## 🧩 Manipulação de Dados (DML + DQL)

DML (Data Manipulation Language) e DQL (Data Query Language) lidam com os dados contidos nas tabelas, permitindo inserções, atualizações, exclusões e consultas.

### ➕ INSERT – Inserir dados

```sql
INSERT INTO produtos (id, nome, preco) 
VALUES (1, 'Camiseta', 49.90);
```

### 📝 UPDATE – Atualizar dados

```sql
UPDATE produtos 
SET preco = 59.90 
WHERE id = 1;
```

### ❌ DELETE – Remover dados

```sql
DELETE FROM produtos 
WHERE id = 1;
```

### 🔍 SELECT – Consultar dados

```sql
SELECT * FROM produtos;

SELECT nome, preco 
FROM produtos 
WHERE preco > 50;
```

### 🔄 INSERT com SELECT – Inserir dados baseados em consulta

```sql
INSERT INTO produtos_backup (id, nome, preco)
SELECT id, nome, preco 
FROM produtos 
WHERE preco > 100;
```

### 🔧 UPDATE avançado

```sql
UPDATE produtos 
SET estoque = estoque - 1 
WHERE id = 1 AND estoque > 0;
```

### ⚠️ DELETE com segurança

```sql
DELETE FROM produtos 
WHERE preco < 10;
```

> Sempre use `WHERE` com `DELETE` ou `UPDATE` para evitar alterações em massa indesejadas!

---
