## mtick.json, API 2.0 
```json
"mtick"{
"tick": [ { "security": "EUR_USD", "price": 1.105780, "pips": 5, "liquidity": 5000000, "side": "ask" },
          { "security": "EUR_USD", "price": 1.105690, "pips": 5, "liquidity": 1000000, "side": "bid" }],

"order": [ { "tempid": 190, "orderid": "TRD_20151027132551451_0126", "fixid": "TRD_20151027132551451_0126",
             "commcurrency": "USD", "commission": 15.000000,
             "security": "EUR_USD", "pips": 5, "quantity": 100000, "side": "buy", "type": "market",
             "finishedprice": 1.105090, "finishedquantity": 100000,  "priceatstart": 1.105090,
             "status": "executed" }],

"accounting": { "m2mcurrency": "EUR", "strategyPL": 9.049119, "totalequity": 9.049119,
                "usedmargin": 2499.773772, "freemargin": -2490.724653},

"assetposition": [ { "asset": "EUR", "exposure": 0.000000,"totalrisk": 100000.000000, "pl": 100000.000000 },
                   { "asset": "USD", "exposure": 21.000000, "totalrisk": -110498.000000,  "pl": -103285.242458 }],

"securityposition": [ { "security": "EUR_USD", "exposure": 100000.000000, "side": "buy",
                        "price": 1.105190, "pips": 5, "pl": -12.348273 }],

"timestamp": "1445952273.659753" }
```
### Long:

```json
"tick": [
      { "security": "EUR_USD",
        "tinterface": "TI1",
        "price": 1.105760,
        "pips": 5,
        "liquidity": 1000000,
        "side": "ask" },

      { "security": "EUR_USD",
        "tinterface": "TI1",
        "price": 1.105770,
        "pips": 5,
        "liquidity": 1000000,
        "side": "ask" },

      { "security": "EUR_USD",
        "tinterface": "TI1",
        "price": 1.105780,
        "pips": 5,
        "liquidity": 5000000,
        "side": "ask" },

      { "security": "EUR_USD",
        "tinterface": "TI1",
        "price": 1.105850,
        "pips": 5,
        "liquidity": 10000000,
        "side": "bid" },

      { "security": "EUR_USD",
        "tinterface" : "TI1",
        "price": 1.105800,
        "pips": 5,
        "liquidity": 10000000,
        "side": "bid" },

      { "security": "EUR_USD",
        "tinterface": "TI1",
        "price": 1.105690,
        "pips": 5,
        "liquidity": 1000000,
        "side": "bid" } ],

"order": [
      { "tempid": 190,
        "orderid": "TRD_20151027132551451_0126",
        "fixid": "TRD_20151027132551451_0126",
        "account": "AC1",
        "tinterface": "TI1",
        "security": "EUR_USD",
        "pips": 5,
        "quantity": 100000,
        "side": "buy",
        "type": "market",
        "finishedprice": 1.105090,
        "finishedquantity": 100000,
        "commcurrency": "USD",
        "commission": 15.000000,
        "priceatstart": 1.105090,
        "userparam": 0,
        "status": "executed" } ],

"accounting": {
        "m2mcurrency": "EUR",
        "strategyPL": 9.049119,
        "totalequity": 9.049119,
        "usedmargin": 2499.773772,
        "freemargin": -2490.724653 },

"assetposition": [
              { "account": "AC1",
                "asset": "EUR",
                "exposure": 0.000000,
                "totalrisk": 100000.000000,
                "pl": 100000.000000 },

              { "account": "<AGGREGATED>",
                "asset": "EUR",
                "exposure": 0.000000,
                "totalrisk": 100000.000000,
                "pl": 100000.000000 },

              { "account": "AC1",
                "asset": "USD",
                "exposure": 21.000000,
                "totalrisk": -110498.000000,
                "pl": -103285.242458 },

              { "account": "<AGGREGATED>",
                "asset": "USD",
                "exposure": 21.000000,
                "totalrisk": -110498.000000,
                "pl": -103285.242458 } ],

"securityposition": [
              { "account": "AC1",
                "security": "EUR_USD",
                "exposure": 100000.000000,
                "side": "buy",
                "price": 1.105190,
                "pips": 5,
                "pl": -12.348273 },

              { "account": "<AGGREGATED>",
                "security": "EUR_USD",
                "exposure": 100000.000000,
                "side": "buy",
                "price": 1.105190,
                "pips": 5,
                "pl": -12.348273 } ],

"timestamp": "1445952273.659753" }
}


```
