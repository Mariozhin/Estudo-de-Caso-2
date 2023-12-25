# Estudo-de-Caso-2

CREATE DATABASE AgroBomDB;

USE AgroBomDB;


CREATE TABLE Cliente (
    numeroCliente INT NOT NULL,
    nome VARCHAR(255),
    telefone VARCHAR(20),
    endereco VARCHAR(255),
    novoCliente BOOLEAN,
     
    CONSTRAINT pk_cliente PRIMARY KEY (numeroCliente)
);

CREATE TABLE Produto (
    numeroProduto INT NOT NULL,
    descricao VARCHAR(255),
    quantidadeEstoque INT,
    unidadeMedida VARCHAR(20),
    qtdMinIdealEstoque INT,
    
    CONSTRAINT pk_produto PRIMARY KEY (numeroProduto)
);

CREATE TABLE Pedido (
    numeroPedido INT NOT NULL,
    dataPedido DATE,
    valorTotal Float,
    desconto Float,
    numeroCliente INT,
    
    CONSTRAINT pk_Pedido PRIMARY KEY (numeroPedido),
    
    CONSTRAINT fk_Cliente FOREIGN KEY (numeroCliente)
    REFERENCES Cliente(numeroCliente)
    ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE PedidoProduto (
    numeroPedido INT,
    numeroProduto INT,
    
    CONSTRAINT fk_Pedido_Pedido FOREIGN KEY (numeroPedido) 
    REFERENCES Pedido(numeroPedido)
    ON UPDATE CASCADE ON DELETE CASCADE,
    
    CONSTRAINT fk_Produto_Produto FOREIGN KEY (numeroProduto) 
    REFERENCES Produto(numeroProduto) 
    ON UPDATE CASCADE ON DELETE CASCADE
);

CREATE TABLE Fornecedor (
    CNPJFornecedor VARCHAR(20) PRIMARY KEY,
    nome VARCHAR(255),
    telefone VARCHAR(20)
);

CREATE TABLE ProdutoFornecedor (
    numeroProduto INT,
    CNPJFornecedor VARCHAR(20),
    PRIMARY KEY (numeroProduto, CNPJFornecedor),
    FOREIGN KEY (numeroProduto) REFERENCES Produto(numeroProduto),
    FOREIGN KEY (CNPJFornecedor) REFERENCES Fornecedor(CNPJFornecedor)
);

CREATE TABLE SolicitacaoCompra (
    numeroSolicitacao INT PRIMARY KEY,
    situacao VARCHAR(20),
    CNPJFornecedor VARCHAR(20),
    FOREIGN KEY (CNPJFornecedor) REFERENCES Fornecedor(CNPJFornecedor)
);

INSERT INTO Cliente (numerocliente, Nome, Telefone, Endereço, NovoCliente)
VALUES (1, 'Joaquim Ferreira', '123-456-7890', 'Rua Alto, 123', FALSE),
       (2, 'Mario Oliveira', '987-654-3210', 'Avenida Calhau, 456', TRUE);

INSERT INTO Pedido (numeropedido, DataPedido, ValorTotal, Desconto, numerocliente)
VALUES (101, '2023-12-18', 500.00, 20.00, 1),
	   (102, '2023-12-19', 300.00, 10.00, 2);


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
/*SELECT Pedido.*, Cliente.nome AS nomeCliente
FROM Pedido
JOIN Cliente ON Pedido.NúmeroCliente = Cliente.NúmeroCliente;

/*3-Contar o número de produtos em cada pedido*/
/*SELECT NúmeroPedido, COUNT(NúmeroProduto) AS QtdProdutos
FROM PedidoProduto
GROUP BY NúmeroPedido;

/*4-Listar todos os produtos em estoque*/
/*SELECT * FROM Produto WHERE quantidadeEstoque > 0;

/*5-Mostrar os fornecedores de cada produto*/
/*SELECT Produto.*, Fornecedor.nome AS nomeFornecedor
FROM Produto
JOIN ProdutoFornecedor ON Produto.NúmeroProduto = ProdutoFornecedor.NúmeroProduto
JOIN Fornecedor ON ProdutoFornecedor.CNPJFornecedor = Fornecedor.CNPJFornecedor;

/*6-Exibir as solicitações de compra encerradas*/
/*SELECT * FROM SolicitacaoCompra WHERE situacao = 'Encerrada';
