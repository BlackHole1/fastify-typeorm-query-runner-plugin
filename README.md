# fastify-typeorm-query-runner-plugin

![CI](https://github.com/BlackHole1/fastify-typeorm-query-runner-plugin/workflows/CI/badge.svg)
[![NPM version](https://img.shields.io/npm/v/fastify-typeorm-query-runner-plugin.svg?style=flat)](https://www.npmjs.com/package/fastify-typeorm-query-runner-plugin)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://standardjs.com/)
[![NPM size](https://img.shields.io/bundlephobia/min/fastify-typeorm-query-runner-plugin)](https://www.npmjs.com/package/fastify-typeorm-query-runner-plugin)
[![Coverage Status](https://coveralls.io/repos/github/BlackHole1/fastify-typeorm-query-runner-plugin/badge.svg?branch=main)](https://coveralls.io/github/BlackHole1/fastify-typeorm-query-runner-plugin?branch=main)

A plugin for Fastify that adds support for `typrorm QueryRunner and Transaction`.

Supports Fastify versions 4.x.

> Support TypeScript

## Install

```shell
# npm
npm i fastify-typeorm-query-runner-plugin

# pnpm
pnpm add fastify-typeorm-query-runner-plugin

# yarn
yarn add fastify-typeorm-query-runner-plugin
```

## Usage

```JavaScript
const fastify = require('fastify')()

fastify.register(require('fastify-typeorm-query-runner-plugin'), {
  dataSource: dataSource,
  transaction: true,
  match: request => request.routerPath.startsWith('/v2'),
  respIsError: (respStr) => respStr === '{"status":false}'
})

fastify.get('/', async (req, reply) => {
  console.log(req.queryRunner)
  console.log(req.queryRunner.manager)

  await req.queryRunner.manager.insert();
  
  reply.send({ hello: 'world' })
})

fastify.listen(3000)
```

### Options

* `dataSource`(Required): TypeORM dataSource

* `transaction`(Optional): Whether to bind the life cycle of a thing to a request. - **default: `false`**
  - Receiving requests: opening transaction
  - Return response: close transaction
  - Requesting an error: rolling back transaction

* `match`(Optional): Only matching requests will enable the plugin. - **default: `() => true`**
  - Receiving requests: opening transaction
  - Return response: close transaction
  - Requesting an error: rolling back transaction
```javascript
match: request => {
  return request.routerPath.startsWith('/v2')
}
```

* `respIsError`(Optional): When the response matches the condition, it is considered an error - **default: `() => false`**
```javascript
respIsError: (respStr) => {
  return respStr === '{"status":false}'
}
```

## License

Licensed under [MIT](./LICENSE).
