## React/MobX tests

*Code snippets and guidelines for testing React components with jest and enzyme*

## Jest and Enzyme 
- **Jest**: Jest is a general purpose unit testing framework which provides test runners, assertion library, spies, mocks, coverage.
- **Enzyme**: Enzyme is a JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output.

## Enzyme Shallow vs Mount Rendering
- Use **Shallow** when rendering component without its children
- Use **Mount** when you need to render children, test lifecycle components or provide a store

## Installation
```bash
npm i --save-dev jest
npm i --save-dev enzyme enzyme-adapter-react-16
```

## Adapter Setup
```js
import Enzyme from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

Enzyme.configure({ adapter: new Adapter() })
```

## Mounting
```js
import {shallow, mount} from 'enzyme'
wrap = shallow(<MyComponent />)
wrap = mount(<MyComponent />)
```

## Basic test
```js
import { shallow } from 'enzyme'
import MyComponent from '../MyComponent'
it('it works!', () => {
  const wrap = shallow(
    <MyComponent name='my-component' />
  )

  expect(wrap.text()).toEqual('my-component')
})
```

## Props and state
```js
import {shallow, mount} from 'enzyme'
wrap = shallow(<MyComponent />)
wrap = mount(<MyComponent />)

expect(wrap.props('name')).toEqual('Moe')
expect(wrap.state('show')).toEqual(true)
```

## Simulating events
```js
wrap.find('input').simulate('click')

wrap.find('input').simulate('change', {
  target: { value: 'hello' }
})
```

## Mocking session storage
```js
class Common {
    getStorageMock() {
        let store = {};
        return {            
            getItem(key: string) {
                return store[key];
            }, 
            setItem(key: string, value: any) {
                store[key] = value.toString();
            },
            clear(key: string) {
                store = {};
            },      
            removeItem(key: string) {
                delete store[key];
            }
        };
    }

    defineStorageMock() {
        Object.defineProperty(window, 'sessionStorage', { value: this.getStorageMock() });
    }
}

export default new Common();
```