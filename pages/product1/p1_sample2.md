---
title: Manual and algorithmic trader
keywords: sample
summary: "This is just a sample topic..."
sidebar: product1_sidebar
permalink: p1_sample2.html
folder: product1
---

ACT allows traders to trade either manually through the GUI or algorithmically through a RESTful API. Each type of trading is performed on its own accounting unit, allowing the trader to separately track performance.

ACT supports hosted trading systems as well through a low latency, low level C API. Details can be requested by directly contacting Arthika Trading's customer support.

A full reference of ACT's RESTful API including a complete set of open source code examples can be found in the [ACT API REST Github repository](https://github.com/Arthika/REST-API/wiki)

## General Information on the RESTful API  

* Order/trading services allow a trading strategy to send buy or sell orders, modify (replace) or cancel them or check the status of any order.
* Any order can be accepted (before generating a 'trade') or refused by many reasons (not enough margin, invalid trading interface or security, ...) After an order is accepted, it will remain in a 'pending order' state, till it will be executed.
* Once an order is accepted, it can be modified or cancelled by the trading strategy.
* The result of executing an order is a 'trade'. Any trade object has much more properties than the original order sent by the strategy.
* A startegy can try to execute several order on a single request to improve lantecy.
 
## Examples  

*  Sending a 100,000 EUR-USD 'Buy' order of type market, using a given trading interface (TI):  

```javascript
$ curl -i --data '{"setOrder":{"user":"demo","token":"927814C103AAF349D435659059BCCEAD9A91C8E9","order":[{"security":"EUR_USD","tinterface":"Baxter_CNX","quantity":100000,"side":"buy","type":"market"}]}}' http://demo.arthikatrading.com:81/fcgi-bin/IHFTRestAPI/setOrder --header 'Content-Type: application/json'

HTTP/1.1 200 OK
Date: Thu, 02 Jul 2015 00:00:00 GMT
Server: Apache/2.4.7 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json;charset=iso-8859-1

{ "setOrderResponse": { 
   "order": [ 
      { "security": "EUR_USD", 
        "tinterface": "Baxter_CNX", 
        "quantity": 100000, 
        "side": "buy", 
        "type": "market",
        "timeinforce": "fill or kill", 
        "tempid": 233, 
        "result": "order requested" } ], 
   "timestamp": "1445961592.763712" } 
}
```

*  Sending a 100,000 EUR-USD 'Sell' order of type market, using a given trading interface (TI):

```javascript
$ curl -i --data '{"setOrder":{"user":"demo","token":"927814C103AAF349D435659059BCCEAD9A91C8E9","order":[{"security":"EUR_USD","tinterface":"Baxter_CNX","quantity":100000,"side":"sell","type":"market"}]}}' http://demo.arthikatrading.com:81/fcgi-bin/IHFTRestAPI/setOrder --header 'Content-Type: application/json'

HTTP/1.1 200 OK
Date: Thu, 02 Jul 2015 00:00:00 GMT
Server: Apache/2.4.7 (Ubuntu)
Transfer-Encoding: chunked
Content-Type: application/json;charset=iso-8859-1

{ "setOrderResponse": { 
   "order": [ 
      { "security": "EUR_USD", 
        "tinterface": "Baxter_CNX", 
        "quantity": 100000, 
        "side": "sell", 
        "type": "market", 
        "timeinforce": "fill or kill", 
        "tempid": 234, 
        "result": "order requested" } ], 
   "timestamp": "1445961770.890986" } 
}
```

## Sending an order

#### setOrder() Request: setOrder

* **user**: 		*Required*.	Strategy login assigned by the backend administrator.
* **token**: 		*Required*.	Token obtained during the [authentication procedure](https://github.com/Arthika/API-REST/wiki/Authentication).
* **order**:            *Required*.	List of orders to be created (one or more orders). Each order will have following fields:
    * **security**: 	Required.	Security/instrument to be applied for this order.
    * **tinterface**: 	Optional.	Trading interface where order will be routed through. Default value: 	Trading interface with the best top-of-book price (best price).
    * **quantity**: 	Required.	Number of security items to be bought/sell.
    * **side**: 	Required.	"buy" or "sell".
    * **type**: 	Required.	"market" or "limit".
    * **timeinforce**:	Optional.	"day", "good till cancel", "inmediate or cancel" or "fill or kill". Default value: "fill or kill".
    * **price**: 	Optional.	Can be set only with "limit" orders. Defines the price to be reached before executing the order. If price is not reached, order will remain "pending" till expiration time will be reached. Default value: Trading interface with the top-of-book price (best price).
    * **expiration**: 	Optional.	Can be set only with "limit" orders. Defines the maximum time in seconds that order can exist as 'pending'. When expiration time is reached, order is self-destroyed. Default value: 	Trading interface with the top-of-book price (best price).
    * **param**: 	Optional.	Private parameter defined by the user for custom usage (must be an integer).

### Response: setOrderResponse

  * **result**: 	Error code.
  * **message**: 	String with the result message in case of error.
  * **order**: 		List of created orders with following fields:
      * **security**: 	Security/instrument applied for the order.
      * **tinterface**: 	Trading interface where order will be routed through.
      * **quantity**: 	Number of security items to be bought/sell.
      * **side**: 		"buy" or "sell".
      * **type**: 		"market" or "limit".
      * **timeinforce**:	"day", "good till cancel", "inmediate or cancel" or "fill or kill".
      * **price**: 		Only present on "limit" orders.
	Defines the price to be reached before executing the order.
	If price is not reached, order will remain "pending" till expiration time will be reached.
      * **expiration**: 	Only present on "limit" orders. Defines the maximum time that order can exist as 'pending'. It is defined in seconds. When expiration time is reached, order is self-destructed.
      * **userparam**: 	Private parameter defined by the user for custom usage (an integer).
      * **tempid**: 	Temporary order ID assigned by Arthika system.
      * **result**: 	"order requested" when order was received by Arthika system to be processed, or an error.

## cancelOrder()

### Request: cancelOrder

* **user**: 		*Required*.	Strategy login assigned by the backend administrator.
* **token**: 		*Required*.	Token obtained during the [authentication procedure](https://github.com/Arthika/API-REST/wiki/Authentication).
* **fixid**:            *Required*. 	List of orders to be cancelled (one or more orders). Each order will be defined by the Fix ID field returned by getOrder web service (streaming or polling)

### Response: cancelOrderResponse
  * **result**: 	Error code.
  * **message**: 	String with the result message in case of error.
  * **order**: 		List of cancelled orders with following fields:
      * **fixid**: 		Fix ID identifiying the order to be cancelled.
      * **result**: 		String indicating the result of this operation, that can be an error or "cancel requested" if cancelation is in progress.

## modifyOrder()

###Request: modifyOrder

* **user**: 		*Required*.	Strategy login assigned by the backend administrator.
* **token**: 		*Required*.	Token obtained during the [authentication procedure](https://github.com/Arthika/API-REST/wiki/Authentication).
* **order**:            *Required*. 	List of pending orders to be modified (one or more orders). Each order will have following fields:
      * **fixid**:            *Required*. 	Fix ID field returned by getOrder web service (streaming or polling) after order was launched by 'setOrder' web service.
      * **price**:            *Required*. 	New price to be applied in the order.
      * **quantity**:         *Required*. 	New quantity to be applied in the order.

### Response: modifyOrderResponse

  * **result**: 	Error code.
  * **message**: 	String with the result message in case of error.
  * **order**: 		List of modified orders with following fields:
      * **fixid**: 		Fix ID identifiying the order to be modified.
      * **result**: 		String indicating the result of this operation, that can be an error or "modify requested" if modification is in progress.


## Sample Content

Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.

It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English. Many desktop publishing packages and web page editors now use Lorem Ipsum as their default model text, and a search for 'lorem ipsum' will uncover many web sites still in their infancy. Various versions have evolved over the years, sometimes by accident, sometimes on purpose (injected humour and the like).


## More sample content

Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots in a piece of classical Latin literature from 45 BC, making it over 2000 years old. Richard McClintock, a Latin professor at Hampden-Sydney College in Virginia, looked up one of the more obscure Latin words, consectetur, from a Lorem Ipsum passage, and going through the cites of the word in classical literature, discovered the undoubtable source. Lorem Ipsum comes from sections 1.10.32 and 1.10.33 of "de Finibus Bonorum et Malorum" (The Extremes of Good and Evil) by Cicero, written in 45 BC. This book is a treatise on the theory of ethics, very popular during the Renaissance. The first line of Lorem Ipsum, "Lorem ipsum dolor sit amet..", comes from a line in section 1.10.32.

The standard chunk of Lorem Ipsum used since the 1500s is reproduced below for those interested. Sections 1.10.32 and 1.10.33 from "de Finibus Bonorum et Malorum" by Cicero are also reproduced in their exact original form, accompanied by English versions from the 1914 translation by H. Rackham.

There are many variations of passages of Lorem Ipsum available, but the majority have suffered alteration in some form, by injected humour, or randomised words which don't look even slightly believable. If you are going to use a passage of Lorem Ipsum, you need to be sure there isn't anything embarrassing hidden in the middle of text. All the Lorem Ipsum generators on the Internet tend to repeat predefined chunks as necessary, making this the first true generator on the Internet. It uses a dictionary of over 200 Latin words, combined with a handful of model sentence structures, to generate Lorem Ipsum which looks reasonable. The generated Lorem Ipsum is therefore always free from repetition, injected humour, or non-characteristic words etc.

{% include links.html %}
