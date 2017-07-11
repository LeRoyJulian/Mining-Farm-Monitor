# Mining-Farm-Monitor

> A monitoring app for your workers.
[DEMO](https://monitor.blockchainvest.io)

## Configuration

Works only with Ethpool.org currently, but going to be improved for Ethermine and Nanopool.
Edit /ethpool.php and replace with your wallet address (without 0x).

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
