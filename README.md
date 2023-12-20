# Estudo-de-Caso-2


CREATE DATABASE AgroBomDB;

USE AgroBomDB;

CREATE TABLE Cliente (
    numerocliente INT NOT NULL,
    nome VARCHAR(255),
    Telefone VARCHAR(20),
    Endereço VARCHAR(255),
    NovoCliente BOOLEAN,
     
     CONSTRAINT pk_Cliente PRIMARY KEY (numerocliente)
);

CREATE TABLE Pedido (
    NúmeroPedido INT NOT NULL,
    DataPedido DATE,
    ValorTotal DECIMAL(10, 2),
    Desconto DECIMAL(5, 2),
    numerocliente INT ,
    
    CONSTRAINT pk_Pedido PRIMARY KEY (NúmeroPedido),
    
    CONSTRAINT fk_Cliente FOREIGN KEY (numerocliente)
    REFERENCES Cliente(numeroCliente)
    ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE PedidoProduto (
    NúmeroPedido INT,
    NúmeroProduto INT,
    
     CONSTRAINT pk_PedidoProduto PRIMARY KEY (NúmeroPedido, NúmeroProduto),
     
     CONSTRAINT fk_Pedido_Produto FOREIGN KEY (NúmeroPedido) 
     REFERENCES Pedido(NúmeroPedido) 
     ON UPDATE CASCADE ON DELETE CASCADE,
     
     CONSTRAINT fk_Pedido_Pedido FOREIGN KEY (NúmeroProduto) 
     REFERENCES Produto(NúmeroProduto)
     ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Produto (
    NúmeroProduto INT NOT NULL,
    Descrição VARCHAR(255),
    QuantidadeEstoque INT,
    UnidadeMedida VARCHAR(20),
    QtdMinIdealEstoque INT
    
     CONSTRAINT pk_Produto PRIMARY KEY (NúmeroProduto)
);

CREATE TABLE Fornecedor (
    CNPJFornecedor VARCHAR(20) PRIMARY KEY,
    Nome VARCHAR(255),
    Telefone VARCHAR(20)
);

CREATE TABLE ProdutoFornecedor (
    NúmeroProduto INT,
    CNPJFornecedor VARCHAR(20),
    PRIMARY KEY (NúmeroProduto, CNPJFornecedor),
    FOREIGN KEY (NúmeroProduto) REFERENCES Produto(NúmeroProduto),
    FOREIGN KEY (CNPJFornecedor) REFERENCES Fornecedor(CNPJFornecedor)
);

CREATE TABLE SolicitacaoCompra (
    NúmeroSolicitacao INT PRIMARY KEY,
    Situação VARCHAR(20),
    CNPJFornecedor VARCHAR(20),
    FOREIGN KEY (CNPJFornecedor) REFERENCES Fornecedor(CNPJFornecedor)
);

INSERT INTO Cliente (NúmeroCliente, Nome, Telefone, Endereço, NovoCliente)
VALUES (1, 'Joaquim Ferreira', '123-456-7890', 'Rua Alto, 123', FALSE),
       (2, 'Mario Oliveira', '987-654-3210', 'Avenida Calhau, 456', TRUE);

INSERT INTO Pedido (NúmeroPedido, DataPedido, ValorTotal, Desconto, NúmeroCliente)
VALUES (101, '18-12-2023', 500.00, 20.00, 1),
       (102, '19-12-2023', 300.00, 10.00, 2);


INSERT INTO PedidoProduto (NúmeroPedido, NúmeroProduto)
VALUES (101, 1),
       (101, 2),
       (102, 3);

INSERT INTO Produto (NúmeroProduto, Descrição, QuantidadeEstoque, UnidadeMedida, QtdMinIdealEstoque)
VALUES (1, 'Produto X', 50, 'Unidade', 10),
       (2, 'Produto Y', 30, 'Kg', 5),
       (3, 'Produto Z', 20, 'Litro', 8);

INSERT INTO Fornecedor (CNPJFornecedor, Nome, Telefone)
VALUES ('12345678901234', 'Fornecedor XYZ', '111-222-3333'),
       ('56789012345678', 'Fornecedor ABC', '444-555-6666');

INSERT INTO ProdutoFornecedor (NúmeroProduto, CNPJFornecedor)
VALUES (1, '12345678901234'),
       (2, '12345678901234'),
       (3, '56789012345678');

INSERT INTO SolicitacaoCompra (NúmeroSolicitacao, Situação, CNPJFornecedor)
VALUES (201, 'Em aberto', '12345678901234'),
       (202, 'Encerrada', '56789012345678');
       
