# Mining-Farm-Monitor

> A monitoring app for your workers.
[DEMO](https://monitor.blockchainvest.io)

## Configuration

You'll need PHP to make this working. If you find a way to retrieve Ethpool's JSON without CORS error, let me know !  
Works only with Ethpool and Nanopool currently, but going to be improved for Ethermine.  
Edit /pool.php and replace with your wallet address (without 0x).

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080 (doesn't work because of php socket, use ethpool.json for tests)
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
