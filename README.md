# PayFast Client for Go

A client library written in Go (golang) for the [PayFast](https://www.payfast.co.za/) payment gateway which uses no
external dependencies.

## Scope
This client has parity with the existing API functionality provided by PayFast.

## Installation
```shell script
go get -u github.com/huysamen/payfast-go
```

## Quickstart
Create a new client default client to start accessing your PayFast account.
```go
import payfast "github.com/huysamen/payfast-go"

pf, err := payfast.Default()
``` 

This will create a client with the default settings.  It also expects the following environment variables to be set:
```
PAYFAST_MERCHANT_ID=[your payfast merchant id]
PAYFAST_MERCHANT_PASSPHRASE=[your payfast merchant passphrase]
```

You can also create a more configurable client which accepts an `*http.Client` as well as the merchant ID and passphrase.

```go
pf, err := payfast.New(123, "passphrase", httpClient)
```

## Examples and How To

### Health checks

#### API health check
```go
ok, err := pf.Health.Ping()
```

### Subscriptions

#### Fetch subscription
```go
sub, err := pf.Subscriptions.Fetch("subscription-token")
```

#### Update subscription
An example of how to use the nullable types:
```go
sub, err := pf.Subscriptions.Update(
	"subscription-token",
	subscriptions.UpdateSubscriptionReq{
		Cycles:    types.Numeric{},     // default instance treated as nil and ignored
		Frequency: types.NewNumeric(types.Annual),
		//RunDate: types.Time{},        // excluded field treated as nil and ignored
		Amount: types.NewNumeric(123),
	},
)
```