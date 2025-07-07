# Gold-auto-signal-
import requests
import time

# إعدادات Alpha Vantage
symbol = "XAUUSD"
interval = "5min"
apikey = "demo"  # مفتاح عام مجاني - استخدم مفتاحك لاحقًا للحصول على بيانات محدثة
url = f"https://www.alphavantage.co/query?function=TIME_SERIES_INTRADAY&symbol={symbol}&interval={interval}&apikey={apikey}"

# إعدادات Telegram
token = "8077653985:AAGhWIjLPaSf3Y3J6gNJw-hIQOW4gKPvvIM"
chat_id = "@ARYtrade_bot"

def get_signal():
    r = requests.get(url)
    data = r.json()
    candles = list(data["Time Series (5min)"].items())
    if len(candles) < 3:
        return None

    last_close = float(candles[0][1]["4. close"])
    prev_close = float(candles[1][1]["4. close"])

    if last_close > prev_close:
        return "📈 BUY Signal on GOLD (XAUUSD)"
    elif last_close < prev_close:
        return "📉 SELL Signal on GOLD (XAUUSD)"
    else:
        return "⚠️ No clear signal"

def send_telegram(msg):
    url = f"https://api.telegram.org/bot{token}/sendMessage"
    payload = {"chat_id": chat_id, "text": msg}
    requests.post(url, data=payload)

if __name__ == "__main__":
    signal = get_signal()
    if signal:
        send_telegram(signal)
