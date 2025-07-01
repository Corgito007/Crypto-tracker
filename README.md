crypto-price-agent/
│
├── agent.py                 # Main script to fetch and log crypto prices
├── requirements.txt         # Python dependencies
├── Dockerfile               # Container setup
└── .nosana-ci.yaml          # Nosana pipeline config
import requests
import time

COINS = ['bitcoin', 'ethereum']
API_URL = 'https://api.coingecko.com/api/v3/simple/price'

def fetch_prices():
    response = requests.get(API_URL, params={
        'ids': ','.join(COINS),
        'vs_currencies': 'usd'
    })
    return response.json()

def main():
    while True:
        prices = fetch_prices()
        for coin in COINS:
            print(f"{coin.upper()}: ${prices[coin]['usd']}")
        time.sleep(60)  # Wait 1 min before next check

if __name__ == '__main__':
    main()
    requests
    FROM python:3.10-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "agent.py"]
version: 1.0
jobs:
  crypto-tracker:
    runs-on: gpu
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Build Docker Image
        run: docker build -t crypto-agent .

      - name: Run Agent
        run: docker run crypto-agent
