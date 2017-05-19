# chromy

chromy is a library for operating chrome. 

## Installation

```bash
npm i chromy
```

## Usage

```js
const Chromy = require('chromy')

// not headless
// let chromy = new Chromy({visible:true})
let chromy = new Chromy()
chromy.chain()
      .goto('http://example.com/')
      .evaluate(() => {
        return document.querySelectorAll('*').length
      })
      .result((r) => console.log(r))
      .end()
      .then(() => chromy.close())
```

You can also use async/await interfaces like this:

```js
// You dont't need to call such methods: chain(), end()
let chromy = new Chromy()
await chromy.goto('http://example.com/')
await chromy.evaluate(() => {
        return document.querySelectorAll('*').length
      })
await chromy.result((r) => console.log(r))
await chromy.close()
```

## API

##### Chromy(options)

options:  
visible(default: false): If set to true, chrome is launched in visible mode.  
port(default: 9222): --remote-debugging-port  
waitTimeout(default: 30000): If wait() does not be finished in a specified time WaitTimeoutError will be throwed.  
gotoTimeout(default: 30000): If goto() does not be finished in a specified time GotoTimeoutError will be throwed.  
evaluateTimeout(default: 30000): If evaluate() does not be finished in a specified time EvaluateTimeError will be throwed.

##### .goto(url)

##### .evaluate(func)

##### .result(func)

result() receive a result of previous directive.

```js
chromy.chain()
      .goto('http://example.com')
      .evaluate(() => {
        return document.querySelectorAll('*').length
      })
      .result((length) => {
        // length is a result of evaluate() directive.
        console.log(length)
      }
      .end()
```

##### .end()

##### .wait(msec)

alias for .sleep(msec)

##### .wait(selector)

wait until selector you specified appear in a DOM tree.

##### .wait(func)

wait until function you supplied is evaluated as true.

##### .sleep(msec)

wait for milli seconds you specified.

##### .type(selector, text)

##### .click(selector)

##### .defineFunction(func)

```js
function outerFunc () {
  return 'VALUE'
}
chromy.chain()
      .goto('http://example.com')
      .defineFunction(outerFunc)
      .evaluate(() => {
        outerFunc()
      })
      .end()
```

##### .screenshot(format = 'png', quality = 100, fromSurface = true)

It export a current screen as an image data. 
`format` must be eather 'png' or 'jpeg'.

See examples: [examples/screenshot.js]

##### .pdf()

It export a current screen as a PDF data. 

See examples: [examples/screenshot.js]

##### .console(func)

```js
chromy.chain()
      .goto('http://example.com')
      .console((text) => {
        console.log(text)
      })
      .evaluate(() => {
        console.log('HEY')
      })
      .end()
```

##### .receiveMessage(func)

receive a message from browser.

```js
chromy.chain()
      .goto('http://example.com')
      .receiveMessage((msg) => {
        console.log(msg[0].value)
      })
      .evaluate(() => {
        sendToChromy({value: 'foo'})
      })
```

##### static cleanup()

close all browsers.

```js
process.on('SIGINT', async () => {
  await Chromy.cleanup()
  process.exit(1)
})
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/OnetapInc/chromy

