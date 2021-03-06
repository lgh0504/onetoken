
# 主要交易所的现货和期货的历史tick以及zhubi数据

## 教程
* __Tick数据__
  * 首先从api 获取某一天的所有交易对的列表 [http://alihz-net-0.qbtrade.org/contracts?date=2018-01-02](http://alihz-net-0.qbtrade.org/contracts?date=2018-01-02)
  * 选好想要的下载的交易对 比如 okex/btc.usdt 以及 okef/btc.usd.q
  * 合约命名规则 交易所symbol 可以参考 [支持交易所](exchange-overview)
  * 使用 /hist-tick 接口 下载特定日期 指定交易对的数据
    * [http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okex/btc.usdt](http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okex/btc.usdt)
    * [http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okef/btc.usd.q](http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okef/btc.usd.q)
* **Zhubi数据**
  * 选好想要的下载的交易对 比如 okex/btc.usdt 以及 okef/btc.usd.q
  * 合约命名规则 交易所symbol 可以参考 [支持交易所](exchange-overview)
  * 使用 /zhubi 接口来获得数据，可以指定时间区间、指定交易对以及本次请求的最大zhubi数目。
    * https://1token.trade/api/v1/quote/zhubi?contract=binance/btc.usdt&since=2018-05-01T00&#58;00&#58;00Z&until=2018-05-01T00&#58;10&#58;00Z&size=2000
* 关于zhubi和tick，可以先查看[名词解释](/instruction/word-explanation)和[数据结构](/instruction/data-structure)两个小节。


## 获取支持的合约列表
* 获取支持的合约列表，可配置关心的日期

```
GET /historical-quote/hist-ticks?date={date}
```
* Response is a list of available contracts.

```
    [
      "bigone/1st.btc",
      "bittrex/1st.btc",
      "hitbtc/1st.btc",
      ...
    ]
```
#### Example
* [http://alihz-net-0.qbtrade.org/contracts?date=2018-01-02](http://alihz-net-0.qbtrade.org/contracts?date=2018-01-02)


## 历史Tick数据

* 获取tick历史数据，并且格式化后输出
* GET /historical-quote/hist-ticks
* 默认的输出格式为gzip文件.
* 每一行数据都是JSON对象
* `bids`/买单 按价格从高到低排序 `asks`/卖单 按价格从低到高排序 交易所不同深度不同 一般有20档
* `last` 最新成交价
* `time` isoformat的时间 (不保证是utc+8时区)
* `volume` 是上一个tick到这一个tick之间的成交量 按标的物计算 比如 eth.btc 交易对 就是成交的eth的数量
* `amount` 是tick换算成的成交额(按人民币计价)

```
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

####  Example

*  [http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okex/btc.usdt](http://alihz-net-0.qbtrade.org/hist-ticks?date=2018-01-02&contract=okex/btc.usdt)

## 历史zhubi数据

* 获取zhubi历史数据，并且格式化后输出
* 请求中params含义如下：
  * `since` zhubi的开始时间，isoformat的时间
  * `until` zhubi的结束时间，isoformat的时间
  * `contract` 交易对
  * `size` 返回的zhubi条目不超过size值
* 整个数据是一个JSON对象
* 将获取的数据parse之后是列表，列表中每个元素即为zhubi，是一个json对象：
  * `price` 成交价格
  * `amount` 是zhubi换算成的成交额(按人民币计价)
  * `bs` 'b'是买，'s'是卖
  * `contract` 合约
  * `time` isoformat的时间 (不保证是utc+8时区)
* 具体可参看[/zhubi接口说明](https://1token.trade/swagger?url=/swagger/quote.yml#/Quote/get_zhubi)


```
[
  {
    "amount": 0.0017,
    "bs": "b",
    "contract": "huobip/btc.usdt",
    "exchange_time": "2018-05-01T00:00:09.646000+00:00",
    "price": 9243.53,
    "time": "2018-05-01T00:00:09.646000+00:00"
  },
  {
    "amount": 0.03835414570315204,
    "bs": "b",
    "contract": "huobip/btc.usdt",
    "exchange_time": "2018-05-01T00:00:08.575000+00:00",
    "price": 9243.52,
    "time": "2018-05-01T00:00:08.575000+00:00"
  },
  {
    "amount": 0.063,
    "bs": "b",
    "contract": "huobip/btc.usdt",
    "exchange_time": "2018-05-01T00:00:05.825000+00:00",
    "price": 9243.58,
    "time": "2018-05-01T00:00:05.825000+00:00"
  }
]
```

#### Example

* <https://1token.trade/api/v1/quote/zhubi?contract=binance/btc.usdt&since=2018-05-01T00&#58;00&#58;00Z&until=2018-05-01T00&#58;10&#58;00Z&size=2000>

## 限制

* `alihz-net-0`限制为1秒1个请求，超过这个数量会返回`429`

## 已知存在的问题

### Tick数据文件格式问题

* json数据可能某些行是不完整的json格式, 请丢弃那些错误行

### 数据有一些已知的缺失，我们有计划逐步恢复缺失的部分数据

* 2017-12-17至2018-01-19期间每小时最后可能会缺失部分数据
* 2018-01-22币安有维护，当日部分时间段无数据
* 2018-02-08至2018-02-10期间币安有长时间维护，这段时间无币安数据
