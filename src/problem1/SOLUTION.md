Provide your CLI command here:

```bash
cat transaction-log.txt | grep 'TSLA' | grep '"side": "sell"' | awk -F'"' '{print $4;}' | while read order_id; do curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET "https://example.com/api/$order_id" >> output.txt; done
```

# For your convenience run the following command:
```bash
sh script.sh
```

# Step by step debug
## Step 1
```bash
cat transaction-log.txt | grep 'TSLA'
```
```log
{"order_id": "12346", "symbol": "TSLA", "quantity": 50, "price": 890.15, "side": "sell", "timestamp": "2025-02-18T09:16:10Z"}
{"order_id": "12362", "symbol": "TSLA", "quantity": 60, "price": 885.10, "side": "sell", "timestamp": "2025-02-18T09:28:50Z"}
```

## Step 2
```bash
cat transaction-log.txt | grep 'TSLA' | grep '"side": "sell"'
```
```log
{"order_id": "12346", "symbol": "TSLA", "quantity": 50, "price": 890.15, "side": "sell", "timestamp": "2025-02-18T09:16:10Z"}
{"order_id": "12362", "symbol": "TSLA", "quantity": 60, "price": 885.10, "side": "sell", "timestamp": "2025-02-18T09:28:50Z"}
```

## Step 3
```bash
cat transaction-log.txt | grep 'TSLA' | grep '"side": "sell"' | awk -F'"' '{print $4;}'
```
```log
12346
12362
```

## Step 4
```bash
cat transaction-log.txt | grep 'TSLA' | grep '"side": "sell"' | awk -F'"' '{print $4;}' | while read order_id; do echo $order_id; done
```
```log
12346
12362
```

## Step 5
```bash
cat transaction-log.txt | grep 'TSLA' | grep '"side": "sell"' | awk -F'"' '{print $4;}' | while read order_id; do curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET "https://example.com/api/$order_id" >> output.txt; done
```
