# SQL Project

Este projeto configura um banco de dados PostgreSQL usando Docker. Inclui scripts SQL para definir o esquema e carregar dados.

<a href="https://github.com/Lucas-Linarelo/SQL-EBAC/blob/main/Tarefa-Demonstrativo.mp4" target="_blank">
  <img src="https://img.shields.io/badge/Assistir%20ao%20Vídeo-Video%20Demonstrativo-brightgreen?style=for-the-badge" alt="Assistir ao Vídeo Demonstração">
</a>

## Estrutura do Projeto

Banco de dados para armazenar dados de projetos, clientes e contratos de obras.

- Criar as 3 tabelas;
- Relacionamento das constrains e inserções de cadastro de vendas;
- Exibição das tabelas por selects.

### Criação de Tabelas

```sql
-- Tabela de Projetos
CREATE TABLE projetos (
    id_projeto SERIAL PRIMARY KEY,
    nome_projeto VARCHAR(100) NOT NULL,
    valor DECIMAL(15, 2) NOT NULL
);

-- Tabela de Clientes
CREATE TABLE clientes (
    id_cliente SERIAL PRIMARY KEY,
    nome_cliente VARCHAR(100) NOT NULL,
    email_cliente VARCHAR(100) UNIQUE NOT NULL
);

-- Tabela de Contratos de Obras
CREATE TABLE contratos (
    id_contrato SERIAL PRIMARY KEY,
    id_projeto INT NOT NULL,
    id_cliente INT NOT NULL,
    data_assinatura TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    prazo_dias INT CHECK (prazo_dias > 0),
    valor_contrato DECIMAL(15, 2) NOT NULL,
    CONSTRAINT fk_projeto FOREIGN KEY (id_projeto) REFERENCES projetos(id_projeto),
    CONSTRAINT fk_cliente FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);
```

### Inserção de Dados: Cadastro de Projetos, Clientes e Contratos

### Inserindo dados na tabela projetos:

```sql
INSERT INTO projetos (nome_projeto, valor) VALUES
('Construção Residencial', 500000.00),
('Reforma Comercial', 200000.00),
('Edifício Empresarial', 1500000.00),
('Construção de Condomínio', 2500000.00);
```

### Inserindo dados na tabela clientes:

```sql
INSERT INTO clientes (nome_cliente, email_cliente) VALUES
('Carlos Almeida', 'carlos.almeida@email.com'),
('Ana Pereira', 'ana.pereira@email.com'),
('Fernando Santos', 'fernando.santos@email.com');
```

### Inserindo dados na tabela contratos:

```sql
INSERT INTO contratos (id_projeto, id_cliente, prazo_dias, valor_contrato) VALUES
(1, 1, 180, 500000.00), -- Carlos Almeida assinou contrato para "Construção Residencial"
(2, 2, 90, 200000.00),  -- Ana Pereira assinou contrato para "Reforma Comercial"
(3, 3, 365, 1500000.00), -- Fernando Santos assinou contrato para "Edifício Empresarial"
(4, 1, 540, 2500000.00); -- Carlos Almeida assinou contrato para "Construção de Condomínio"
```

### 3. Consultas (SELECTs)

### Exibe todos os contratos com detalhes dos projetos e clientes:

```sql
SELECT
    contratos.id_contrato,
    clientes.nome_cliente,
    projetos.nome_projeto,
    contratos.prazo_dias,
    contratos.valor_contrato,
    contratos.data_assinatura
FROM contratos
INNER JOIN clientes ON contratos.id_cliente = clientes.id_cliente
INNER JOIN projetos ON contratos.id_projeto = projetos.id_projeto;
```

### Exibe contratos por cleinte:

```sql
SELECT
    clientes.nome_cliente,
    COUNT(contratos.id_contrato) AS total_contratos,
    SUM(contratos.valor_contrato) AS total_gasto
FROM contratos
INNER JOIN clientes ON contratos.id_cliente = clientes.id_cliente
GROUP BY clientes.nome_cliente;
```

### Exibe contratos por projeto:

```sql
SELECT
    projetos.nome_projeto,
    COUNT(contratos.id_contrato) AS total_contratos,
    SUM(contratos.valor_contrato) AS total_faturado
FROM contratos
INNER JOIN projetos ON contratos.id_projeto = projetos.id_projeto
GROUP BY projetos.nome_projeto;
```
