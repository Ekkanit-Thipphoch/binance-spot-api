# CHANGELOG for Binance SPOT Testnet 

**Last Updated: 2024-11-05**

**Note:** All features here will only apply to the [SPOT Testnet](https://testnet.binance.vision/). 
This is not always synced with the live exchange. 

## 2024-11-05 

**Note:** This is in the process of being deployed. Please consult the Spot Test Network's [homepage](https://testnet.binance.vision/) to be informed of the release completion.

Changes to Exchange Information (i.e. [`GET /api/v3/exchangeInfo`](rest-api.md#exchangeInfo) from REST and [`exchangeInfo`](web-socket-api.md#exchangeInfo) for WebSocket API).

* A new optional parameter `showPermissionSets` can be used to hide the permissions from `permissionsSets`; This can be used for a reduced payload size.
* A new optional parameter `symbolStatus` can now be used to only show symbols with the specified status. (e.g. `TRADING`, `HALT`, `BREAK`)

---

## 2024-10-02 

REST and WebSocket API:

* Reminder that SBE 1:0 schema will be disabled on 2024-10-04, [6 months after being deprecated](../faqs/sbe_faq.md), as per our SBE policy.
* The [SBE lifecycle for Testnet](https://github.com/binance/binance-spot-api-docs/blob/master/sbe/schemas/sbe_schema_lifecycle_testnet.json) has been updated to reflect this change. 

---

## 2024-09-04 

* Spot Testnet now supports Unfilled Order Count. Please refer to this [page](../faqs/order_count_decrement.md) on how you can decrement your unfilled order count when placing orders.
* The documentation has been updated to reflect the wording.

---

## 2024-08-16

General Changes:

* New error messages have been added when quote quantity market orders (aka reverse market orders) are rejected in low-liquidity situations.

---

## 2024-08-07

* The [QuickFix schema](https://github.com/binance/binance-spot-api-docs/blob/master/fix/schemas/spot-fix-oe.xml) has been modified.

---

## 2024-07-23

**Note:** This will be deployed starting around 7am UTC. Please consult the Spot Test Network's [homepage](https://testnet.binance.vision/) to be informed of the release completion.

* FIX Drop Copy sessions are now supported on the Spot Test Network.
* New API Key permission `FIX_API_READ_ONLY` has been introduced.

---

## 2024-07-17 

General changes:

* Fixed a bug where klines had incorrect timestamps.
  * REST API: `GET /api/v3/klines` and `GET /api/v3/uiKlines` with `timeZone` parameter
  * WebSocket API: `klines` and `uiKlines` with `timeZone` parameter
  * WebSocket Streams: `<symbol>@kline_<interval>@+08:00` streams

---

## 2024-06-21

* [FIX API](fix-api.md) will be available today (2024-06-21) on the Spot Test Network. Please consult the Spot Test Network's [homepage](https://testnet.binance.vision/) to be informed of the release completion.
  * Using the FIX API requires an Ed25519 API Key with the `FIX_API` permission.
  * The release date on the live exchange has not been determined.

---

## 2024-06-05

WebSocket Streams

-  Buyer order ID (`b`) and Seller order ID (`a`) have been removed from the Trade streams. (i.e. `<symbol>@trade`)
-  To monitor if your order was part of a trade, please listen to the [User Data Streams](user-data-stream.md).

---

## 2024-05-30 

WebSocket API

* `wss://ws-api.testnet.binance.vision/ws-api/v3` is now the primary URL for the Spot Testnet WebSocket API. Other URLs will be phased out over time.

WebSocket Streams

* `wss://stream.testnet.binance.vision/ws` and `wss://stream.testnet.binance.vision/stream` are now the primary URLs for the Spot Testnet WebSocket Streams. Other URLs will be phased out over time.

---

## 2024-05-23 

REST API

* `orderRateLimitExceededMode` has been added to `POST /api/v3/order/cancelReplace`

WebSocket API

* `orderRateLimitExceededMode` has been added to `order.cancelReplace`

WebSocket Streams

* Kline/Candlestick streams can now support a UTC+8:00 timezone offset. (e.g. `btcusdt@kline_1d@+08:00`)

---

## 2024-05-02

* One-Triggers-the-Other (OTO) orders and One-Triggers-a-One-Cancels-The-Other (OTOCO) orders are now enabled.
* New requests have been added:
    * REST API:
        * `POST /api/v3/orderList/oto`
        * `POST /api/v3/orderList/otoco`
    * WebSocket API:
        * `orderList.place.oto`
        * `orderList.place.otoco`

---

## 2024-04-04 

General changes:

* Symbol permission information in Exchange Information responses has moved from field `permissions` to field `permissionSets`.
* Field `permissions` will be empty and will be removed in a future release.
* Previously, `"permissions":["SPOT","MARGIN"]` meant that you could place an order on the symbol if your account had `SPOT` or `MARGIN` permissions. The equivalent is `"permissionSets":[["SPOT","MARGIN"]]`. (Note the extra set of square brackets.) Each array of permissions inside the `permissionSets` array is called a "permission set".
* Symbol permissions can now be more complex. `"permissionSets":[["SPOT","MARGIN"],["TRD_GRP_004","TRD_GRP_005"]]` means that you may place an order on the symbol if your account has SPOT or MARGIN permissions **and** `TRD_GRP_004` or `TRD_GRP_005` permissions. There may be an arbitrary number of permission sets in a symbol's `permissionSets`.
* **The weight of the following requests has increased from 10 to 25**: 
	* `GET /api/v3/trades`
	* `GET /api/v3/historicalTrades`
	* `trades.recent`
	* `trades.historical`

REST API

* The `POST /api/v3/order/oco` endpoint is now deprecated on the REST API. You should use the new `POST /api/v3/orderList/oco` endpoint instead. Note that this new endpoint uses different parameters.
* `POST /api/v3/order/oco` has been removed from the Rest API documentation for SPOT Testnet.
* `otoAllowed` will now appear on `GET /api/v3/exchangeInfo`, that indicates if One-Triggers-the-Other (OTO) orders are supported on that symbol.

WebSocket API

* The `orderList.place` request is now deprecated on the WebSocket API. You should now use the new `orderList.place.oco` request instead. Note that this new request uses different parameters.
* `orderList.place` has been removed from the WebSocket API documentation for SPOT Testnet.
* `otoAllowed` will now appear on `exchangeInfo`, that indicates if One-Triggers-the-Other (OTO) orders are supported on that symbol.

SBE

* A new schema 2:0 [spot_2_0.xml](https://github.com/binance/binance-spot-api-docs/blob/master/sbe/schemas/spot_2_0.xml) has been released for SPOT Testnet. The current schema, 1:0 [spot_1_0.xml](https://github.com/binance/binance-spot-api-docs/blob/becd4d44a09d94821d2dc761ba9197aae8b495c3/sbe/schemas/spot_1_0.xml), will thus be deprecated and retired from the Testnet APIs in 6 months as per our schema deprecation policy.
* When using schema 1:0 on REST API or WebSocket API, group "permissions" in message "ExchangeInfoResponse" will always be empty. Upgrade to schema 2:0 to find permission information in group "permissionSets". See General changes above for more details.
* Responses for deprecated OCO requests are supported by both schema 1:0 and 2:0



---

## 2024-03-13

General changes:

* `GET /api/v3/account` has a new optional parameter `omitZeroBalances`, allowing to hide all zero balances
* `account.status` has a new optional parameter `omitZeroBalances` allowing to hide all zero balances.


User Data Stream:

* New event `listenKeyExpired` is now emitted when a `listenKey` expires.

