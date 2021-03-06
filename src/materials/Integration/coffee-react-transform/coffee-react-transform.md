---
path: '/materials/coffee-react-transform'
type: 'GitHub'
img: './screenshot.png'
material:
  title: 'coffee-react-transform'
  url: 'https://github.com/jsdf/coffee-react-transform'
  github_url: 'https://github.com/jsdf/coffee-react-transform'
  subscribers_count: '11'
  stargazers_count: '437'
  tags: ['']
  subtitle: 'DEPRECATED – Provides React JSX support for Coffeescript'
  clone_url: 'https://github.com/jsdf/coffee-react-transform.git'
  ssh_url: 'git@github.com:jsdf/coffee-react-transform.git'
  pushed_at: '2017-08-02T05:34:34Z'
  updated_at: '2018-09-04T02:59:03Z'
  author:
    name: 'jsdf'
    avatar: 'https://avatars0.githubusercontent.com/u/1232587?v=4'
    github_url: 'https://github.com/jsdf'
  latestRelease:
    tag_name: null
    name: null
    url: null
    created_at: null
---
# Coffeescript React JSX Transformer


**[CoffeeScript 2 has JSX built in. You should use that instead](http://coffeescript.org/v2/#jsx)**


# STATUS: DEPRECATED

This tool is no longer maintained. If you need to transition your codebase from
it, a codemod is available to do so: [cjsx-codemod](https://github.com/jsdf/cjsx-codemod). You can also use [CoffeeScript 2, which has JSX built in.](http://coffeescript.org/v2/#jsx)


This project started as a way for me to explore how JSX could fit into
Coffeescript syntax, as a quickly hacked together prototype. While I never
really promoted it, it quickly took on a life of its own, and before long people
were asking for it to support all kinds of different use cases. On top of that I
had no experience writing parsers, so the result is something with 
[insurmountable limitations](https://github.com/jsdf/coffee-react/issues/32).

As I eventually stopped using Coffeescript I ended up neglecting this project,
but as people were using it I didn't want to kill it. I really should have,
however, because it meant that people were using a crappy, ill-conceived,
unmaintained tool. Now, long overdue, I'm putting it out to pasture.

Original readme follows:

Provides support for an equivalent of JSX syntax in Coffeescript (called CJSX) so you can write your React components with the full awesomeness of Coffeescript. [Try it out](https://jsdf.github.io/coffee-react-transform/).

#### Example

car-component.coffee

```coffee
Car = React.createClass
  render: ->
    <Vehicle doors={4} locked={isLocked()} data-colour='red' on>
      <Parts.FrontSeat />
      <Parts.BackSeat />
      <p className='seat'>Which seat can I take? {@props?.seat or 'none'}</p>
      {# also, this is an inline comment}
    </Vehicle>
```

transform

```bash
cjsx-transform car-component.coffee
```

output

```coffee
Car = React.createClass
  render: ->
    React.createElement(Vehicle, {'doors': (4), 'locked': (isLocked()), 'data-colour': 'red', 'on': true},
      React.createElement(Parts.FrontSeat, null),
      React.createElement(Parts.BackSeat, null),
      React.createElement('p', {'className': 'seat'}, 'Which seat can I take? ', (@props?.seat or 'none'))
    )
```

### Getting Started
`coffee-react-transform` simply handles preprocessing Coffeescript with JSX-style markup into valid Coffeescript. Instead of using it directly, you may want to make use of one of these more high-level tools:
- [coffee-react](https://github.com/jsdf/coffee-react): a drop-in replacement for the `coffee` executable, for compiling CJSX.
- [node-cjsx](https://github.com/SimonDegraeve/node-cjsx): `require` CJSX files on the server (also possible with [coffee-react/register](https://github.com/jsdf/coffee-react)).
- [coffee-reactify](https://github.com/jsdf/coffee-reactify): bundle CJSX files via [browserify](https://github.com/substack/node-browserify), see also [cjsxify](https://github.com/SimonDegraeve/cjsxify).
- [cjsx-loader](https://github.com/KyleAMathews/cjsx-loader): loader module for Webpack.
- [react-coffee-quickstart](https://github.com/SimonDegraeve/react-coffee-quickstart): equivalent to [react-quickstart](https://github.com/andreypopp/react-quickstart).
- [coffee-react-quickstart](https://github.com/KyleAMathews/coffee-react-quickstart): Quickstart for building React single page apps using Coffeescript, Gulp, Webpack, and React-Router
- [sprockets preprocessor](https://github.com/jsdf/sprockets-coffee-react): use CJSX with Rails/Sprockets
- [ruby coffee-react gem](https://github.com/jsdf/ruby-coffee-react) for general ruby integration
- [vim plugin](https://github.com/mtscout6/vim-cjsx) for syntax highlighting
- [sublime text package](https://github.com/Guidebook/sublime-cjsx) for syntax highlighting
- [mimosa plugin](https://github.com/mtscout6/mimosa-cjsx) for the mimosa build tool
- [karma preprocessor](https://github.com/mtscout6/karma-cjsx-preprocessor) for karma test runner
- [broccoli plugin](https://github.com/ghempton/broccoli-cjsx) for the broccoli build tool

### CLI

```bash
cjsx-transform [input file]
```
Outputs Coffeescript code to stdout. Redirect it to a file or straight to the Coffeescript compiler, eg.
```bash
cjsx-transform examples/car.coffee | coffee -cs > car.js
```

### API
```coffee
transform = require 'coffee-react-transform'

transformed = transform('...some CJSX code...')
```

### Installation
From [npm](https://www.npmjs.org/):
```bash
npm install -g coffee-react-transform
```

#### Version compatibility
- 4.x - React >=0.14.x
- 3.x - React >=0.13.x <=0.14.x
- 2.1.x - React 0.12.1
- 2.x - React 0.12
- 1.x - React 0.11.2
- 0.x - React 0.11 and below

#### Spread attributes
JSX/CJSX 'spread attributes' allow merging in an object of props when creating an element, eg:
```coffee
extraProps = color: 'red', speed: 'fast'
<div color='blue' {...extraProps} />
```
which is transformed to:
```coffee
extraProps = color: 'red', speed: 'fast'
React.createElement('div', Object.assign({'color': 'blue'},  extraProps)
```

If you use this syntax in your code, be sure to include a shim for `Object.assign` for browsers/environments which don't yet support it. [object.assign](https://www.npmjs.org/package/object.assign), [core-js](https://github.com/zloirock/core-js) and 
[es6-shim](https://github.com/es-shims/es6-shim) are some possible choices.

#### UMD bundle for the browser
If you want to use coffee-react-transform in the browser or under ExecJS or some other environment that doesn't support CommonJS modules, you can use this build provided by [BrowserifyCDN](wzrd.in), which will work as an AMD module or just a plain old script tag:

[http://wzrd.in/standalone/coffee-react-transform](http://wzrd.in/standalone/coffee-react-transform)

```html
<script src='http://wzrd.in/standalone/coffee-react-transform'></script>
<script>
  coffeeReactTransform('-> <a />');
  // returns '-> React.createElement('a', null)'
</script>
```


### Tests

`npm test` or `cake test` or `cake watch:test`

### Changelog

See [CHANGELOG.md](CHANGELOG.md).
