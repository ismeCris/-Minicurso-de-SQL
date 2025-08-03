
---

### 🔑 Chaves, Integridade Referencial e Índices

No contexto de **bancos de dados relacionais**, essas três estruturas são fundamentais para garantir **organização, consistência e performance**.

---

#### 🟡 PRIMARY KEY (Chave Primária)

A **chave primária** é usada para identificar **unicamente** cada linha (registro) em uma tabela. Ela garante que:

- Nenhum valor seja **duplicado**
- Nenhum valor seja **nulo**

> Em outras palavras: cada linha da tabela terá um "RG único".

📌 **Exemplo:**

```sql
CREATE TABLE usuarios (
  id INT PRIMARY KEY,        -- id é a chave primária
  nome VARCHAR(100) NOT NULL
);
```

- Aqui, a coluna `id` será única para cada usuário.
- Se tentar inserir dois usuários com o mesmo `id`, o banco recusará.

---

#### 🔵 FOREIGN KEY (Chave Estrangeira)

A **chave estrangeira** é usada para **ligar** duas tabelas. Ela cria uma **relação entre registros** garantindo que os dados estejam **coerentes** com outras tabelas.

> Exemplo: Um pedido sempre precisa estar vinculado a um usuário existente.

📌 **Exemplo:**

```sql
CREATE TABLE pedidos (
  id INT PRIMARY KEY,
  usuario_id INT,
  FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);
```

- Aqui, `usuario_id` é uma **chave estrangeira** que aponta para `usuarios.id`.
- Se alguém tentar inserir um pedido com `usuario_id = 999` (e esse usuário não existir), o banco **não permitirá**.

---

#### 🟢 Integridade Referencial

É o **conjunto de regras** que garante que as relações entre tabelas sejam **válidas**.

- Evita registros "órfãos", como um pedido que aponta para um usuário que não existe.
- Quando usamos `FOREIGN KEY`, o banco aplica automaticamente essas regras.

📌 **Exemplo Prático:**

```sql
-- Tenta inserir um pedido com um usuario_id inexistente
INSERT INTO pedidos (id, usuario_id) VALUES (1, 999);
-- Resultado: Erro! Violação da integridade referencial
```

---

#### 🔴 INDEX (Índice)

Índices funcionam como **atalhos**: eles aceleram a busca e filtragem de dados em colunas específicas.

> Sem índice: o banco precisa "varrer" todas as linhas  
> Com índice: o banco vai direto ao ponto como se fosse um índice de livro

📌 **Criando um índice simples:**

```sql
CREATE INDEX idx_nome ON usuarios(nome);
```

- Agora, pesquisas com `WHERE nome = 'João'` serão **muito mais rápidas**, especialmente em tabelas grandes.

---

#### 🧠 Dica importante

- **PRIMARY KEY** e **FOREIGN KEY** ajudam na **organização e segurança dos dados**
- **INDEX** melhora a **performance**, mas pode **aumentar o tempo de escrita** (inserção, atualização)

---

📌 **Exemplo completo integrando tudo:**

```sql
-- Tabela de usuários com chave primária e índice
CREATE TABLE usuarios (
  id INT PRIMARY KEY,
  nome VARCHAR(100),
  email VARCHAR(100) UNIQUE
);

-- Criando um índice na coluna nome
CREATE INDEX idx_nome ON usuarios(nome);

-- Tabela de pedidos com chave estrangeira
CREATE TABLE pedidos (
  id INT PRIMARY KEY,
  data DATE,
  usuario_id INT,
  FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);
```

---
```
