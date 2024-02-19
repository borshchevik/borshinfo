# How to Deploy borshinfo

borshinfo is splitted into 3 repos:
* [https://github.com/borshchevik/borshinfo](https://github.com/borshchevik/borshinfo)
* [https://github.com/borshchevik/borshinfo-api](https://github.com/borshchevik/borshinfo-api)
* [https://github.com/borshchevik/borshinfo-ui](https://github.com/borshchevik/borshinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy borsh core
1. `git clone --recursive https://github.com/borshchevik/borsh.git --branch=borshinfo`
2. Follow the instructions of [https://github.com/borshchevik/borsh/blob/master/README.md#building-borsh-core](https://github.com/borshchevik/borsh/blob/master/README.md#building-borsh-core) to build borsh
3. Run `borshd` with `-logevents=1` enabled

## Deploy borshinfo
1. `git clone https://github.com/borshchevik/borshinfo.git`
2. `cd borshinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `borshinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `borshinfo` under a process manager (like `pm2`), to restart the process when `borshinfo` crashes.

## Deploy borshinfo-api
1. `git clone https://github.com/borshchevik/borshinfo-api.git`
2. `cd borshinfo-api && npm install`
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

## Deploy borshinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/borshchevik/borshinfo-ui.git`
2. `cd borshinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "borshINFO_API_BASE_CLIENT=/api/ borshINFO_API_BASE_SERVER=http://localhost:3001/ borshINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `borshinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
