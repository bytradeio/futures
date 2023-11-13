 <h1 class="curproject-name"> futures-open </h1> 

 futures open api

# Websocket Public API

## Market KLine
<a id=klines> </a>

**Sub**

```
 {"method":"subscribe.kline","params":{"market":"BTCUSDT","period":"15min"},"id":6288}
```

**UbSub**

```
 {"method":"unsubscribe.kline","params":{"market":"BTCUSDT","period":"15min"},"id":6288}
```

## Market Transaction
<a id=deals> </a>

**Sub**

```
{"method":"subscribe.trade","id":1564,"params":{"market":"BTCUSDT"}}
```

**UbSub**

```
{"method":"unsubscribe.trade","id":1564,"params":{"market":"BTCUSDT"}}
```



## Market Depth
<a id=Market Depth> </a>

**Sub**

```
{"method":"subscribe.depth","id":1477,"params":{"market":"BTCUSDT","merge":"0"}}
```

**UbSub**

```
{"method":"unsubscribe.depth","id":1477,"params":{"market":"BTCUSDT","merge":"0"}}
```

## Market Quotes
<a id=State> </a>

**Sub**

```
{"method":"subscribe.state","id":1953,"params":{}}
```

**UbSub**

```
{"method":"unsubscribe.state","id":1953,"params":{}}
```

## Heartbeat
<a id=Ping> </a>

```
{"method":"ping"}
```

# Websocket Private API



## User Positions
<a id=userPositions> </a>

**Sub**

```
{"method":"subscribe.position","id":8538,"params":{}}
```

**UbSub**

```
{"method":"unsubscribe.position","id":8538,"params":{}}
```

## User Order
**Sub**

```
{"method":"subscribe.order","id":8538,"params":{}}
```

**UbSub**

```
{"method":"unsubscribe.order","id":8538,"params":{}}
```

## User Assets
**Sub**

```
{"method":"subscribe.asset","id":8538,"params":{}}
```

**UbSub**

```
{"method":"unsubscribe.asset","id":8538,"params":{}}
```

## Signature

```
{"method":"subscribe.sign","id":8538
"params": {
            "client_id": you_client_id,
            "nonce": nonce,
            "ts": ts,
            "sign": sign
        },
}
```



## Python Sample Code

```
# -*- coding:utf-8 -*-
import datetime
import time
import hmac
import hashlib
import websockets
import json
import asyncio
import uvloop

asyncio.set_event_loop_policy(uvloop.EventLoopPolicy())

host = "baseurl"
client_id = ""
client_key = ""


async def ping(ws):
    obj = {"method": "ping"}
    op = json.dumps(obj)
    await ws.send(op)


def sign():
    ts = str(int(time.time()))
    nonce = "abcdefg"
    s = "client_id=%s&nonce=%s&ts=%s" % (client_id, nonce, ts)
    v = hmac.new(client_key.encode(), s.encode(), digestmod=hashlib.sha256)
    obj = {
        "method": "sign",
        "params": {
            "client_id": client_id,
            "nonce": nonce,
            "ts": str(ts),
            "sign": v.hexdigest()
        },
        "id": 0,
    }
    return obj


async def depth(ws):
    obj = {
        "method": "subscribe.depth",
        "params": {
            "market": "BTCUSDT",
            "merge": "0",
        },
        "id": 0,
    }
    print(obj)
    await ws.send(json.dumps(obj))

async def user_asset(ws):
    obj = {
        "method": "subscribe.asset",
        "id": 0,
    }
    print(obj)
    await ws.send(json.dumps(obj))


async def startup():
    print("start to connect %s..." % host)
    ws = await websockets.connect(host)

    await ws.send(json.dumps(sign()))
    await ping(ws)
    await depth(ws)
    await user_asset(ws)

    while 1:
        try:
            data = await ws.recv()
            print(datetime.datetime.now(), data)
        except websockets.exceptions.ConnectionClosed as e:
            print("connect closed...", e)
            return
        except:
            pass


if __name__ == "__main__":
    loop = asyncio.get_event_loop()
    loop.run_until_complete(startup())

```

# openv2 Publish API

## Get Market K-line
<a id=get-market-kline> </a>
### Basic Information

**Path：** /open/api/v2/market/kline

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark                                                                  |
| ------------ |----------| ------------ |-------------------------------------------------------------------------|
| market | Yes      |  ETHUSDT | Market name                                                             |
| type | Yes      |  1min | K-line period,1min,5min,15min,30min,1hour,4hour,6hour,12hour,1day,1week |
| start_time | No       |  1697617535 | Start time                                                              |
| end_time | No       |  1697627535 | End time                                                                |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": [
    [
      1697560680,
      "1571.23",
      "1571.23",
      "1571.23",
      "1571.23",
      "0",
      "0",
      "ETHUSDT"
    ]
  ]
}
```
## Get Market List
<a id=get-market-list> </a>
### Basic Information

**Path：** /open/api/v2/market/list

**Method：** GET

**API Description：**


### Request Parameters

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "type": 1,
      "leverages": [
        "3",
        "5",
        "8",
        "10",
        "15",
        "20",
        "30",
        "50",
        "100"
      ],
      "name": "BTCUSTT",
      "stock": "BTC",
      "money": "USTT",
      "fee_prec": 5,
      "tick_size": "0.01",
      "stock_prec": 8,
      "money_prec": 2,
      "amount_prec": 4,
      "amount_min": "0.0001",
      "available": true,
      "limits": [
        [
          "2500.0001",
          "3",
          "0.15"
        ],
        [
          "2000.0001",
          "5",
          "0.1"
        ],
        [
          "1500.0001",
          "8",
          "0.063"
        ],
        [
          "1000.0001",
          "10",
          "0.03"
        ],
        [
          "500.0001",
          "15",
          "0.025"
        ],
        [
          "250.0001",
          "20",
          "0.02"
        ],
        [
          "100.0001",
          "30",
          "0.015"
        ],
        [
          "50.0001",
          "50",
          "0.01"
        ],
        [
          "20.0001",
          "100",
          "0.005"
        ]
      ],
      "sort": 1
    }
  ]
}
```
## Get Market Transactions
<a id=get-market-transactions> </a>
### Basic Information

**Path：** /open/api/v2/market/deals

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark  |
| ------------ |----------| ------------ | ------------ |
| market | Yes      |  ETHUSDT |   |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "id": 27699216,
      "price": "1573.89",
      "amount": "0.922",
      "type": "buy",
      "time": 1697619536.256684
    }
  ]
}
```
## Get Market Depth
<a id=get-market-depth> </a>
### Basic Information

**Path：** /open/api/v2/market/depth

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark           |
| ------------ |----------| ------------ |------------------|
| market | Yes      |  ETHUSDT | Market name      |
| merge | No       |  0 | Merge depth，0，0.01，0.02 |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "index_price": "1577.63",
    "sign_price": "1581.5",
    "time": 1697620709569,
    "last": "1573.89",
    "asks": [
      [
        "1621.22",
        "30.613"
      ]
    ],
    "bids": [
      [
        "1573.84",
        "0.819"
      ]
    ]
  }
}
```
## Get Market Status
<a id=get-market-status> </a>
### Basic Information

**Path：** /open/api/v2/market/state

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark   |
| ------------ |----------| ------------ |----------|
| market | Yes      |  ETHUSDT | Market name |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "market": "ETHUSDT",
    "amount": "4753.05",
    "high": "1573.89",
    "last": "1573.89",
    "low": "1571.23",
    "open": "1571.23",
    "change": "0.0016929411989333",
    "period": 86400,
    "volume": "3.02",
    "funding_time": 400,
    "position_amount": "2.100",
    "funding_rate_last": "0.00375",
    "funding_rate_next": "0.00293873",
    "funding_rate_predict": "-0.00088999",
    "insurance": "10500.45426906585552617850",
    "sign_price": "1581.98",
    "index_price": "1578.12",
    "sell_total": "112.974",
    "buy_total": "170.914"
  }
}
```
## Get All Market Status
<a id=get-all-market-status> </a>
### Basic Information

**Path：** /open/api/v2/market/state/all

**Method：** GET

**API Description：**


### Request Parameters

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "1000SHIBUSDT": {
      "market": "1000SHIBUSDT",
      "amount": "0",
      "high": "0.007072",
      "last": "0.007072",
      "low": "0.007072",
      "open": "0.007072",
      "change": "0",
      "period": 86400,
      "volume": "0",
      "funding_time": 400,
      "position_amount": "0",
      "funding_rate_last": "0.00375",
      "funding_rate_next": "0.00375",
      "funding_rate_predict": "0.00375",
      "insurance": "10500.45426906585552617850",
      "sign_price": "0.006919",
      "index_price": "0.006898",
      "sell_total": "6079974",
      "buy_total": "8153875"
    }
  }
}
```
# openv2 Private API

## Adjust Position Margin
<a id=adjust-position-margin> </a>
### Basic Information

**Path：** /open/api/v2/position/margin

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> type</span></td><td key=1><span>integer</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Adjustment type: 1 means increase margin, 2 means decrease margin</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> quantity</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>100</td><td key=4><span style="white-space: pre-wrap">Adjustment limit</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{"code": 0, "msg": "success", "data": {"position_id": 4750887, "profit_unreal": "9.0806004045", "finish_type": 1, "liq_time": 0, "create_time": 1697619536.255882, "update_time": 1697620824.557015, "mainten_margin": "0.02", "user_id": 9108, "market": "ETHUSDT", "adl_sort_val": "0.01254794", "type": 1, "liq_price": "1525.67027360000000000644", "side": 2, "open_val_max": "1573.88688000000000000000", "sys": 0, "margin_amount": "79.69434400000000000000", "amount": "1.000", "open_margin": "0.05063536967790213741", "deal_asset_fee": "0", "amount_max": "1.000", "close_left": "1.000", "open_price": "1573.88688000000000000000", "open_val": "1573.88688000000000000000", "open_margin_imply": "0", "mainten_margin_amount": "31.47773760000000000000", "leverage": "20", "profit_real": "-0.78694344000000000000", "profit_clearing": "-0.78694344000000000000", "liq_order_time": 0, "liq_amount": "0", "liq_profit": "0", "liq_order_price": "0", "fee_asset": "", "bkr_price": "1494.19253600000000000644", "liq_price_imply": "0", "bkr_price_imply": "0", "adl_sort": 1, "total": 5}}
```
## Get User Transaction
<a id=get-user-transaction> </a>
### Basic Information

**Path：** /open/api/v2/order/deals

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | Yes      |  ETHUSDT | Market name |
| start_time | No       |  1697619536 | Start time  |
| end_time | No       |  1697619536 | End time    |
| page | Yes      |  1 | Page        |
| page_size | Yes      |  10 | Page size   |

### Response Data

```javascript
{"code": 0, "msg": "success", "data": {"records": [{"position_type": 1, "market": "ETHUSDT", "time": 1697619536.256684, "side": 2, "leverage": "20", "user_id": 9108, "order_id": 1470801730, "role": 2, "amount": "0.922", "price": "1573.89", "deal_fee": "0.7255", "deal_stock": "1451.1265", "filled_type": 1, "trade_type": 1, "deal_profit": "--"}, {"position_type": 1, "market": "ETHUSDT", "time": 1697619536.255877, "side": 2, "leverage": "20", "user_id": 9108, "order_id": 1470801730, "role": 2, "amount": "0.078", "price": "1573.85", "deal_fee": "0.0613", "deal_stock": "122.7603", "filled_type": 1, "trade_type": 1, "deal_profit": "--"}, {"position_type": 1, "market": "ETHUSDT", "time": 1697619520.667859, "side": 1, "leverage": "20", "user_id": 9108, "order_id": 1470799856, "role": 2, "amount": "0.01", "price": "1573.84", "deal_fee": "0.0078", "deal_stock": "15.7384", "filled_type": 1, "trade_type": 2, "deal_profit": "-0.0001"}, {"position_type": 1, "market": "ETHUSDT", "time": 1697619451.950652, "side": 2, "leverage": "20", "user_id": 9108, "order_id": 1470791802, "role": 2, "amount": "0.01", "price": "1573.85", "deal_fee": "0.0078", "deal_stock": "15.7385", "filled_type": 1, "trade_type": 1, "deal_profit": "--"}], "page": 1, "page_size": 10, "count": 0}}

```
## Get Completed Orders
<a id=get-completed-orders> </a>
### Basic Information

**Path：** /open/api/v2/order/finished

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | Yes      |  ETHUSDT | Market name |
| start_time | No       |  1697617535 | Start time  |
| end_time | No       |  1697617535 | End time    |
| page | Yes      |  1 | Page        |
| page_size | Yes      |  10 | Page size   |

### Response Data

```javascript
{"code": 0, "msg": "success", "data": {"records": [{"order_id": 1470801730, "position_id": 0, "market": "ETHUSDT", "type": 2, "side": 2, "left": "0", "amount": "1", "filled": "1", "deal_fee": "0.7869", "price": "0", "avg_price": "1573.88", "deal_stock": "1573.8868", "position_type": 1, "leverage": "20", "update_time": 1697619536.256684, "create_time": 1697619536.255875, "status": 3, "stop_loss_price": "-", "take_profit_price": "-"}, {"order_id": 1470799856, "position_id": 0, "market": "ETHUSDT", "type": 2, "side": 1, "left": "0", "amount": "0.01", "filled": "0.01", "deal_fee": "0.0078", "price": "0", "avg_price": "1573.84", "deal_stock": "15.7384", "position_type": 1, "leverage": "20", "update_time": 1697619520.667859, "create_time": 1697619520.667853, "status": 3, "stop_loss_price": "-", "take_profit_price": "-"}, {"order_id": 1470791802, "position_id": 0, "market": "ETHUSDT", "type": 2, "side": 2, "left": "0", "amount": "0.01", "filled": "0.01", "deal_fee": "0.0078", "price": "0", "avg_price": "1573.85", "deal_stock": "15.7385", "position_type": 1, "leverage": "20", "update_time": 1697619451.950652, "create_time": 1697619451.950649, "status": 3, "stop_loss_price": "-", "take_profit_price": "-"}], "page": 1, "page_size": 10, "count": 0}}

```
## Market Order
<a id=market-order> </a>
### Basic Information

**Path：** /open/api/v2/order/market

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value             | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> side</span></td><td key=1><span>integer</span></td><td key=2>Yes</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Side，1 sell，2 buy</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> quantity</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Quantity</span></td><td key=5></td></tr><tr key=0-3><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> client_oid</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3></td><td key=4><span style="white-space: pre-wrap">User-defined order id</span></td><td key=5></td></tr><tr key=0-4><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> is_stop</span></td><td key=1><span>boolean</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Is it a take profit order or a stop loss order?</span></td><td key=5></td></tr><tr key=0-5><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_loss_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Stop loss price</span></td><td key=5></td></tr><tr key=0-6><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_loss_price_type</span></td><td key=1><span>integer</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Stop price type，1 last price，2 index price，3 sign price</span></td><td key=5></td></tr><tr key=0-7><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> take_profit_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Take profit price</span></td><td key=5></td></tr><tr key=0-8><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> take_profit_price_type</span></td><td key=1><span>integer</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">Take profit price type，1 last price，2 index price，3 sign price</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": 1471038436
}
```
## Cancel all orders in a single market
<a id=cancel-all-orders-in-a-single-market> </a>
### Basic Information

**Path：** /open/api/v2/order/cancel/all

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{"code": 0, "msg": "success", "data": null}

```
## Get Order Details
<a id=get-order-details> </a>
### Basic Information

**Path：** /open/api/v2/order/detail

**Method：** GET

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | Yes      |  ETHUSDT | Market name |
| order_id | Yes      |  23523523 | Order id    |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> </span></td><td key=1><span></span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{"code": 0, "msg": "success", "data": {"order_id": 1470445037, "position_id": 0, "market": "ETHUSDT", "type": 2, "side": 1, "left": "0", "amount": "1", "filled": "1", "deal_fee": "0.7869", "price": "0", "avg_price": "1573.84", "deal_stock": "1573.84", "position_type": 1, "leverage": "20", "update_time": 1697616547.90107, "create_time": 1697616547.901067, "status": 3, "stop_loss_price": "-", "take_profit_price": "-"}}

```
## Cancel Order
<a id=cancel-order> </a>
### Basic Information

**Path：** /open/api/v2/order/cancel

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> order_id</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>23523523</td><td key=4><span style="white-space: pre-wrap">Order id</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{"code": 0, "msg": "success", "data": 1471202902}

```
## Submit Limit Order
<a id=submit-limit-order> </a>
### Basic Information

**Path：** /open/api/v2/order/limit

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> side</span></td><td key=1><span>integer</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Side，1 sell，2 buy</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> price</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>21421</td><td key=4><span style="white-space: pre-wrap">Price</span></td><td key=5></td></tr><tr key=0-3><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> quantity</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Quantity</span></td><td key=5></td></tr><tr key=0-4><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> client_oid</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">User-defined order number</span></td><td key=5></td></tr><tr key=0-5><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> is_stop</span></td><td key=1><span>boolean</span></td><td key=2>No</td><td key=3>true</td><td key=4><span style="white-space: pre-wrap">Is it a take profit order or a stop loss order</span></td><td key=5></td></tr><tr key=0-6><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_loss_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>121421</td><td key=4><span style="white-space: pre-wrap">Stop loss price</span></td><td key=5></td></tr><tr key=0-7><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_loss_price_type</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Stop loss order price type，1 last price，2 index price，3 sign price</span></td><td key=5></td></tr><tr key=0-8><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> take_profit_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>1241252</td><td key=4><span style="white-space: pre-wrap">Take profit order price</span></td><td key=5></td></tr><tr key=0-9><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> take_profit_price_type</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Take profit order price type，1 last price，2 index price，3 sign price</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{"code": 0, "msg": "success", "data": 1471038436}
```
## Get the entrusted order
<a id=get-the-entrusted-order> </a>
### Basic Information

**Path：** /open/api/v2/order/pending

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | No       |  ETHUSDT | Market name |
| page | Yes      |  1 | Page        |
| page_size | Yes      |  10 | Page size   |

### Response Data

```javascript
{"code": 0, "msg": "success", "data": {"records": [{"order_id": 1471038436, "position_id": 0, "market": "ETHUSDT", "type": 1, "side": 1, "left": "1.000", "amount": "1.000", "filled": "0", "deal_fee": "0", "price": "1700", "avg_price": "", "deal_stock": "0", "position_type": 1, "leverage": "20", "update_time": 1697621580.978632, "create_time": 1697621580.978632, "status": 1, "stop_loss_price": "-", "take_profit_price": "-"}], "page": 1, "page_size": 10, "count": 1}}

```
## Submit stop order
<a id=submit-stop-order> </a>
### Basic Information

**Path：** /open/api/v2/order/stop

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> side</span></td><td key=1><span>integer</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Side，1 sell，2 buy</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> order_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>222</td><td key=4><span style="white-space: pre-wrap">Order price，If left blank, the order will be placed based on the market price.</span></td><td key=5></td></tr><tr key=0-3><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_price</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>222</td><td key=4><span style="white-space: pre-wrap">Stop price</span></td><td key=5></td></tr><tr key=0-4><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> cut_price</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>222</td><td key=4><span style="white-space: pre-wrap">Last market price</span></td><td key=5></td></tr><tr key=0-5><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_price_type</span></td><td key=1><span>integer</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Price type，1 last price，2 index price，3 sign price</span></td><td key=5></td></tr><tr key=0-6><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> quantity</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Quantity</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{"code": 0, "msg": "success", "data": null}

```
## Cancel stop order
<a id=cancel-stop-order> </a>
### Basic Information

**Path：** /open/api/v2/order/stop/cancel

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> order_id</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>235235235235235235235</td><td key=4><span style="white-space: pre-wrap">Order id</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": 1471202902
}
```
## Cancel all stop orders for a single market
<a id=cancel-all-stop-orders-for-a-single-market> </a>
### Basic Information

**Path：** /open/api/v2/order/stop/cancel/all

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": null
}
```
## Get the stop order in the commission
<a id=get-the-stop-order-in-the-commission> </a>
### Basic Information

**Path：** /open/api/v2/order/stop/pending

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | No       |  ETHUSDT | Market name |
| page | Yes      |  1 | Page        |
| page_size | Yes      |  10 | Page size   |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "contract_order_id": "1693809549574678795",
        "order_id": 0,
        "position_id": 0,
        "market": "BTCUSDT",
        "contract_order_type": 0,
        "trigger_type": 3,
        "trigger_price": "30000",
        "order_price": "0",
        "size": "1",
        "order_time": 1693809549,
        "update_time": 1693809549,
        "cut_price": "",
        "status": 2,
        "side": 1,
        "position_type": 1,
        "leverage": "100",
        "entrust_id": "gyWTY4NgLg"
      }
    ],
    "page": 1,
    "page_size": 1,
    "count": 2
  }
}
```
## Get completed stop order
<a id=get-completed-stop-order> </a>
### Basic Information

**Path：** /open/api/v2/order/stop/finished

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | No       |  ETHUSDT | Market name |
| page | Yes      |  1 | Page        |
| page_size | Yes      |  10 | Page size   |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "contract_order_id": "1693479864228767250",
        "order_id": 0,
        "position_id": 0,
        "market": "BTCUSDT",
        "contract_order_type": 0,
        "trigger_type": 3,
        "trigger_price": "38000",
        "order_price": "0",
        "size": "1",
        "order_time": 1693479864,
        "update_time": 1693479966,
        "cut_price": "",
        "status": 4,
        "side": 1,
        "position_type": 1,
        "leverage": "100",
        "entrust_id": "gxf0Qpu1WE"
      }
    ],
    "page": 1,
    "page_size": 10,
    "count": 6
  }
}
```
## Adjust market opening leverage/position mode
<a id=adjust-market-opening-leverage-position-mode> </a>
### Basic Information

**Path：** /open/api/v2/setting/leverage

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> leverage</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>10</td><td key=4><span style="white-space: pre-wrap">Leverage</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> position_type</span></td><td key=1><span>integer</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Position type，1 Isolated position 2 Cross position</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": null
}
```
## Query the market opening leverage/position mode
<a id=query-the-market-opening-leverage-position-mode> </a>
### Basic Information

**Path：** /open/api/v2/setting/leverage

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | Yes      |  ETHUSDT | Market name |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "leverage": "100",
    "position_type": 1
  }
}
```
## Query Assets
<a id=query-assets> </a>
### Basic Information

**Path：** /open/api/v2/asset/query

**Method：** GET

**API Description：**


### Request Parameters

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "USDT": {
      "available": "8300.569",
      "frozen": "0",
      "margin": "0",
      "balance_total": "8300.569",
      "profit_unreal": "0",
      "transfer": "8300.569",
      "bonus": "0"
    },
    "USTT": {
      "available": "99808254.3273",
      "frozen": "0",
      "margin": "0",
      "balance_total": "99808254.3273",
      "profit_unreal": "0",
      "transfer": "99808254.3273",
      "bonus": "0"
    }
  }
}
```
## Query Asset Bill
<a id=query-asset-bill> </a>
### Basic Information

**Path：** /open/api/v2/asset/history

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark     |
| ------------ |----------| ------------ |------------|
| asset | No       |  USDT | Asset name |
| business | No       |  fee | Business   |
| start_time | No       |  1693496496 | Start time |
| end_time | No       |  1693496496 | End time   |
| page | Yes      |  1 | Page       |
| page_size | Yes      |  10 | Page size  |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "records": [
      {
        "id": 1562,
        "time": 1693496496.157132,
        "user_id": 9108,
        "asset": "USDT",
        "business": "Close pnl",
        "change": "-482.541",
        "balance": "8300.569"
      }
    ],
    "page": 1,
    "page_size": 1,
    "count": 406
  }
}
```
## User Positions
<a id=user-positions> </a>
### Basic Information

**Path：** /open/api/2/position/pending

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| market | No       |  ETHUSDT | Market name |

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": [
    {
      "position_id": 2497913,
      "create_time": 1693809270.599202,
      "update_time": 1693809270.599314,
      "user_id": 9108,
      "market": "BTCUSDT",
      "type": 1,
      "side": 2,
      "amount": "1.0000",
      "close_left": "1.0000",
      "open_price": "25992.1",
      "open_margin": "0.01",
      "margin_amount": "259.921",
      "leverage": "100",
      "profit_unreal": "-18.5706",
      "liq_price": "25862.13",
      "mainten_margin": "0.005",
      "mainten_margin_amount": "129.9605",
      "adl_sort": 5,
      "roe": "-0.0714",
      "stop_loss_price": "-",
      "take_profit_price": "-"
    }
  ]
}
```
## Get adjustable margin
<a id=get-adjustable-margin> </a>
### Basic Information

**Path：** /open/api/2/position/margin

**Method：** GET

**API Description：**


### Request Parameters
**Query**

| Parameter Name  | Required | Example  | Remark      |
| ------------ |----------| ------------ |-------------|
| position_id | Yes      |  124 | Position id |
| market | Yes      |  ETHUSDT | Market name |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> </span></td><td key=1><span></span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap"></span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": {
    "amount": "1.0000",
    "available": "8410.56909378778272067016",
    "leverage": "100",
    "margin_amount": "372.541",
    "max_removable_margin": "64.7714395551"
  }
}
```
## Limit Close
<a id=limit-close> </a>
### Basic Information

**Path：** /open/api/v2/position/close/limit

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> position_id</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>11</td><td key=4><span style="white-space: pre-wrap">Position id</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> quantity</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Closing quantity</span></td><td key=5></td></tr><tr key=0-3><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> price</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>24214</td><td key=4><span style="white-space: pre-wrap">Closing price</span></td><td key=5></td></tr><tr key=0-4><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> client_oid</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3></td><td key=4><span style="white-space: pre-wrap">User-defined order id</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": 325235235
}
```
## Market Close
<a id=market-close> </a>
### Basic Information

**Path：** /open/api/v2/position/close/market

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> position_id</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>111</td><td key=4><span style="white-space: pre-wrap">Position id</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> client_oid</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>12421412</td><td key=4><span style="white-space: pre-wrap">User-defined order id</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": 235235325
}
```
## Position Take Profit And Stop Loss Setting/Modification
<a id=Position-take-profit-and-stop-loss-setting-modification/> </a>
### Basic Information

**Path：** /open/api/v2/position/close/stop

**Method：** POST

**API Description：**


### Request Parameters
**Headers**

| Parameter Name  | Value            | Required | Example  | Remark  |
| ------------ |------------------|----------| ------------ | ------------ |
| Content-Type  | application/json | Yes      |   |   |
**Body**

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>Name</th><th key=type>Type</th><th key=required>Required</th><th key=default>Default Value</th><th key=desc>Remark</th><th key=sub>Other</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> market</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>ETHUSDT</td><td key=4><span style="white-space: pre-wrap">Market name</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> position_id</span></td><td key=1><span>string</span></td><td key=2>Yes</td><td key=3>1111</td><td key=4><span style="white-space: pre-wrap">Position id</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_loss_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>1242</td><td key=4><span style="white-space: pre-wrap">Stop loss price</span></td><td key=5></td></tr><tr key=0-3><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> stop_loss_price_type</span></td><td key=1><span>integer</span></td><td key=2>No</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Stop loss price type</span></td><td key=5></td></tr><tr key=0-4><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> take_profit_price</span></td><td key=1><span>string</span></td><td key=2>No</td><td key=3>422</td><td key=4><span style="white-space: pre-wrap">Take profit price</span></td><td key=5></td></tr><tr key=0-5><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> take_profit_price_type</span></td><td key=1><span>integer</span></td><td key=2>No</td><td key=3>1</td><td key=4><span style="white-space: pre-wrap">Take profit price type</span></td><td key=5></td></tr>
               </tbody>
              </table>

### Response Data

```javascript
{
  "code": 0,
  "msg": "success",
  "data": null
}
```

## Python Example Code

```
# -*- coding:utf-8 -*-
import json
import requests
import time
import hmac
import hashlib

host = "baseurl"
client_id = ""
client_key = ""


def gen_sign(client_id, client_key):
    ts = int(time.time())
    nonce = "abcdefg"
    obj = {"ts": ts, "nonce": nonce, "sign": "", "client_id": client_id}
    s = "client_id=%s&nonce=%s&ts=%s" % (client_id, nonce, ts)
    v = hmac.new(client_key.encode(), s.encode(), digestmod=hashlib.sha256)
    obj["sign"] = v.hexdigest()
    return obj

# market api
def market_list():
    print("> market list")
    path = "/market/list"
    res = requests.get(host + path)
    print(json.dumps(res.json()))

# setting api
def adjust_leverage(market: str, leverage: int, position_type: int):
    print("> adjust leverage")
    path = "/setting/leverage"
    obj = gen_sign(client_id, client_key)
    obj.update({"market": market, "leverage": leverage, "position_type": position_type})
    res = requests.post(host + path, data=obj)
    print(json.dumps(res.json()))
    
# asset api
def get_asset():
    print(">get asset")
    path = "/asset/query"
    obj = gen_sign(client_id, client_key)
    res = requests.get(host + path, params=obj)
    print(json.dumps(res.json()))

# order api
def put_limit_order(
        market: str,
        side: int,
        price: str,
        quantity: str,
        client_oid: str,
        is_stop: bool,
        stop_loss_price: str,
        stop_loss_price_type: int,
        take_profit_price: str,
        take_profit_price_type: int
):
    print("> put limit order")
    path = "/order/limit"
    obj = gen_sign(client_id, client_key)
    obj.update(
        {
            "market": market,
            "side": side,
            "price": price,
            "quantity": quantity,
            "client_oid": client_oid,
            "is_stop": is_stop,
            "stop_loss_price": stop_loss_price,
            "stop_loss_price_type": stop_loss_price_type,
            "take_profit_price": take_profit_price,
            "take_profit_price_type": take_profit_price_type,
        }
    )
    res = requests.post(host + path, data=obj)
    print(json.dumps(res.json()))

```