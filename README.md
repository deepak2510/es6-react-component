# ECMAScript 6 React Card Component [![Build Status](https://travis-ci.org/aaronkaka/es6-react-component.svg?branch=master)](https://travis-ci.org/aaronkaka/es6-react-component)

![Image](https://cloud.githubusercontent.com/assets/1950683/13100443/594fa604-d4fa-11e5-8042-77dc6b77280a.jpg?raw=true "screenshot")

## Goals

Card web component written with ES6 modules, classes, and syntax using [React](http://facebook.github.io/react) to 
achieve the following goals:

- Reusable and responsively-designed; just drop into DOM node
- Self-contained; no knowledge of React required to consume the component
- No component API; event all interaction
- External styling is bundled with and scoped to the component; targets Bootstrap 3
- Cross-browser for modern browsers [no IE]; thus no style scoped attribute or Shadow DOM encapsulation

[no IE] Uses the native CustomEvent constructor that no version of IE supports (MS Edge does!). This choice was made to 
reduce complexity and encourage adherence to web standards - add the polyfill if needed.

## Toolchain

- [Node.js](http://nodejs.org) `v4+`
- [webpack](https://webpack.github.io/)
    - Bundle javascript, styles and icon
    - Babel 6 transpiles ES6 and JSX
    - ESLint

Recommendation: If you are using different node versions on your machine, use [nvm](https://github.com/creationix/nvm) 
to manage them.

## Playtime

### Can I see the project working before I change anything?

After cloning the repo:

    cd es6-react-component
    npm install
    npm run dev

Once bundling is complete, open a browser tab to **localhost:8080/demo**, and dev.card-component.js is served in memory.

### Hot Reload

Hot module replacement is activated in the webpack dev server; changes to react_components are automatically reloaded in
 the browser.

### Test

The project is wired to unit test with the Jest framework.

    npm test    

## How do I consume the npm module?

Do as described in this section from the consuming application:
     
    > npm i es6-react-component --save

### Include Script

The transpiled, minified bundle will be available as /node_modules/es6-react-component/build/dist.card-component.js.

Include it in the consuming page, then event `cardstrap` with the container type, e.g.

    { detail: '.container' }
    
To remove a card instance, event `destroyCard` with the DOM element ID, e.g.

    { detail: 'div2' }

### ...or Build in your App

Example javascript:

    var cardstrap = require('es6-react-component').default;
    cardstrap('.container');
    
Then build the required bundle with webpack (requires installation of correct dependencies, loaders, and config), 
similar to this webpack.config.js:

    module.exports = {
      entry: ['./eventing.js'],
      output: {
        path: './',
        filename: 'bundle.js'
      },
      module: {
        loaders: [
          {test: /\.css$/, loader: 'style-loader!css-loader'},
          {
            test: /\.js$/, loader: 'babel', exclude: /node_modules/,
            query: {
              cacheDirectory: true,
              presets: ['es2015', 'react']
            }
          },
          {test: /\.(woff|ttf|eot|svg)(\?[a-z0-9]+)?$/, loader: 'url?limit=100000'}
        ]
      }
    };
        
### Card Instances

After the card component is bootstrapped for a container, each instance is initialized with the `initCard` event:

      eventedElement.dispatchEvent(new CustomEvent('initCard', {
          detail: {
            eventedElem: eventedElementId,
            targetElem: "div2",                         // Required
            userId: "c685a8ed331c70a47dea8812da69c1bd", // Required
            username: "joe.schmo",                      // Required
            bio: "This is Joe's bio.",
            avatar: "images/wired.jpg"
          }
        })
      );

The following events and data are emitted by each card instance.

<table>
    <tr>
        <th>CustomEvent</th><th>detail</th>
    </tr>
    <tr>
        <td>card-bioDeleted</td><td>{ userId: [sUserId], username: [sUsername] }</td>
    </tr>
    <tr>
        <td>card-liked</td><td>{ userId: [sUserId], username: [sUsername], likes: [iLikes] }</td>
    </tr>
    <tr>
        <td>card-comment</td><td>{ userId: [sUserId], username: [sUsername], comment: [sComment] }</td>
    </tr>
</table>