# ๐ ๋นํธ์ฝ์ธ ์๋๋งค๋งค

## ์ฐธ๊ณ  ์๋ฃ
- ํ์ด์ฌ์ ์ด์ฉํ ๋นํธ์ฝ์ธ ์๋๋งค๋งค - ์กฐ๋ํ
<img src="./Image/5.jpg" width=500>
<br/><br/><br/>

## ๋งค๋งค ์ ๋ต (๋ณ๋์ฑ ๋ํ ์ ๋ต)
1. ๊ฐ๊ฒฉ ๋ณ๋ํญ ๊ณ์ฐ: ํฌ์ํ๋ ค๋ ๊ฐ์ํํ์ ์ ์ผ ๊ณ ๊ฐ(high)์์ ์ ์ผ ์ ๊ฐ(low)๋ฅผ ๋นผ์ ๊ฐ์ํํ์ ๊ฐ๊ฒฉ ๋ณ๋ํญ์ ๊ตฌํ๋ค.
2. ๋งค์ ๊ธฐ์ค: ๋น์ผ ์๊ฐ์์ (๋ณ๋ํญ * k) ์ด์ ์์นํ๋ฉด ํด๋น ๊ฐ๊ฒฉ์ ๋ฐ๋ก ๋งค์ํ๋ค.
3. ๋งค๋ ๊ธฐ์ค: ๋น์ผ ์ข๊ฐ์ ๋งค๋ํ๋ค.
<br/><br/><br/>

## ์ฝ๋ ๋ฆฌ๋ทฐ
1. ์ฃผ๊ธฐ์ ์ผ๋ก ํ์ฌ๊ฐ ๋ฐ๊ธฐ
```python
def get_current_price(ticker):
    """ํ์ฌ๊ฐ ์กฐํ"""
    return pyupbit.get_orderbook(ticker=ticker)["orderbook_units"][0]["ask_price"]

while True:
    current_price = get_current_price("KRW-BTC")
    time.sleep(1)
```
<br/><br/>

2. ๋ชฉํ๊ฐ ๊ณ์ฐํ๊ธฐ
```python
def get_target_price(ticker, k):
    """๋ณ๋์ฑ ๋ํ ์ ๋ต์ผ๋ก ๋งค์ ๋ชฉํ๊ฐ ์กฐํ"""
    df = pyupbit.get_ohlcv(ticker, interval="day", count=2)
    target_price = df.iloc[0]['close'] + (df.iloc[0]['high'] - df.iloc[0]['low']) * k
    return target_price

while True:
    target_price = get_target_price("KRW-BTC", 0.5)
    time.sleep(1)
```
<br/><br/>

3. ๋งค์, ๋งค๋ํ๊ธฐ
```python
 if start_time < now < end_time - datetime.timedelta(seconds=10):
            target_price = get_target_price("KRW-BTC", 0.5)
            current_price = get_current_price("KRW-BTC")
            if target_price < current_price:
                krw = get_balance("KRW")
                if krw > 5000:
                    upbit.buy_market_order("KRW-BTC", krw*0.9995)
        else:
            btc = get_balance("BTC")
            if btc > 0.00008:
                upbit.sell_market_order("KRW-BTC", btc*0.9995)
        time.sleep(1)
```
<br/><br/><br/>

## AWS ์๋ฒ์ ๊ตฌ์ถํ๊ธฐ
1. EC2 ๊ตฌ์ถ
<img src="./Image/2.png">
<br/><br/>

2. EC2 ์ธ์คํด์ค์ ์ฝ๋ ์๋ก๋
```bash
git clone https://github.com/JAEHYUN2022/bitcoinAutoTrade.git
```
<img src="./Image/3.png">
<br/><br/>

3. ๋ฐฑ๊ทธ๋ผ์ด๋ ํ๊ฒฝ์์ ์ฝ๋ ์คํ
```bash
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul/ /etc/localtime

sudo apt update

sudo apt install python3-pip

pip3 install pyupbit

nohup python3 bitcoinAutoTrade.py > output.log &
```
<img src="./Image/4.png">
