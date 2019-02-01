# React Style Guide

## Basic Rules

 - Only include one component per file
 - If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass`

      ```jsx
      // bad
      const Listing = React.createClass({
        // ...
        render() {
          return <div>{this.state.hello}</div>;
        }
      });

      // good
      class Listing extends React.Component {
        // ...
        render() {
          return <div>{this.state.hello}</div>;
        }
      }
      ```

      And if you don't have state or refs, prefer normal functions (not arrow functions) over classes. [`refer this`](https://stackoverflow.com/questions/37288950/why-does-the-airbnb-style-guide-say-that-relying-on-function-name-inference-is-d).

      ```jsx
      // bad
      class Listing extends React.Component {
        render() {
          return <div>{this.props.hello}</div>;
        }
      }

      // bad (relying on function name inference is discouraged)
      const Listing = ({ hello }) => (
        <div>{hello}</div>
      );

      // good
      function Listing({ hello }) {
        return <div>{hello}</div>;
      }
      ```

## Naming

- **Extensions**: Use `.js` extension for React components.
- **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.js`.
- **Reference Naming**: Use PascalCase for React components and camelCase for their instances.

  ```jsx
  // bad
  import reservationCard from './ReservationCard';

  // good
  import ReservationCard from './ReservationCard';

  // bad
  const ReservationItem = <ReservationCard />;

  // good
  const reservationItem = <ReservationCard />;
  ```

- **Component Naming**: Use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:

  ```jsx
  // bad
  import Footer from './Footer/Footer';

  // bad
  import Footer from './Footer/index';

  // good
  import Footer from './Footer';
  ```
- **Higher-order Component Naming**: Use a composite of the higher-order component's name and the passed-in component's name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

> Why? A component's `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

  ```jsx
  // bad
  export default function withFoo(WrappedComponent) {
    return function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }
  }

  // good
  export default function withFoo(WrappedComponent) {
    function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }

    const wrappedComponentName = WrappedComponent.displayName
      || WrappedComponent.name
      || 'Component';

    WithFoo.displayName = `withFoo(${wrappedComponentName})`;
    return WithFoo;
  }
  ```

- **Props Naming**: Avoid using DOM component prop names for different purposes.

> Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

  ```jsx
  // bad
  <MyComponent style="fancy" />

  // good
  <MyComponent variant="fancy" />
  ```

## Alignment

- Follow these alignment styles for JSX syntax.

  ```jsx
  // bad
  <Foo superLongParam="bar"
       anotherSuperLongParam="baz" />

  // good
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  />

  // if props fit in one line then keep it on the same line
  <Foo bar="bar" />

  // children get indented normally
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  >
    <Quux />
  </Foo>
  ```

## Spacing

- Always include a single space in your self-closing tag.

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

- Do not pad JSX curly braces with spaces.

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## Props

  - Always use camelCase for prop names.

        ```jsx
        // bad
        <Foo
          UserName="hello"
          phone_number={12345678}
        />

        // good
        <Foo
          userName="hello"
          phoneNumber={12345678}
        />
        ```

  - Omit the value of the prop when it is explicitly `true`

        ```jsx
        // bad
        <Foo
          hidden={true}
        />

        // good
        <Foo
          hidden
        />
        ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`.

        ```jsx
        // bad
        <img src="hello.jpg" />

        // good
        <img src="hello.jpg" alt="Me waving hello" />

        // good
        <img src="hello.jpg" alt="" />

        // good
        <img src="hello.jpg" role="presentation" />
        ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props.
      > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

        ```jsx
        // bad
        <img src="hello.jpg" alt="Picture of me waving hello" />

        // good
        <img src="hello.jpg" alt="Me waving hello" />
        ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions).

        ```jsx
        // bad - not an ARIA role
        <div role="datepicker" />

        // bad - abstract ARIA role
        <div role="range" />

        // good
        <div role="button" />
        ```

  - Avoid using an array index as `key` prop, prefer a unique ID. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

      ```jsx
      // bad
      {todos.map((todo, index) =>
        <Todo
          {...todo}
          key={index}
        />
      )}

      // good
      {todos.map(todo => (
        <Todo
          {...todo}
          key={todo.id}
        />
      ))}
      ```

  - Always define explicit defaultProps for all non-required props.

      > Why? propTypes are a form of documentation, and providing defaultProps means the reader of your code doesn’t have to assume as much. In addition, it can mean that your code can omit certain type checks.

      ```jsx
      // bad
      function SFC({ foo, bar, children }) {
        return <div>{foo}{bar}{children}</div>;
      }
      SFC.propTypes = {
        foo: PropTypes.number.isRequired,
        bar: PropTypes.string,
        children: PropTypes.node,
      };

      // good
      function SFC({ foo, bar }) {
        return <div>{foo}{bar}</div>;
      }
      SFC.propTypes = {
        foo: PropTypes.number.isRequired,
        bar: PropTypes.string,
      };
      SFC.defaultProps = {
        bar: '',
        children: null,
      };
      ```
## Refs

  - Always use ref callbacks.

    ```jsx
    // bad
    <Foo
      ref="myRef"
    />

    // good
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```
## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line.

    ```jsx
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

  - Always self-close tags that have no children.

    ```jsx
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - If your component has multi-line properties, close its tag on a new line.

    ```jsx
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```
