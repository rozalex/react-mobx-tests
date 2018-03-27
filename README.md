## React/MobX tests

*Code snippets and guidelines for testing React components with jest and enzyme*

## Jest and Enzyme 
- **Jest**: Jest is a general purpose unit testing framework which provides test runners, assertion library, spies, mocks, coverage.
- **Enzyme**: Enzyme is a JavaScript Testing utility for React that makes it easier to assert, manipulate, and traverse your React Components' output.

## Installation
```bash
npm i --save-dev jest
npm i --save-dev enzyme enzyme-adapter-react-16
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
## Enzyme Shallow vs Mount Rendering
- Use **Shallow** when rendering component without its children
- Use **Mount** when you need to render children, test lifecycle components or provide a store

```js
describe('Clock panel component tests', () => {
    it('clock panel renders', () => {
        const clockPanel = mount(<ClockPanel store={WorkingHoursStore} />);
        expect(clockPanel.find('.clk-section').length).toBe(1)
    });
});
``` 

## Event simulation and state assertion
```js
it('error state when username or password not entered', () => {
    const login = shallow(<Login />);
    login.find('button').simulate('click');
    expect(login.state().errUsername).toEqual(true);
});
```