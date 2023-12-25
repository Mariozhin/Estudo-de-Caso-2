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

CREATE TABLE Produto (
    numeroproduto INT NOT NULL,
    Descrição VARCHAR(255),
    QuantidadeEstoque INT,
    UnidadeMedida VARCHAR(20),
    QtdMinIdealEstoque INT,
    
    CONSTRAINT pk_produto PRIMARY KEY (numeroproduto)
);

CREATE TABLE Pedido (
    numeropedido INT NOT NULL,
    DataPedido DATE,
    ValorTotal Float,
    Desconto Float,
    numerocliente INT,
    
    CONSTRAINT pk_Pedido PRIMARY KEY (numeropedido),
    
    CONSTRAINT fk_Cliente FOREIGN KEY (numerocliente)
    REFERENCES Cliente(numerocliente)
    ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE PedidoProduto (
    numeropedido INT,
    numeroproduto INT,
    
    CONSTRAINT fk_Pedido_Pedido FOREIGN KEY (numeropedido) 
    REFERENCES Pedido(numeropedido)
    ON UPDATE CASCADE ON DELETE CASCADE,
    
    CONSTRAINT fk_Produto_Produto FOREIGN KEY (numeroproduto) 
    REFERENCES Produto(numeroproduto) 
    ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Fornecedor (
    CNPJFornecedor VARCHAR(20) PRIMARY KEY,
    Nome VARCHAR(255),
    Telefone VARCHAR(20)
);

CREATE TABLE ProdutoFornecedor (
    numeroproduto INT,
    CNPJFornecedor VARCHAR(20),
    PRIMARY KEY (numeroproduto, CNPJFornecedor),
    FOREIGN KEY (numeroproduto) REFERENCES Produto(numeroproduto),
    FOREIGN KEY (CNPJFornecedor) REFERENCES Fornecedor(CNPJFornecedor)
);

CREATE TABLE SolicitacaoCompra (
    NúmeroSolicitacao INT PRIMARY KEY,
    Situação VARCHAR(20),
    CNPJFornecedor VARCHAR(20),
    FOREIGN KEY (CNPJFornecedor) REFERENCES Fornecedor(CNPJFornecedor)
);


INSERT INTO Cliente (numerocliente, Nome, Telefone, Endereço, NovoCliente)
VALUES (1, 'Joaquim Ferreira', '123-456-7890', 'Rua Alto, 123', FALSE),
       (2, 'Mario Oliveira', '987-654-3210', 'Avenida Calhau, 456', TRUE);

INSERT INTO Pedido (numeropedido, DataPedido, ValorTotal, Desconto, numerocliente)
VALUES (101, '18-12-2023', 500.00, 20.00, 1),
       (102, '19-12-2023', 300.00, 10.00, 2);


INSERT INTO PedidoProduto (numeropedido, numeroproduto)
VALUES (101, 1),
       (101, 2),
       (102, 3);

INSERT INTO Produto(numeroproduto, Descrição, QuantidadeEstoque, UnidadeMedida, QtdMinIdealEstoque)
VALUES (1, 'Produto X', 50, 'Unidade', 10),
       (2, 'Produto Y', 30, 'Kg', 5),
       (3, 'Produto Z', 20, 'Litro', 8);

INSERT INTO Fornecedor (CNPJFornecedor, Nome, Telefone)
VALUES ('12345678901234', 'Fornecedor XYZ', '111-222-3333'),
       ('56789012345678', 'Fornecedor ABC', '444-555-6666');

INSERT INTO ProdutoFornecedor (numeroproduto, CNPJFornecedor)
VALUES (1, '12345678901234'),
       (2, '12345678901234'),
       (3, '56789012345678');

INSERT INTO SolicitacaoCompra (NúmeroSolicitacao, Situação, CNPJFornecedor)
VALUES (201, 'Em aberto', '12345678901234'),
       (202, 'Encerrada', '56789012345678');

/* 1-Selecionar todos os clientes*/
/*SELECT * FROM Cliente;

/*2-Selecionar todos os pedidos com detalhes do cliente*/
/*SELECT Pedido.*, Cliente.Nome AS NomeCliente
FROM Pedido
JOIN Cliente ON Pedido.NúmeroCliente = Cliente.NúmeroCliente;

/*3-Contar o número de produtos em cada pedido*/
/*SELECT NúmeroPedido, COUNT(NúmeroProduto) AS QtdProdutos
FROM PedidoProduto
GROUP BY NúmeroPedido;

/*4-Listar todos os produtos em estoque*/
/*SELECT * FROM Produto WHERE QuantidadeEstoque > 0;

/*5-Mostrar os fornecedores de cada produto*/
/*SELECT Produto.*, Fornecedor.Nome AS NomeFornecedor
FROM Produto
JOIN ProdutoFornecedor ON Produto.NúmeroProduto = ProdutoFornecedor.NúmeroProduto
JOIN Fornecedor ON ProdutoFornecedor.CNPJFornecedor = Fornecedor.CNPJFornecedor;

/*6-Exibir as solicitações de compra encerradas*/
/*SELECT * FROM SolicitacaoCompra WHERE Situação = 'Encerrada';
