# Burn-Payment-Encoder

> Payment-Encoder provides the encode/decode functions between a Colored Coins burn-transaction payment Object to buffer

### Installation

```sh
$ npm install cc-burn-payment-encoder
```


### Encode

Params:

`paymentObject` - A standard Colored Coins payment object with the following properties:

```js
{
  // Skip input after reading asset
  skip: "Boolean"

  // Range or fixed value output (valid only if burn is false or undefined)
  range: "Boolean"

  // percent or fixed amount
  percent: "Boolean"

  // Output to send asset to - max value is 30 if range is false and 8191 if true (valid only if burn is false or undefined)
  output: "Number"

  // Total amount of units to send
  amount: "Number"

  // Should this payment be interpreted as an execution of "burn" (valid only if output value and range are undefined)
  burn: "Boolean"
}

```

Returns a new Buffer holding the encoded payment.

##### Example:

```js
var paymentEncode = require('cc-burn-payment-encoder')
var paymentObject = {
    skip: false,
    range: false,
    percent: true,
    output: 1,
    amount: 321321321
}

var code = paymentEncode.encode(paymentObject)

console.log(code) // Will print: <Buffer 21 80 99 37 cb 48>
```
Alternatively, you can encode a "burn" payment:
```js
var paymentObject = {
    skip: false,
    percent: false,
    amount: 13,
    burn: true
}

var code = paymentEncode.encode(paymentObject)

console.log(code) // Will print: <Buffer 1f 0d>
```

### Decode

Params:

- consume - takes a consumable buffer (You can use [buffer-consumer] like in the example to create one)

Returns a Colored Coins payment Object

##### Example:

```js
var paymentEncode = require('cc-burn-payment-encoder')
var consumer = require('buffer-consumer')

var decode = paymentEncode.decode(consumer(code))
var codeBuffer = new Buffer([0x82,0x76,0x0e,0x1b,0x48])

console.log(paymentEncode.decode(consumer(codeBuffer)))
// Will print:
// {
//  skip: false,
//  range: false,
//  percent: true,
//  output: 1,
//  amount: 321321321
//  }
```

### Testing

In order to test you need to install [mocha] globaly on your machine

```sh
$ cd /"module-path"/cc-burn-payment-encoder
$ mocha
```


## License

This software is licensed under the Apache License, Version 2.0.
See [LICENSE](LICENSE) for the full license text.


[mocha]:https://www.npmjs.com/package/mocha
[buffer-consumer]:https://www.npmjs.com/package/buffer-consumer
