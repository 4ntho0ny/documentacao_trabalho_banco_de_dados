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
