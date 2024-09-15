# Notas e desenvolvimento do estudo - SQL

### **Resumo sobre Bancos de Dados**

### **SQL (Structured Query Language)**

- **Definição:** SQL é uma linguagem utilizada para gerenciar e manipular bancos de dados relacionais. Ela organiza e armazena informações de um domínio específico, geralmente em tabelas com linhas e colunas.
- **Tipos de SGBD Comuns:** Oracle, DB2, MySQL, SQL Server, PostgreSQL.
- **Características dos Bancos de Dados Relacionais:**
    - **Estrutura:** Baseado em tabelas, onde cada tabela tem uma chave primária única para identificar cada linha. Essas chaves podem relacionar tabelas entre si.
    - **Vantagens:**
        - Rapidez na manipulação e acesso de informações.
        - Reduz o esforço humano para desenvolvimento e uso.
        - Diminui a redundância e inconsistência de dados.
        - Facilita o compartilhamento de dados.
        - Aplica automaticamente restrições de segurança.
        - Melhora a integridade dos dados (reduz problemas de inconsistência e erro nos dados).
    - **Desvantagens:**
        - Pode ser caro.
        - Requer uma equipe especializada para gerenciamento.
        - Pode ter problemas de redundância de dados.

### **NoSQL (Not Only SQL)**

- **Definição:** NoSQL refere-se a bancos de dados não relacionais que são otimizados para desempenho e escalabilidade horizontal. Não utilizam SQL como a linguagem principal para consultas.
- **Tipos de Bancos de Dados NoSQL:**
    1. **Documento:**
        - **Definição:** Armazena dados em documentos, geralmente no formato JSON ou BSON.
        - **Exemplo:** MongoDB.
        - **Exemplo de Código (JSON):**
    
    ```sql
    {
      "id": "12345",
      "nome": "João Silva",
      "email": "joao.silva@example.com",
      "senha": "senhaFicticia123"
    }
    ```
    

**Chave-Valor:**

- **Definição:** Armazena dados como pares de chave e valor. É eficiente para grandes volumes de dados e operações rápidas.
- **Exemplo:** Redis.
- **Exemplo de Código:**
    
    ```sql
    chave: "user:12345"
    valor: {
      "nome": "João Silva",
      "email": "joao.silva@example.com",
      "senha": "senhaFicticia123"
    }
    ```
    

**Colunas:**

- **Definição:** Armazena dados em colunas e famílias de colunas, em vez de linhas. É útil para grandes volumes de dados distribuídos.
- **Exemplo:** Apache Cassandra.
- **Exemplo de Estrutura:**
    
    ```sql
    Tabela: Users
    Coluna Família: Info
      - Coluna: id
      - Coluna: nome
      - Coluna: email
      - Coluna: senha
    ```
    

**Grafos:**

- **Definição:** Armazena dados em forma de grafos, com nós (entidades) e arestas (relações). Ideal para dados com muitas relações complexas.
- **Exemplo:** Neo4j.
- **Explicação:** Os bancos de dados de grafos utilizam a estrutura de grafos para representar e analisar relações entre dados. Isso é útil em situações como redes sociais, onde é necessário entender as conexões e interações entre os usuários.

### **Comandos DDL (Data Definition Language)**

Os comandos DDL são usados para definir e modificar a estrutura de um banco de dados, como a criação, alteração ou exclusão de tabelas. Eles não manipulam os dados em si, mas sim a forma como eles são armazenados.

### **Exemplo de Criação de Tabela com Diversos Tipos de Dados**

```sql
CREATE TABLE pessoa (
    id BIGINT PRIMARY KEY,  -- Número inteiro de 8 bytes, chave primária
    nome VARCHAR(50) NOT NULL,  -- Cadeia de caracteres de tamanho variável até 50, valor obrigatório
    endereco TEXT,  -- Texto sem limite definido de tamanho
    telefone INTEGER,  -- Número inteiro de 4 bytes
    salario DECIMAL(10, 2),  -- Decimal com até 10 dígitos, sendo 2 após a vírgula
    altura REAL,  -- Número de ponto flutuante de 4 bytes
    peso DOUBLE PRECISION,  -- Número de ponto flutuante de 8 bytes
    nascimento DATE,  -- Apenas data (sem hora)
    ultima_atualizacao TIMESTAMP WITH TIME ZONE,  -- Data e hora com fuso horário
    duracao_trabalho INTERVAL,  -- Intervalo de tempo
    ativo BOOLEAN  -- Valores booleanos (TRUE/FALSE)
);
```

### **Tipos de Dados**

Aqui estão os principais tipos de dados que podem ser usados na criação de tabelas:

1. **Números inteiros**:
    - SMALLINT: 2 bytes, faixa de -32.768 a 32.767.
    - INTEGER: 4 bytes, faixa de -2.147.483.648 a 2.147.483.647.
    - BIGINT: 8 bytes, permite números muito grandes.
        
        Exemplo:
        
    
    ```sql
    idade SMALLINT,
    numero_conta INTEGER,
    patrimonio BIGINT
    ```
    

**Números fracionários**:

- DECIMAL(precision, scale) ou NUMERIC(precision, scale): Usados para valores com precisão fixa, como valores monetários.
- REAL: 4 bytes, até 6 dígitos de precisão.
- DOUBLE PRECISION: 8 bytes, até 15 dígitos de precisão.

Exemplo:

```sql
idade SMALLINT,
numero_conta INTEGER,
patrimonio BIGINT
```

**Cadeias de caracteres**:

- VARCHAR(n) ou CHARACTER VARYING(n): Cadeia de caracteres de tamanho variável até n.
- CHAR(n) ou CHARACTER(n): Cadeia de tamanho fixo, preenchida com espaços.
- TEXT: Texto de tamanho ilimitado.

Exemplo:

```sql
nome VARCHAR(100),
descricao TEXT,
codigo CHAR(10)
```

**Datas e Horas**:

- DATE: Apenas a data.
- TIME [WITH/WITHOUT TIME ZONE]: Hora, com ou sem fuso horário.
- TIMESTAMP [WITH/WITHOUT TIME ZONE]: Data e hora completas.
- INTERVAL: Representa intervalos de tempo (ex.: "1 mês", "2 horas").

Exemplo:

```sql
nascimento DATE,
ultima_atualizacao TIMESTAMP WITH TIME ZONE,
duracao_projeto INTERVAL
```

**Booleanos**:

- BOOLEAN: Aceita valores TRUE, FALSE, ou equivalentes como 1 (TRUE) e 0 (FALSE).

Exemplo:

```sql
ativo BOOLEAN
```

### **Comandos de Alteração de Tabela (DDL)**

- **Adicionar coluna**:
    
    ```sql
    ALTER TABLE produto ADD COLUMN preco DECIMAL(10, 2);
    ```
    

**Modificar uma coluna** (definir valor padrão ou tornar obrigatória):

```sql
ALTER TABLE pessoa ALTER COLUMN ativo SET DEFAULT TRUE;
ALTER TABLE pessoa ALTER COLUMN nome SET NOT NULL;
```

**Renomear coluna ou tabela**:

```sql
ALTER TABLE pessoa RENAME COLUMN telefone TO contato;
ALTER TABLE pessoa RENAME TO pessoas_registradas;
```

**Excluir todos os dados de uma tabela (TRUNCATE)**:

```sql
TRUNCATE TABLE produto;
```

**Excluir tabela**:

```sql
DROP TABLE produto;
```

### **Comandos DML (Data Manipulation Language)**

Esses comandos são utilizados para manipulação direta dos dados.

### **Inserção de dados**:

```sql
INSERT INTO produto (id, nome, quantidade, data_entrada, data_validade, preco)
VALUES (1, 'Caixas de Morangos', 5, '2024-06-09', '2024-09-16', 5.00);

INSERT INTO produto (id, nome, quantidade, data_entrada, data_validade, preco)
VALUES (2, 'Caixas de Uvas', 50, '2024-06-09', '2024-09-26', 8.00);
```

### **Atualização de dados**:

```sql
UPDATE produto SET quantidade = 50 WHERE id = 1;
```

### **Exclusão de dados**:

```sql
DELETE FROM produto WHERE id = 1;
```

### **Consultas de dados** (DQL):

- Selecionar todos os dados:
    
    ```sql
    SELECT * FROM produto;
    ```
    
- Selecionar todos os dados:
    
    ```sql
    SELECT * FROM produto WHERE preco > 5.00;
    ```
    
- Ordenação dos resultados:
    
    ```sql
    SELECT * FROM produto ORDER BY nome DESC;
    ```
    

### Índices

Um índice é uma estrutura de dados usada para melhorar a velocidade das operações de consulta em uma tabela. Ele permite que o banco de dados localize rapidamente as linhas sem precisar varrer toda a tabela.

### Sintaxe para criar um índice:

```sql
CREATE INDEX index_name ON table_name(column_name);
```

### Exemplo:

```sql
-- Criação de índice para otimizar a busca por CPF
CREATE INDEX idx_cpf ON tb_pessoa(cpf);

-- Consulta otimizada
SELECT * FROM tb_pessoa WHERE cpf = 123456;
```

### Constraints (Restrições)

As constraints são regras de validação que podem ser aplicadas a colunas ou a tabelas inteiras. Elas garantem a integridade e a consistência dos dados.

### Tipos de Key Constraints:

1. **Primary Key**: Garante que a coluna ou conjunto de colunas é único e não nulo.
2. **Foreign Key**: Garante que o valor de uma coluna ou conjunto de colunas exista em outra tabela.

### Exemplo de **Primary Key**:

```sql
CREATE TABLE tb_estado (
    id BIGINT NOT NULL,
    nome VARCHAR(50) NOT NULL,
    CONSTRAINT pk_id_estado PRIMARY KEY(id)
);
```

- A PRIMARY KEY impõe que o valor da coluna id seja único e não nulo.

### Exemplo de **Foreign Key**:

```sql
CREATE TABLE tb_pessoa (
    id BIGINT NOT NULL,
    nome VARCHAR(50) NOT NULL,
    id_estado BIGINT,
    CONSTRAINT pk_id_pessoa PRIMARY KEY(id),
    CONSTRAINT fk_id_estado_pessoa FOREIGN KEY(id_estado) REFERENCES tb_estado(id)
);
```

- A FOREIGN KEY garante que os valores de id_estado na tabela tb_pessoa estejam presentes como id na tabela tb_estado.

### Explicação da Relação:

A tabela tb_pessoa se relaciona com a tabela tb_estado através de id_estado. Isso garante que toda pessoa tenha um estado válido e cadastrado. Se você realizar uma consulta na tabela tb_pessoa, poderá obter o estado associado através dessa chave estrangeira.

### Normalização de Tabelas

A normalização é o processo de organizar os dados para eliminar redundâncias e garantir a integridade. A ideia é quebrar os dados em tabelas menores e conectá-las por chaves primárias e estrangeiras.

### Exemplo de tabelas normalizadas:

```sql
CREATE TABLE tb_pessoa (
    id BIGINT NOT NULL,
    nome VARCHAR(50) NOT NULL,
    idade INTEGER CHECK (idade > 0 AND idade < 120),
    id_estado BIGINT,
    CONSTRAINT pk_id_pessoa PRIMARY KEY(id),
    CONSTRAINT fk_id_estado_pessoa FOREIGN KEY(id_estado) REFERENCES tb_estado(id)
);

CREATE TABLE tb_estado (
    id BIGINT NOT NULL,
    nome VARCHAR(50) NOT NULL,
    sigla VARCHAR(2) NOT NULL,
    CONSTRAINT pk_id_estado PRIMARY KEY(id)
);
```

### Colum Constraints

### **NOT NULL**

Garante que a coluna não pode ter valores nulos.

```sql
CREATE TABLE tb_pessoa (
    id BIGINT NOT NULL,
    nome VARCHAR(50) NOT NULL
);
```

### **DEFAULT**

Define um valor padrão para uma coluna.

```sql
CREATE TABLE tb_produto (
    id BIGINT NOT NULL,
    preco DECIMAL(10, 2) DEFAULT 0.00
);
```

### **CHECK**

Impoe uma condição que os valores da coluna devem satisfazer.

```sql
CREATE TABLE tb_pessoa (
    idade INTEGER CHECK (idade > 0 AND idade < 120)
);
```

### **UNIQUE**

Garante que os valores da coluna sejam únicos, mas diferente da PRIMARY KEY, pode permitir valores nulos.

```sql
CREATE TABLE tb_pessoa (
    cpf BIGINT,
    CONSTRAINT uq_cpf UNIQUE(cpf)
);
```

### Sequence

Uma sequence é um objeto que gera valores numéricos sequenciais, frequentemente usado para gerar valores automáticos para colunas id.

### Exemplo:

```sql
CREATE SEQUENCE sq_pessoa
    START WITH 1
    INCREMENT BY 1
    OWNED BY tb_pessoa.id;

-- Utilizando a sequence
INSERT INTO tb_pessoa (id, nome, idade, cpf) VALUES (nextval('sq_pessoa'), 'João', 30, 123456789);
```

O nextval('sq_pessoa') garante que o próximo valor disponível seja atribuído ao id.

### Joins

Os joins são usados para combinar dados de duas ou mais tabelas com base em uma condição relacionada.

### **INNER JOIN**:

Retorna apenas os registros que têm correspondência em ambas as tabelas.

```sql
SELECT p.nome, e.nome
FROM tb_pessoa p
INNER JOIN tb_estado e ON p.id_estado = e.id;
```

### **LEFT JOIN**:

Retorna todos os registros da tabela à esquerda (Tabela 1), e os registros correspondentes da tabela à direita (Tabela 2). Se não houver correspondência, o resultado será NULL para a tabela da direita.

```sql
SELECT p.nome, e.nome
FROM tb_pessoa p
LEFT JOIN tb_estado e ON p.id_estado = e.id;
```

### **RIGHT JOIN**:

Retorna todos os registros da tabela à direita, e os registros correspondentes da tabela à esquerda. Se não houver correspondência, o resultado será NULL para a tabela da esquerda.

```
SELECT p.nome, e.nome
FROM tb_pessoa p
RIGHT JOIN tb_estado e ON p.id_estado = e.id;
```

### **FULL JOIN**:

Combina o resultado de um LEFT JOIN e de um RIGHT JOIN. Retorna todos os registros de ambas as tabelas, com NULL onde não há correspondência.

```sql
SELECT p.nome, e.nome
FROM tb_pessoa p
FULL JOIN tb_estado e ON p.id_estado = e.id;
```

### **CROSS JOIN**:

Retorna o produto cartesiano entre as duas tabelas, ou seja, combina cada linha de uma tabela com cada linha da outra.

```sql
SELECT p.nome, e.nome
FROM tb_pessoa p
CROSS JOIN tb_estado e;
```

### Resumo:

- **Índices**: usados para melhorar o desempenho de consultas.
- **Constraints**: garantem integridade de dados (PK, FK, UNIQUE, CHECK).
- **Joins**: combinam informações de tabelas diferentes, com tipos como INNER, LEFT, RIGHT, e FULL.
---
