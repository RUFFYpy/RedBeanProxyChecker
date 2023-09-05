# Red Bean Proxy Checker (BbProxCheck)
## A Simple Multithreaded Proxy Checker

This Python script checks the validity of a list of proxy servers using multithreading. It utilizes the `requests` library to make HTTP requests through each proxy server and checks if the request to "http://ipinfo.io/json" is successful (status code 200). If it's successful, the proxy server is considered valid, and its details are printed.

## Code Explanation

```python
import threading
import queue
import requests

# Create a queue to hold proxy server addresses
q = queue.Queue()

# List to store valid proxy servers
valid_proxies = []

# Read proxy server addresses from a file and add them to the queue
with open("checkvalid_proxies.txt", "r") as f:
    proxies = f.read().split("\n")
    for p in proxies:
        q.put(p)

# Function to check the validity of proxy servers
def check_proxies():
    global q
    while not q.empty():
        proxy = q.get()
        try:
            # Make an HTTP request using the proxy server
            res = requests.get("http://ipinfo.io/json", 
                               proxies={"http": proxy,
                                        "https": proxy})
        except:
            # Continue to the next proxy if an exception occurs
            continue
        if res.status_code == 200:
            # If the request is successful, print the valid proxy
            print(proxy)

# Create 10 threads to concurrently check proxy servers
for _ in range(10):
    threading.Thread(target=check_proxies).start()
```
## How it works
1. The script reads a list of proxy server addresses from a file called "checkvalid_proxies.txt" and adds them to a queue for processing.

2. The check_proxies function is defined to check the validity of proxy servers. It continuously dequeues proxy addresses from the queue and attempts to make an HTTP request to "http://ipinfo.io/json" using each proxy.

3. If the request is successful (status code 200), it means the proxy server is valid, and its address is printed.

4. The script creates 10 threads to run the check_proxies function concurrently, allowing for faster validation of proxy servers.

   This script can be useful for checking the health and validity of a large number of proxy servers in parallel.

   ### Stay Frosty!
