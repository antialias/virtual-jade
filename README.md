
# virtual-jade

[![NPM version][npm-image]][npm-url]
[![Build status][travis-image]][travis-url]
[![Test coverage][coveralls-image]][coveralls-url]
[![Dependency Status][david-image]][david-url]
[![License][license-image]][license-url]
[![Downloads][downloads-image]][downloads-url]
[![Gittip][gittip-image]][gittip-url]

Server-side function to compile your [jade](https://github.com/jadejs/jade) templates into [virtual-dom](https://github.com/Matt-Esch/virtual-dom) functions.

Create a template:

```jade
.Item(class=item.active ? 'active' : '')
  .Item-title= item.title
  .Item-description= item.description
```

`require()` your template as a function:

```js
const createElement = require('virtual-dom/create-element')
const h = require('virtual-dom/h')

const renderItem = require('./item.jade')

// create a render function using the templates
function render(locals) {
  // note: this could be included in the template,
  // which would remove the requirement of creating this function
  return h('.items', locals.items.map(renderItem))
}

let rootTree = render(locals)
const rootNode = createElement(rootTree)
document.body.appendChild(rootNode)
```

Later, you can update the nodes:

```js
const patch = require('virtual-dom/patch')
const diff = require('virtual-dom/diff')

locals.items.push({
  title: 'new item',
  description: 'new description'
})

requestAnimationFrame(function () {
  let newTree = render(locals)
  let patches = diff(rootTree, newTree)
  rootNode = patch(rootNode, patches)
  rootTree = newTree
})
```

## Notes

- Requires a CommonJS environment for client-side `require()`s.
- Requires you to install [virtual-dom](https://github.com/Matt-Esch/virtual-dom) on the client via `npm`.

## API

### str = render(src, options)

`src` is the jade source.

Options are:

- `.pretty=false` - whether to beautify the resulting JS.
  Requires you to install `js-beautify` yourself.
- `.runtime=true` - whether to include the runtime,
  which is just a bunch of `require()`s to `virtual-dom` files.

[npm-image]: https://img.shields.io/npm/v/virtual-jade.svg?style=flat-square
[npm-url]: https://npmjs.org/package/virtual-jade
[github-tag]: http://img.shields.io/github/tag/jonathanong/virtual-jade.svg?style=flat-square
[github-url]: https://github.com/jonathanong/virtual-jade/tags
[travis-image]: https://img.shields.io/travis/jonathanong/virtual-jade.svg?style=flat-square
[travis-url]: https://travis-ci.org/jonathanong/virtual-jade
[coveralls-image]: https://img.shields.io/coveralls/jonathanong/virtual-jade.svg?style=flat-square
[coveralls-url]: https://coveralls.io/r/jonathanong/virtual-jade
[david-image]: http://img.shields.io/david/jonathanong/virtual-jade.svg?style=flat-square
[david-url]: https://david-dm.org/jonathanong/virtual-jade
[license-image]: http://img.shields.io/npm/l/virtual-jade.svg?style=flat-square
[license-url]: LICENSE
[downloads-image]: http://img.shields.io/npm/dm/virtual-jade.svg?style=flat-square
[downloads-url]: https://npmjs.org/package/virtual-jade
[gittip-image]: https://img.shields.io/gratipay/jonathanong.svg?style=flat-square
[gittip-url]: https://gratipay.com/jonathanong/
