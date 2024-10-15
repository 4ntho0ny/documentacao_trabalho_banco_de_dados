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

r |o--|| ca : ocupar
r |o--|| cl : solicitar
r |o--|| f : fazer
r ||--|| p : ter
```

## Desenvolvimento do Banco de Dados

```
create database locadora_veiculos; 

use locadora_veiculos;

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

create table Carro( 
    id_carro int primary key auto_increment, 
    placa varchar(7) not null, 
    cor varchar(30) not null, 
    ano int not null, 
    marca varchar(40), 
    modelo varchar(40) not null, 
    chassi varchar(17) not null 
); 

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

create table Pagamento( 
    id_pagamento int primary key auto_increment, 
    valor_total decimal(11,2) not null, 
    metodo_pagamento enum('debito', 'credito', 'boleto', 'pix', 'dinheiro') 
); 

create table Endereco (
    id_endereco int primary key auto_increment, 
    rua varchar(50) not null, 
    numero int not null, 
    bairro varchar(50) not null, 
    cep varchar(8) not null, 
    complemento varchar(60) not null, 
    cidade varchar(60) not null, 
    estado varchar(60) not null 
); 
```
