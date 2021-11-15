# How to Deploy htmlcoininfo

htmlcoininfo is splitted into 3 repos:
* [https://github.com/denuoweb/htmlcoininfo](https://github.com/denuoweb/htmlcoininfo)
* [https://github.com/denuoweb/htmlcoininfo-api](https://github.com/denuoweb/htmlcoininfo-api)
* [https://github.com/denuoweb/htmlcoininfo-ui](https://github.com/denuoweb/htmlcoininfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy htmlcoin core
1. `git clone --recursive https://github.com/htmlcoin/htmlcoin.git --branch=htmlinfo`
2. Follow the instructions of [https://github.com/htmlcoin/htmlcoin/blob/master/README.md#building-htmlcoin-core](https://github.com/htmlcoin/htmlcoin/blob/master/README.md#building-htmlcoin-core) to build htmlcoin
3. Run `htmlcoind` with `-logevents=1` enabled

## Deploy htmlcoininfo
1. `git clone https://github.com/denuoweb/htmlcoininfo.git`
2. `cd htmlcoininfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `htmlcoininfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `htmlcoininfo` under a process manager (like `pm2`), to restart the process when `htmlcoininfo` crashes.

## Deploy htmlcoininfo-api
1. `git clone https://github.com/denuoweb/htmlcoininfo-api.git`
2. `cd htmlcoininfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy htmlcoininfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/denuoweb/htmlcoininfo-ui.git`
2. `cd htmlcoininfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "HTMLCOININFO_API_BASE_CLIENT=/api/ HTMLCOININFO_API_BASE_SERVER=http://localhost:3001/ HTMLCOININFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `htmlcoininfo-ui` on port 3000
4. `npm run build`
5. `npm start`
