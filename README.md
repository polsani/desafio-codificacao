# Code Challenge: Autorizador

O desafio é implementar uma função que autoriza reservas para um cliente específico seguindo uma série de regras predefinidas.

## Entrada

Os **parâmetros** da função são:
- A reserva a ser autorizada
- O estado do cliente

A **reserva** é feita para um voo (`Flight`), possui um determinado valor (`Amount`), a data e hora de partida e chegada (`DepartureDateTime`, `ArriveDateTime`), aeroporto de partida e chegada ('DepartureAirport', 'ArriveAirport') e a distancia do voo ('Distance')

O **estado** do cliente inclui as seguintes informações:
- Se o cliente está ativo (`Active`)
- Milhas que podem ser utilizadas (`AvailableMiles`)
- O histórico das reservas daquele cliente (`History`)

## Saída

O estado atual da conta junto de quaisquer violações da lógica de negócios. Se não houverem violações no processamento da operação, o campo 'Violations' deve retornar um vetor vazio [].

## Regras de negócio

A autorização de uma reserva deve seguir as seguintes regras:

- Nenhuma reserva deve ser autorizada para um cliente inativo: 'customer-not-active'
- Não deve haver mais que 1 reserva similar (mesmo aeroporto origem/destino e data e hora de chegada/partida): 'doubled-booking'
- Não deve haver reservas onde a data/hora de partida invada outra reserva (no caso de reservas para o mesmo dia): 'invalid-booking-date'
- O intervalo mínimo entre a chegada de uma reserva e a partida de outra no caso delas serem no mesmo dia é de no mínimo 3 horas: 'invalid-booking-interval'

Se todas as regras forem atendidas, a quantidade de milhas deve ser adicionada no saldo de milhas do cliente e o histórico de reservas deve ser atualizado.

## Teste sua solução
