The Smart API Go client
The official Go client for communicating with the Angel Broking Smart APIs.

SmartAPI is a set of REST-like APIs that expose many capabilities required to build a complete investment and trading platform. Execute orders in real time, manage user portfolio, stream live market data (WebSockets), and more, with the simple HTTP API collection.


Installation
go get github.com/angelbroking-github/smartapi-golang
API usage
package main

import (
	"fmt"
	SmartApi "github.com/angelbroking-github/smartapi-golang"
)

func main() {

	// Create New Angel Broking Client
	ABClient := SmartApi.New("ClientCode", "Password")

	fmt.Println("Client :- ",ABClient)

	// User Login and Generate User Session
	session, err := ABClient.GenerateSession()

	if err != nil {
		fmt.Println(err.Error())
		return
	}

	//Renew User Tokens using refresh token
	session.UserSessionTokens, err = ABClient.RenewAccessToken(session.RefreshToken)

	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println("User Session Tokens :- ", session.UserSessionTokens)

	//Get User Profile
	session.UserProfile, err = ABClient.GetUserProfile()

	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println("User Profile :- ", session.UserProfile)
	fmt.Println("User Session Object :- ", session)

	//Place Order
	order, err := ABClient.PlaceOrder(SmartApi.OrderParams{Variety: "NORMAL", TradingSymbol: "SBIN-EQ", SymbolToken: "3045", TransactionType: "BUY", Exchange: "NSE", OrderType: "LIMIT", ProductType: "INTRADAY", Duration: "DAY", Price: "19500", SquareOff: "0", StopLoss: "0", Quantity: "1"})

	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println("Placed Order ID and Script :- ", order)
}
Examples
Check examples folder for more examples.

You can run the following after updating the Credentials in the examples:

go run examples/example.go
Run unit tests
go test -v