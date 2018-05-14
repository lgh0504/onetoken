RESTful API
===

### 历史数据

OneToken提供主要交易所的现货和期货的历史Tick级别数据，支持JSON及CSV格式。

Host URL
---
```
http://alihz-net-0.qbtrade.org
```

Contracts
---
Get supported contracts of specified date and format.

```$xslt
GET /historical-quote/hist-ticks
params: date={date}&format={json|csv}
```
Response is a list of available contracts.
```$xslt
[
"bigone/1st.btc",
"bittrex/1st.btc",
"hitbtc/1st.btc",
...
]
```
## Example

http://alihz-net-0.qbtrade.org/contracts?date=2018-01-02

Hist-Ticks
---
Get historical ticks of specified contract, date and format.

```$xslt
GET /historical-quote/hist-ticks
```
Response is a **gzip** file of daily ticks.

CSV format, '|' is the delimiter:
```$xslt
time|last|volume|bids|asks|contract|source|bs
2018-01-02T00:01:53.352549+08:00|0.0038349|3.0|[{"price": 0.0037978, "volume": 1136.0}, {"price": 0.0037976, "volume": 3.0}, {"price": 0.003759, "volume": 8.0}, {"price": 0.0037572, "volume": 4.0}, {"price": 0.003744, "volume": 8.0}, {"price": 0.0037339, "volume": 400.0}, {"price": 0.0037338, "volume": 1705.0}, {"price": 0.003729, "volume": 8.0}, {"price": 0.0037221, "volume": 388.0}, {"price": 0.003714, "volume": 8.0}, {"price": 0.003699, "volume": 8.0}, {"price": 0.003698, "volume": 50.0}, {"price": 0.0036947, "volume": 20.0}, {"price": 0.0036904, "volume": 73.0}, {"price": 0.003685, "volume": 8.0}, {"price": 0.0036821, "volume": 21.0}, {"price": 0.00367, "volume": 8.0}, {"price": 0.0036634, "volume": 437.0}, {"price": 0.0036632, "volume": 4.0}, {"price": 0.00366, "volume": 1300.0}]|[{"price": 0.0038745, "volume": 1166.0}, {"price": 0.0038746, "volume": 94.0}, {"price": 0.0038977, "volume": 400.0}, {"price": 0.0038979, "volume": 117.0}, {"price": 0.0039, "volume": 35.0}, {"price": 0.0039114, "volume": 44.0}, {"price": 0.003913, "volume": 7.0}, {"price": 0.003915, "volume": 44.0}, {"price": 0.0039264, "volume": 4.0}, {"price": 0.003929, "volume": 7.0}, {"price": 0.003944, "volume": 7.0}, {"price": 0.0039468, "volume": 251.0}, {"price": 0.00395, "volume": 10.0}, {"price": 0.00396, "volume": 7.0}, {"price": 0.0039694, "volume": 3288.0}, {"price": 0.003976, "volume": 7.0}, {"price": 0.00398, "volume": 100.0}, {"price": 0.00399, "volume": 1451.0}, {"price": 0.003992, "volume": 7.0}, {"price": 0.003999, "volume": 28.0}]|adx.eth:xtc.binance|pubsub|
...
```
JSON format, each line is a JSON object:
每一行数据都是JSON对象
bids/买单 按价格从高到低排序 asks/卖单 按价格从低到高排序 深度不同交易所不同 一般有20档
last 最新成交价
time isoformat的时间 (不保证是utc+8时区)
volume 是上一个tick到这一个tick之间的成交量 按标的物计算 比如 eth.btc 交易对 就是成交的eth的数量
amount 是tick换算成的成交额(按人民币计价)
```$xslt
{
  "amount": 54.034660536764,
  "asks": [
    {
      "price": 0.0038745,
      "volume": 1166
    },
    {
      "price": 0.0038746,
      "volume": 94
    },
    ...
  ],
  "bids": [
    {
      "price": 0.0037978,
      "volume": 1136
    },
    {
      "price": 0.0037976,
      "volume": 3
    },
    ...
  ],
  "contract": "adx.eth:xtc.binance",
  "last": 0.0038349,
  "time": "2018-01-02T00:01:53.352549+08:00",
  "volume": 3
}
```

## Examples

http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okex/ace.btc&format=json

http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okex/ace.btc&format=csv

### 行情

### 交易

Restful host is `https://1token.trade/api/v1/trade`

API Explorer(https://1token.trade/r/swagger/index)