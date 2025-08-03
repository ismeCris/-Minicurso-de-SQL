## 🧑‍💻 Comandos SQL na Prática  
### 📌 DDL – Estrutura de Tabelas

A DDL (Data Definition Language) é usada para definir e modificar a estrutura dos objetos do banco de dados, como tabelas, índices e relacionamentos.

---

### 🧱 `CREATE TABLE` – Criação de Tabela

Cria uma nova tabela no banco de dados.

```sql
-- Cria a tabela de produtos com chave primária
CREATE TABLE produtos (
  id INT PRIMARY KEY,             -- identificador único
  nome VARCHAR(100),              -- nome do produto
  preco DECIMAL(10,2)             -- preço com até 10 dígitos, 2 casas decimais
);
```

---

### 🛠️ `ALTER TABLE` – Alteração de Tabela

Modifica a estrutura de uma tabela já existente. Pode adicionar, modificar ou remover colunas, chaves etc.

```sql
-- Adiciona uma nova coluna chamada 'estoque' à tabela produtos
ALTER TABLE produtos ADD estoque INT;

-- Modifica o tipo da coluna preco
ALTER TABLE produtos MODIFY preco DECIMAL(12,2);

-- Remove a coluna estoque
ALTER TABLE produtos DROP COLUMN estoque;
```

---

### 🧨 `DROP TABLE` – Exclusão de Tabela

Apaga completamente a tabela e todos os seus dados.

```sql
-- Exclui a tabela produtos do banco de dados
DROP TABLE produtos;
```

> ⚠️ Atenção: `DROP TABLE` é irreversível. Todos os dados serão perdidos!

---

### ✅ Dica: Criação com mais detalhes

```sql
-- Exemplo com chave estrangeira e restrições
CREATE TABLE pedidos (
  id INT PRIMARY KEY,
  produto_id INT,
  quantidade INT CHECK (quantidade > 0),
  data_pedido DATE DEFAULT CURRENT_DATE,
  FOREIGN KEY (produto_id) REFERENCES produtos(id)
);
```

---

### 📎 Resumo dos Comandos DDL

| Comando       | Finalidade                             |
|---------------|----------------------------------------|
| `CREATE TABLE`| Cria uma nova tabela                   |
| `ALTER TABLE` | Modifica estrutura de uma tabela       |
| `DROP TABLE`  | Apaga uma tabela do banco              |
| `CREATE INDEX`| Cria um índice para otimizar buscas    |

