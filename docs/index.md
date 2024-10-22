# Locadora de veículos

## Enunciado

O sistema tem clientes, carros, reservas, e pagamentos. Cada reserva está associada a
um cliente e a um carro específico. Um cliente pode fazer várias reservas, e os carros
podem ser reservados por vários clientes ao longo do tempo.
Pergunta: Como você relacionaria clientes, reservas, e carros? Crie as tabelas e queries
SQL para verificar quais carros estão disponíveis para reserva em uma data específica.

## DER - Diagrama Entidade Relacionamento

``` mermaid
erDiagram

ca[CARRO] {
    int id_carro pk
    varchar(7) placa
    varchar(20) cor
    int ano
    varchar(40) modelo
    varchar(40) marca
    varchar(17) chassi
}

cl[CLIENTE] {
    int id_cliente pk
    int id_endereco fk
    varchar(40) nome
    varchar(11) cpf
    varchar(70) email
    varchar(15) telefone
    varchar(70) endereco
    date data_nascimento
    varchar(9) cnh
    enum sexo "enum('m', 'f')"
}

f[FUNCIONARIO] {
    int id_funcionario pk
    int id_endereco fk
    varchar(40) nome
    varchar(11) cpf
    varchar(70) email
    varchar(15) telefone
    varchar(70) endereco
    date data_admissao
    date tempo_servico
    varchar(25) cargo
}

r[RESERVAS] {
    int id_reserva pk
    int id_carro fk
    int id_cliente fk
    int id_funcionario fk
    int id_pagamento fk
    date data_inicio
    date data_fim
}

p[PAGAMENTO] {
    int id_pagamento pk
    decimal valor_total "decimal(11,2)"
    enum metodo_pagamento "enum('debito', 'credito', 'boleto', 'pix', 'dinheiro')"
}

e[ENDERECO] {
    int id_endereco pk 
    varchar(50) rua 
    int numero
    varchar(50) bairro
    varchar(8) cep
    varchar(60) complemento
    varchar(60) cidade
    varchar(60) estado
}

r |o--|| ca : ocupar
r |o--|| cl : solicitar
r |o--|| f : fazer
r ||--|| p : ter
f ||--|| e : morar
cl ||--|| e : morar
```

## Desenvolvimento do Banco de Dados

```
create database locadora_veiculos; 

use locadora_veiculos;

create table Endereco(
    id_endereco int primary key auto_increment, 
    rua varchar(50) not null, 
    numero int not null, 
    bairro varchar(50) not null, 
    cep varchar(8) not null, 
    complemento varchar(60) not null, 
    cidade varchar(60) not null, 
    estado varchar(60) not null 
); 

INSERT INTO Endereco VALUES (default, 'Rua das Flores', 123, 'Centro', '12345678', 'Apartamento 12', 'Campo Grande', 'Mato Grosso do Sul'),
(default, 'Avenida Paulista', 456, 'Bela Vista', '87654321', 'Sala 1503', 'São Paulo', 'São Paulo'),
(default, 'Rua Oliveira', 789, 'Jardim América', '11223344', 'Casa 2', 'Rio de Janeiro', 'Rio de Janeiro'),
(default, 'Alameda Santos', 321, 'Moema', '99887766', 'Cobertura', 'Belo Horizonte', 'Minas Gerais'),
(default, 'Rua das Palmeiras', 654, 'Vila Mariana', '44332211', 'Bloco B, Apto 45', 'Curitiba', 'Paraná');

create table Carro( 
    id_carro int primary key auto_increment, 
    placa varchar(7) not null, 
    cor varchar(30) not null, 
    ano int not null, 
    marca varchar(40), 
    modelo varchar(40) not null, 
    chassi varchar(17) not null 
);

INSERT INTO Carro VALUES (default, 'ABC1234', 'Preto', 2020, 'Toyota', 'Corolla', '9BWZZZ377VT004251'),
(default, 'XYZ5678', 'Branco', 2018, 'Honda', 'Civic', '1HGCM82633A004352'),
(default, 'JKL9101', 'Azul', 2021, 'Ford', 'Fiesta', '2HGFA16578H500742'),
(default, 'MNO2345', 'Vermelho', 2019, 'Chevrolet', 'Onix', '3VWFE21C04M013657'),
(default, 'PQR6789', 'Prata', 2022, 'Volkswagen', 'Polo', '4T1BG22K41U754983');

create table Cliente( 
    id_cliente int primary key auto_increment, 
    id_endereco int not null, 
    nome varchar(40) not null, 
    cpf varchar(11) not null unique, 
    email varchar(70) not null unique, 
    telefone varchar(15) not null unique, 
    data_nascimento date not null, 
    cnh varchar(9) not null unique, 
    sexo enum('M','F'), 
    foreign key (id_endereco) references Endereco (id_endereco) 
); 

INSERT INTO Cliente VALUES (default, 1, 'João Silva', '12345678901', 'joao.silva@gmail.com', '(67) 91234-5678', '1990-05-15', '123456789', 'M'),
(default, 2, 'Maria Oliveira', '98765432100', 'maria.oliveira@hotmail.com', '(11) 99876-5432', '1985-11-23', '987654321', 'F'),
(default, 3, 'Carlos Pereira', '45678912345', 'carlos.pereira@yahoo.com', '(21) 97765-4321', '1992-07-10', '456789123', 'M'),
(default, 4, 'Fernanda Costa', '32165498700', 'fernanda.costa@outlook.com', '(31) 93456-7890', '1995-02-28', '321654987', 'F'),
(default, 5, 'Lucas Souza', '15975345682', 'lucas.souza@exemplo.com', '(41) 92345-6789', '1988-09-12', '159753456', 'M');

create table Funcionario( 
    id_funcionario int primary key auto_increment, 
    id_endereco int not null, 
    nome varchar(40) not null, 
    cpf varchar(11) not null unique, 
    email varchar(70) not null unique, 
    telefone varchar(15) not null unique, 
    data_admissao date not null, 
    tempo_servico date not null, 
    cargo varchar(30) not null, 
    foreign key (id_endereco) references Endereco (id_endereco) 
);

INSERT INTO Funcionario VALUES (default, 1, 'Pedro Almeida', '12312312311', 'pedro.almeida@empresa.com', '(67) 91234-4321', '2022-01-15', '2024-01-15', 'Gerente de Vendas'),
(default, 2, 'Ana Paula', '32132132122', 'ana.paula@empresa.com', '(11) 99876-1234', '2021-03-12', '2024-03-12', 'Analista de TI'),
(default, 3, 'Lucas Fernandes', '45645645633', 'lucas.fernandes@empresa.com', '(21) 97765-5678', '2020-05-25', '2024-05-25', 'Supervisor de Produção'),
(default, 4, 'Clara Nogueira', '78978978944', 'clara.nogueira@empresa.com', '(31) 93456-9876', '2019-07-30', '2024-07-30', 'Coordenadora de Projetos'),
(default, 5, 'Roberto Silva', '15915915955', 'roberto.silva@empresa.com', '(41) 92345-4321', '2023-02-10', '2024-02-10', 'Assistente Administrativo');

create table Pagamento( 
    id_pagamento int primary key auto_increment, 
    valor_total decimal(11,2) not null, 
    metodo_pagamento enum('debito', 'credito', 'boleto', 'pix', 'dinheiro') 
);

INSERT INTO Pagamento VALUES (default, 250.75, 'credito'),
(default, 123.50, 'debito'),
(default, 89.99, 'pix'),
(default, 450.00, 'boleto'),
(default, 35.00, 'dinheiro');

create table Reserva( 
    id_reserva int primary key auto_increment, 
    id_carro int not null, 
    id_cliente int not null, 
    id_funcionario int not null, 
    id_pagamento int not null, 
    data_inicio datetime not null, 
    data_fim datetime not null, 
    foreign key (id_carro) references Carro (id_carro), 
    foreign key (id_cliente) references Cliente (id_cliente), 
    foreign key (id_funcionario) references Funcionario (id_funcionario), 
    foreign key (id_pagamento) references Pagamento (id_pagamento) 
); 

INSERT INTO Reserva VALUES (default, 5, 1, 2, 3, '2024-10-01', '2024-10-10'),
(default, 1, 2, 3, 4, '2024-11-05', '2024-11-15'),
(default, 2, 3, 4, 5, '2024-12-12', '2024-12-20'),
(default, 3, 4, 5, 1, '2024-10-20', '2024-10-30'),
(default, 4, 5, 1, 2, '2024-10-25', '2024-11-05');

SELECT 
    Cliente.nome AS Nome_Cliente,
    Funcionario.nome AS Nome_Funcionario,
    Carro.modelo AS Modelo_Carro,
    Pagamento.valor_total AS Valor_Pagamento
FROM 
    Reserva
INNER JOIN Cliente ON Reserva.id_cliente = Cliente.id_cliente
INNER JOIN Funcionario ON Reserva.id_funcionario = Funcionario.id_funcionario
INNER JOIN Carro ON Reserva.id_carro = Carro.id_carro
INNER JOIN Pagamento ON Reserva.id_pagamento = Pagamento.id_pagamento;
```
