# Desafio de Modelagem e Implementação de Banco de Dados: Oficina Mecânica

---

## 🚀 Visão Geral do Projeto

Este repositório documenta a jornada completa de modelagem e implementação de um banco de dados para uma oficina mecânica. O projeto abrange desde a criação do esquema conceitual (baseado no desafio anterior), a elaboração do esquema lógico relacional, a escrita do script SQL para criação do banco de dados, a persistência de dados para testes, e a formulação de queries SQL complexas para extração de insights.

O objetivo é demonstrar a capacidade de transformar um modelo conceitual em um banco de dados funcional, aplicando os princípios de modelagem relacional e explorando consultas avançadas para análise de dados.

---

## 💡 Esquema Lógico

O esquema lógico foi projetado para representar as entidades e relacionamentos essenciais de uma oficina, garantindo a integridade e a eficiência dos dados.

### Diagrama Entidade-Relacionamento (DER) do Esquema Lógico

Abaixo está o diagrama que representa o esquema lógico relacional do banco de dados da oficina.

![Esquema Lógico](esquema_logico.png)

---

## 🛠️ Implementação do Banco de Dados

### Script SQL de Criação do Esquema

```sql
USE Oficina;

-- Tabela Cliente
CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY,
    nome VARCHAR(100),
    telefone VARCHAR(20),
    endereco VARCHAR(150)
);

-- Tabela Veiculo
CREATE TABLE Veiculo (
    id_veiculo INT PRIMARY KEY,
    placa VARCHAR(10),
    modelo VARCHAR(50),
    marca VARCHAR(50),
    ano INT,
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
);

-- Tabela Equipe
CREATE TABLE Equipe (
    id_equipe INT PRIMARY KEY,
    nome VARCHAR(100)
);

-- Tabela Mecanico
CREATE TABLE Mecanico (
    id_mecanico INT PRIMARY KEY,
    nome VARCHAR(100),
    endereco VARCHAR(150),
    especialidade VARCHAR(100),
    id_equipe INT,
    FOREIGN KEY (id_equipe) REFERENCES Equipe(id_equipe)
);

-- Tabela Servico
CREATE TABLE Servico (
    id_servico INT PRIMARY KEY,
    descricao VARCHAR(100),
    valor_mao_obra DECIMAL(10,2)
);

-- Tabela Peca
CREATE TABLE Peca (
    id_peca INT PRIMARY KEY,
    descricao VARCHAR(100),
    valor_unitario DECIMAL(10,2)
);

-- Tabela OrdemServico
CREATE TABLE OrdemServico (
    id_os INT PRIMARY KEY,
    data_emissao DATE,
    data_conclusao DATE,
    valor_total DECIMAL(10,2),
    status VARCHAR(50),
    autorizado TINYINT,
    id_veiculo INT,
    id_equipe INT,
    FOREIGN KEY (id_veiculo) REFERENCES Veiculo(id_veiculo),
    FOREIGN KEY (id_equipe) REFERENCES Equipe(id_equipe)
);

-- Tabela OS_Servico
CREATE TABLE OS_Servico (
    id_os INT,
    id_servico INT,
    quantidade INT,
    subtotal DECIMAL(10,2),
    PRIMARY KEY (id_os, id_servico),
    FOREIGN KEY (id_os) REFERENCES OrdemServico(id_os),
    FOREIGN KEY (id_servico) REFERENCES Servico(id_servico)
);

-- Tabela OS_Peca
CREATE TABLE OS_Peca (
    id_os INT,
    id_peca INT,
    quantidade INT,
    subtotal DECIMAL(10,2),
    PRIMARY KEY (id_os, id_peca),
    FOREIGN KEY (id_os) REFERENCES OrdemServico(id_os),
    FOREIGN KEY (id_peca) REFERENCES Peca(id_peca)
);

```

Script SQL para Persistência de Dados
Para fins de teste e demonstração das queries, foram inseridos dados de exemplo em todas as tabelas. A ordem de inserção é crucial para respeitar as chaves estrangeiras.

```sql
USE Oficina;

-- Inserção de Clientes
INSERT INTO Cliente (id_cliente, nome, telefone, endereco) VALUES
(1, 'João Silva', '11999999999', 'Rua A, 100'),
(2, 'Maria Souza', '11888888888', 'Rua B, 200'),
(3, 'Carlos Pereira', '11777777777', 'Rua C, 300'),
(4, 'Ana Costa', '11666666666', 'Rua D, 400'),
(5, 'Lucas Lima', '11555555555', 'Rua E, 500');

-- Inserção de Veículos
INSERT INTO Veiculo (id_veiculo, placa, modelo, marca, ano, id_cliente) VALUES
(1, 'ABC1234', 'Civic', 'Honda', 2015, 1),
(2, 'DEF5678', 'Corolla', 'Toyota', 2016, 2),
(3, 'GHI9012', 'Focus', 'Ford', 2014, 3),
(4, 'JKL3456', 'Fiesta', 'Ford', 2013, 4),
(5, 'MNO7890', 'HB20', 'Hyundai', 2017, 5);

-- Inserção de Equipes
INSERT INTO Equipe (id_equipe, nome) VALUES
(1, 'Equipe Alfa'),
(2, 'Equipe Beta');

-- Inserção de Mecânicos
INSERT INTO Mecanico (id_mecanico, nome, endereco, especialidade, id_equipe) VALUES
(1, 'Pedro Santos', 'Rua X, 10', 'Motor', 1),
(2, 'Rafael Dias', 'Rua Y, 20', 'Freio', 1),
(3, 'Bruno Alves', 'Rua Z, 30', 'Suspensão', 2),
(4, 'Diego Costa', 'Rua W, 40', 'Elétrica', 2);

-- Inserção de Peças
INSERT INTO Peca (id_peca, descricao, valor_unitario) VALUES
(1, 'Pastilha de freio', 150.00),
(2, 'Filtro de ar', 50.00),
(3, 'Amortecedor', 300.00),
(4, 'Correia dentada', 200.00),
(5, 'Bateria 60Ah', 400.00),
(6, 'Velas de ignição', 100.00),
(7, 'Pneu 175/70 R13', 250.00),
(8, 'Disco de freio', 220.00),
(9, 'Filtro de óleo', 45.00),
(10, 'Sensor de ABS', 180.00);

-- Inserção de Serviços
INSERT INTO Servico (id_servico, descricao, valor_mao_obra) VALUES
(1, 'Troca de óleo', 100.00),
(2, 'Alinhamento', 80.00),
(3, 'Balanceamento', 70.00),
(4, 'Revisão geral', 300.00),
(5, 'Troca de freio', 200.00),
(6, 'Troca de bateria', 90.00),
(7, 'Troca de amortecedor', 150.00),
(8, 'Instalação de sensor ABS', 120.00),
(9, 'Substituição de correia', 110.00),
(10, 'Troca de filtro de ar', 60.00);

-- Inserção de Ordens de Serviço
INSERT INTO OrdemServico (id_os, data_emissao, data_conclusao, valor_total, status, autorizado, id_veiculo, id_equipe) VALUES
(1, '2025-06-01', '2025-06-02', 520.00, 'Finalizada', 1, 1, 1),
(2, '2025-06-01', '2025-06-02', 300.00, 'Finalizada', 1, 2, 1),
(3, '2025-06-01', NULL, 0.00, 'Aberta', 0, 3, 2),
(4, '2025-06-01', '2025-06-03', 650.00, 'Finalizada', 1, 4, 2),
(5, '2025-06-01', NULL, 0.00, 'Aberta', 0, 5, 1),
(6, '2025-06-02', '2025-06-04', 780.00, 'Finalizada', 1, 1, 2),
(7, '2025-06-02', NULL, 0.00, 'Aberta', 0, 2, 1),
(8, '2025-06-02', '2025-06-05', 950.00, 'Finalizada', 1, 3, 1),
(9, '2025-06-02', '2025-06-05', 420.00, 'Finalizada', 1, 4, 2),
(10, '2025-06-03', NULL, 0.00, 'Aberta', 0, 5, 1),
(11, '2025-06-04', '2025-06-06', 1100.00, 'Finalizada', 1, 1, 2),
(12, '2025-06-05', '2025-06-07', 670.00, 'Finalizada', 1, 2, 1);

-- Inserção de Peças por Ordem de Serviço
INSERT INTO OS_Peca (id_os, id_peca, quantidade, subtotal) VALUES
(1, 1, 1, 150.00),
(1, 9, 1, 45.00),
(2, 2, 1, 50.00),
(4, 3, 1, 300.00),
(4, 7, 1, 250.00),
(6, 4, 1, 200.00),
(6, 5, 1, 400.00),
(7, 6, 2, 200.00),
(8, 8, 1, 220.00),
(9, 10, 1, 180.00),
(11, 3, 2, 600.00),
(12, 7, 1, 250.00);

-- Inserção de Serviços por Ordem de Serviço
INSERT INTO OS_Servico (id_os, id_servico, quantidade, subtotal) VALUES
(1, 1, 1, 100.00),
(1, 2, 1, 80.00),
(2, 5, 1, 200.00),
(4, 7, 1, 150.00),
(4, 2, 1, 80.00),
(6, 9, 1, 110.00),
(7, 10, 1, 60.00),
(8, 3, 1, 70.00),
(9, 8, 1, 120.00),
(10, 4, 1, 300.00),
(11, 5, 2, 400.00),
(12, 2, 1, 80.00);

```

🔍 Queries SQL
Foram elaboradas queries SQL que utilizam diversas cláusulas para extrair informações valiosas do banco de dados. 
Cada query é acompanhada de uma pergunta que ela se propõe a responder e uma imagem do resultado esperado.

1. Detalhamento de Ordens de Serviço e Valor Total
Pergunta: Qual o valor total de cada ordem de serviço, incluindo mão de obra e peças, detalhando o cliente, veículo e equipe responsável?

```sql
SELECT 
    OS.id_os AS OrdemServicoID,
    Cliente.nome AS Cliente,
    Veiculo.modelo AS ModeloVeiculo,
    Veiculo.marca AS MarcaVeiculo,
    Equipe.nome AS EquipeResponsavel,
    SUM(OS_Servico.quantidade * Servico.valor_mao_obra) + SUM(OS_Peca.quantidade * Peca.valor_unitario) AS ValorTotal,
    OS.status AS Status,
    OS.data_emissao AS DataEmissao,
    OS.data_conclusao AS DataConclusao
FROM OrdemServico OS
JOIN Veiculo ON OS.id_veiculo = Veiculo.id_veiculo
JOIN Cliente ON Veiculo.id_cliente = Cliente.id_cliente
JOIN Equipe ON OS.id_equipe = Equipe.id_equipe
LEFT JOIN OS_Servico ON OS.id_os = OS_Servico.id_os
LEFT JOIN Servico ON OS_Servico.id_servico = Servico.id_servico
LEFT JOIN OS_Peca ON OS.id_os = OS_Peca.id_os
LEFT JOIN Peca ON OS_Peca.id_peca = Peca.id_peca
GROUP BY OS.id_os, Cliente.nome, Veiculo.modelo, Veiculo.marca, Equipe.nome, OS.status, OS.data_emissao, OS.data_conclusao
ORDER BY OS.data_emissao DESC;

```
Esta consulta (SELECT) recupera informações detalhadas sobre cada ordem de serviço, incluindo o nome do cliente, modelo e marca do veículo, e a equipe responsável. Ela cria um atributo derivado (ValorTotal), somando os custos de mão de obra dos serviços e os valores das peças associadas à OS. Utiliza junções (JOIN e LEFT JOIN) para combinar dados de diversas tabelas (OrdemServico, Veiculo, Cliente, Equipe, OS_Servico, Servico, OS_Peca, Peca). Os resultados são agrupados (GROUP BY) pela ordem de serviço e ordenados (ORDER BY) pela data de emissão em ordem decrescente, mostrando as OS mais recentes primeiro.

Resultado: 
!(consulta1.png)
