# Mining-Farm-Monitor

> A monitoring app for your workers.
[DEMO](https://monitor.blockchainvest.io)


## Configuration

You'll need PHP to make this working. If you find a way to retrieve Ethpool's JSON without CORS error, let me know !  
Works only with Ethpool and Nanopool currently, but going to be improved for Ethermine.  
Ethpool : Edit /pool.php and replace with your wallet address (without 0x).  
Nanopool : Edit /pool.php and replace with your JSON's URL.


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080 (doesn't work because of needed PHP socket, use pool.json for tests)
npm run dev

# build for production with minification
npm run build
```

ETH donation : 0x794328e169231b39f36bb60e15b575bd6a38833b
