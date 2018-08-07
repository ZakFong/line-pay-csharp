# LINE PAY dotnet core C# SDK
This is unofficial SDK for LINE Pay for C# developers.

You can find detail API document [here](https://pay.line.me/jp/developers/documentation/download/tech?locale=en_US)

# Getting Started
This repository contains sample web application to use the SDK.
Update ChannelId, ChannelSecret and ServerUri.

# How to use it

**Instantiate client**
Pass channel information and if you use sandbox or not.

```csharp
var channelId = "channel id";
var channelSecret = "channel secret";
var isSandbox = true;
var client = new LinePayClient(
                channelId,
                channelSecret,
                isSandbox);
```

**Reserve the order**
Instantiate reserve object and pass to ReserveAsync

```csharp
var reserve = new Reserve()
{
    ProductName = "チョコレート",
    Amount = 1,
    Currency = Currency.JPY,
    OrderId =  Guid.NewGuid().ToString(),
    ConfirmUrl = $"{configuration["ServerUri"]}/api/pay/confirm",
    CancelUrl = $"{configuration["ServerUri"]}/api/pay/cancel",
    Capture = capture,
    PayType = payType
}

var response = await client.ReserveAsync(reserve);
```

**Confirm the order**
Instantiate confirm object by using reserve you created, and call ConfirmAsync.

```csharp
var transactionId = Int64.Parse(HttpContext.Request.Query["transactionId"]);

var reserve = //get your reserve from db.

var confirm = new Confirm()
{
    Amount = reserve.Amount,
    Currency = reserve.Currency
}

var response = await client.ConfirmAsync(transactionId, confirm);
```

See the sample web app in the repository for more detail.

#License
[MIT](./LICENSE)