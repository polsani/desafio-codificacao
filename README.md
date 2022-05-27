# Code Challenge: Autorizador

O desafio é implementar uma função que autoriza reservas para um cliente específico seguindo uma série de regras predefinidas.

## Entrada

Os **parâmetros** da função são:
- A reserva a ser autorizada
- O estado do cliente

A **reserva** é feita para um voo (`Flight`), possui um determinado valor (`Amount`), a data e hora de partida e chegada (`DepartureDateTime`, `ArriveDateTime`), aeroporto de partida e chegada (`DepartureAirport`, `ArriveAirport`) e a distancia do voo (`Distance`)

O **estado** do cliente inclui as seguintes informações:
- Se o cliente está ativo (`Active`)
- Milhas que podem ser utilizadas (`AvailableMiles`)
- O histórico das reservas daquele cliente (`History`)

## Saída

O estado atual da conta junto de quaisquer violações da lógica de negócios. Se não houver violações no processamento da operação, o campo 'Violations' deve retornar um vetor vazio [].

## Regras de negócio

A autorização de uma reserva deve seguir as seguintes regras:

- Nenhuma reserva deve ser autorizada para um cliente inativo: 'customer-not-active'
- Não deve haver mais que 1 reserva similar (mesmo aeroporto origem/destino e data e hora de chegada/partida): 'doubled-booking'
- Não deve haver reservas onde a data/hora de partida invada outra reserva (no caso de reservas para o mesmo dia): 'invalid-booking-date'
- O intervalo mínimo entre a chegada de uma reserva e a partida de outra no caso delas serem no mesmo dia é de no mínimo 3 horas: 'invalid-booking-interval'

Se todas as regras forem atendidas, a quantidade de milhas (distancia entre os dois aeroportos) deve ser adicionada no saldo de milhas do cliente e o histórico de reservas deve ser atualizado.

## Exemplo de uso

### Reserva autorizada

```javascript
Booking = {
	Amount: 750.80,
	Flight: "ACG0926",
	DepartureDateTime: "2022-05-22T10:00:00",
	ArriveDateTime: "2022-05-22T15:00:00",
	DepartureAirport: "GRU",
	ArriveAirport: "REC",
	Distance: 2000
}

Customer = {
	Active: true,
	AvailableMiles: 400,
	History: []
}

Result = Authorize(Booking, Account);

Result.Customer == {
	Active: true,
	AvailableMiles: 2400,
	History: [{
		Amount: 750.80,
		Flight: "ACG0926",
		DepartureDateTime: "2022-05-22T10:00:00",
		ArriveDateTime: "2022-05-22T15:00:00",
		DepartureAirport: "GRU",
		ArriveAirport: "REC",
		Distance: 2000
	}]
}

Result.Violations == []
```

### Reserva não autorizada

```javascript
Booking = {
	Amount: 950,
	Flight: "TDG1365",
	DepartureDateTime: "2022-05-22T16:00:00",
	ArriveDateTime: "2022-05-22T21:00:00",
	DepartureAirport: "REC",
	ArriveAirport: "GRU",
	Distance: 2000
}

Customer = {
	Active: false,
	AvailableMiles: 400,
	History: [{
		Amount: 750.80,
		Flight: "ACG0926",
		DepartureDateTime: "2022-05-22T10:00:00",
		ArriveDateTime: "2022-05-22T15:00:00",
		DepartureAirport: "GRU",
		ArriveAirport: "REC",
		Distance: 2000
	}]
}

Result = Authorize(Booking, Account);

Result.Customer == {
	Active: true,
	AvailableMiles: 2400,
	History: [{
		Amount: 750.80,
		Flight: "ACG0926",
		DepartureDateTime: "2022-05-22T10:00:00",
		ArriveDateTime: "2022-05-22T15:00:00",
		DepartureAirport: "GRU",
		ArriveAirport: "REC",
		Distance: 2000
	}]
}

Result.Violations == ['customer-not-active', 'invalid-booking-interval']
```

## Dicas

Algumas dicas podem ser úteis para facilitar a execução do desafio. Tenha em mente que elas estão aqui apenas como sugestões e não precisam ser obrigatoriamente seguidas.

- Considere iniciar a implementação de uma regra de negócio e depois expanda para as demais violações;
- Considere em estruturar uma solução que seja extensível ao invés de querer apenas implementar todas as violações sugeridas;
- Considere implementar a solução como se estivesse implementando a lógica de negócio de um serviço. Evite se preocupar com fatores externos a lógica principal;
- Teste a sua solução!
