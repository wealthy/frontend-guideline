# Folder structuring Guide

We use react library for web app and react-native for cross platform mobile application. We maintain the same code structure across both the platforms to make things predictable and maintainable. This documentation is regarding the folder structure we follow or, in future plan to follow.

We follow a **modular code structure**. If a component is required only by its parent and by no other component, we nest the component inside the parent and **not** put them along with common components.

## Overview


  - components
  - screens(for mobile app)
  - pages(for web app)
  - config
  - graphql
  - constants
  - utils
  - data
  - assets
  - api

Lets go over each of them in detail in order to get a better understanding:-

### components

Components folder contains **only** the components which are to be used across the screens(in case of mobile app) or pages(in case of web app).

The components folder is sub divided into following folders:-

- baseComponents
> Contains the components which are inbuilt into the library(either react native or react), which we override to match the theme of our app.
>
> A few examples of components that could be put here are as follows:-
>
> - Button
> - Text
> - Text Input
> - Spinner
> - Slider
- commonComponents
> Contains the custom created components which are used across the entire application.
>
> A few examples of components that could be put here are as follows:-
> - Card
> - GraphCard
> - Header
> - Modals

Also, please note that we use named exports in this folder. Take following example:-

  - baseComponents
    - Button
      - Button.js
        ```javascript
          import React from "react";

          const Button = () => (
            return null;
          );

          export default Button;
        ```
      - index.js
        ```javascript
          export {default as Button} from "./Button"
        ```
    - index.js
      ```javascript
        export * from "./Button";
      ```
  - commonComponents
    - Card
      - Card.js
      ```javascript
        import React from "react";

        const Card = () => (
          return null;
        );

        export default Card;
      ```
      - index.js
      ```javascript
        export {default as Card} from "./Card"
      ```
    - index.js
    ```javascript
      export * from "./Button";
    ```
  - index.js
  ```javascript
    export * from "./baseComponents";
    export * from "./commonComponents";
  ```

> In order to import these components, we use **absolute imports**.

## screens / pages

Screens folder contain list of all the screens or pages of the pages of the application.

```
- Auth
- Payment
```

We divide the screens further into components. Let us consider following example:-


- Auth
    - components
      - Social
          - Social.js
          ```javascript
            import React from "react";

            const AuthSocial = () => (
              return null;
            );

            export default AuthSocial;
          ```
          - index.js
          ```javascript
            import AuthSocial from "./AuthSocial";

            export default AuthSocial;
          ```
      - Email
        - Email.js
        ```javascript
          import React from "react";

          const AuthEmail = () => (
            return null;
          );

          export default AuthEmail;
        ```
        - index.js
        ```javascript
          import AuthEmail from "./Email";

          export default AuthEmail;
        ```
      - index.js
      ```javascript
        import Auth from "./Auth";

        export default Auth;
      ```
      - Auth.js
      ```javascript
        import React from "react";
        import Social from "./components/Social";
        import Email from "./components/Email";

        const Email = () => (
          return (
            <div>
              <Social />
              <Email />
            </div>
          );
        );

        export default Email;
      ```

> We user default exports and relative imports in this folder.
> The nested component should only be **imported** and **used** in immediate parent.

#### Component and Folder Naming

If the components are nested, the names of the folders can get pretty big. To avoid this, name the folders according to what they are.

In the above example, we name the folder **Social** and not **AuthSocial**. However, we name the component by combining the name of all layers, which in this case is Auth + Social = AuthSocial. We do this in order to search components easily using search bar in the editor.

## config

config folder contains basic configuration of the app like colors, propTypes, configuring redux store, configuring apollo store etc.

```
- colors.js
- propTypes.js
- reduxStore.js
- apollo.js
```

# graphql

graphql folders contains queries and mutation for each screen/pages.


# constants

constants used all across the app.

# utils

helper functions used in the application

# data

Redux store

# assets

Images and assets used across entire application

# api

api folder contains endpoints variables and a layer to create endpoints dynamically.  
