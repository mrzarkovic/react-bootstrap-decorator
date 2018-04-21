# react-bootstrap-decorator
React Bootstrap Context Provider using Higher Order Component as a Decorator.

The main idea is to be able to pass down gobal Bootstrap or other CSS to React Components using React Context.

First we define a Context Provider `BootstrapContext.js` that exports BootstrapProvider and BootstrapContext.
Later we make use of React Higher Order Components to make a wrapper `WithBootstrap.js` for our Components that we use as a Decorator.

BootstrapContext.js
```javascript
import React, {Component} from 'react';

const {Provider, Consumer} = React.createContext();

import bootstrap from 'bootstrap/scss/bootstrap.scss';

export class BootstrapProvider extends Component {
  render() {
    return <Provider value={bootstrap}>{this.props.children}</Provider>;
  }
}

export const BootstrapConsumer = Consumer;
```

WithBootstrap.js
```javascript
import React, {Component} from 'react';

import {BootstrapConsumer} from 'Context/BootstrapContext';

export default function withBootstrap(WrappedComponent) {
  return class extends Component {
    render() {
      return (
        <BootstrapConsumer>
          {bootstrap => (
            <WrappedComponent bootstrap={bootstrap} {...this.props} />
          )}
        </BootstrapConsumer>
      );
    }
  };
}
```

App.js
```javascript
import React, {Component} from 'react';

import HomePage from 'Pages/HomePage';

import {BootstrapProvider} from 'Context/BootstrapContext';

export default class App extends Component {
  render() {
    return (
      <BootstrapProvider>
        <HomePage />
      </BootstrapProvider>
    );
  }
}
```

HomePage.js
```javascript
import React, {Component} from 'react';

import WithBootstrap from '../util/WithBootstrap';

@WithBootstrap
export default class HomePage extends Component {
  render() {
    return (
      <div className={this.props.bootstrap.container}>
        {console.log(this.propsbootstrap)}
        Moja prva bootstrap komponenta
      </div>
    );
  }
}
```