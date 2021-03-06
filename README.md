# Registers.js

[![Build Status](https://travis-ci.org/nickcolley/registers.js.svg?branch=master)](https://travis-ci.org/nickcolley/registers.js)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

JavaScript client for the [GOV.UK Registers](https://www.registers.service.gov.uk/) project

Work in progress at the moment, see [todo](#todo) for more.

## Install

```js
npm install --save nickcolley/registers.js
```

### Glitch

You can try this library out on Glitch with the [registers.js boilerplate project](https://glitch.com/~registers-js-boilerplate)

## Node

> This project uses async/await in the examples which requires Node v8.x+.
You can also use regular promises which requires Node 4.x+ by using `new Registers('...').method().then(...)`

First pick the register from the [Government registers collection](https://www.registers.service.gov.uk/registers).

The examples below uses the ['country' register](https://www.registers.service.gov.uk/registers/country), which is a register for "British English names of all countries currently recognised by the UK government".

```js
const Registers = require('registers')
const country = new Registers('country')

async function App () {
  const info = await country.register()
  console.log('GET /register', info)
}

App()
```

### [GET /register](https://docs.registers.service.gov.uk/api_reference/get_register/#get-register)
```js
const info = await country.register()
console.log('GET /register', info)
```

### [GET /records](https://docs.registers.service.gov.uk/api_reference/get_records/#get-records)
```js
const records = await country.records()
console.log('GET /records', records)
```

#### Parameters
- pageIndex (Optional): Collection page number. Defaults to 1.
- pageSize (Optional): Collection page size. Defaults to 100. Maximum is 5000.

```js
const records = await country.records({
  pageIndex: 1,
  pageSize: 5000
})
console.log('GET /records?page-index=1&page-size=5000', records)
```

### [GET /records/{key}](https://docs.registers.service.gov.uk/api_reference/get_records_key/#get-records-key)
```js
const countryRecord = await country.records('GB')
console.log('GET /records/GB', countryRecord)
```

### [GET /records/{key}/entries](https://docs.registers.service.gov.uk/api_reference/get_records_key_entries/#get-records-key-entries)
```js
const countryRecordEntries = await country.records('GB', { entries: true })
console.log('GET /records/GB/entries', countryRecordEntries)
```

### [GET /records/{field-name}/{field-value}](https://docs.registers.service.gov.uk/api_reference/get_records_field_name_field_value/#get-records-field-name-field-value)
```js
const filteredRecords = await country.records({ fieldName: 'name', fieldValue: 'United Kingdom' })
console.log('GET /records/name/United Kingdom', filteredRecords)
```

### [GET /entries](https://docs.registers.service.gov.uk/api_reference/get_entries/#get-entries)
```js
const entries = await country.entries()
console.log('GET /entries', entries)
```

### [GET /entries/{entry-number}](https://docs.registers.service.gov.uk/api_reference/get_entries_entry_number/#get-entries-entry-number)
```js
const secondEntry = await country.entries(6)
console.log('GET /entries/2', secondEntry)
```

### [GET /items/{item-hash}](https://docs.registers.service.gov.uk/api_reference/get_items_item_hash/#get-items-item-hash)
```js
const item = await country.items('sha-256:6b18693874513ba13da54d61aafa7cad0c8f5573f3431d6f1c04b07ddb27d6bb')
console.log('GET /items/sha-256:6b18693874513ba13da54d61aafa7cad0c8f5573f3431d6f1c04b07ddb27d6bb', item)
```

### [GET /download-register](https://docs.registers.service.gov.uk/api_reference/get_download_register/#get-download-register)
```js
const downloadedRegisterAsBuffer = await country.downloadRegister()
console.log('GET /download-register', downloadedRegisterAsBuffer)
```

## Full examples

### Log all countries names
See [examples/countries.js](./examples/countries.js)

### All endpoints
See [examples/all-endpoints.js](./examples/all-endpoints.js)

### Download and save register to disk
See [examples/download.js](./examples/downloadRegister/node.js)

## Browser

Working but not published yet, clone this repository and run `npm run build:browser:production`.

I have put together a really inaccessible example (do not use), but hope to publish some production ready examples when/if this is published to npm:

[JSBin Browser Example](https://output.jsbin.com/bucijav) ([Source](https://jsbin.com/bucijav/edit?js))

## Development

### Todo

- Test browser code
- Publish to npm, going to ask the Registers team for their advice here.
- Pagination
- Error handling o-o
- Validation
- Caching: Since Registers have a timestamp for when they were last updated, could we have a caching mode? This could also include adapters to allow for writing to memory, and also something more persistent
- Performance? Streaming?

### Testing

Run the tests with:

```bash
npm test
```

Under the hood this is running `jest`, so you can [pass any options it uses:](https://jestjs.io/docs/en/cli.html)

```bash
npm test -- --watch
```

### Thanks
- This project was mainly made to learn but is in places inspired by [Octokit/rest.js](https://github.com/octokit/rest.js)
- GOV.UK Registers team for creating a well documented API
