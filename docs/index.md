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
