# react-desc

Add a schema to your React components based on React PropTypes

## Installation

```bash
npm install react-desc
```

## Usage

### Adding documentation

```javascript
// Anchor.js

import React from 'react';
import ReactPropTypes from 'prop-types';
import { describe, PropTypes } from 'react-desc';

const Anchor = (props) => {
  const { path, ...rest } = props;
  return (
    <a href={path} {...rest}>{props.children}</a>
  );
};

export const AnchorWithSchema = describe(Anchor)
  .availableAt([
    {
      badge: 'https://codesandbox.io/static/img/play-codesandbox.svg',
      url: 'https://codesandbox.io/s/github/grommet/grommet-site?initialpath=anchor&amp;module=%2Fscreens%2FAnchor.js',
    },
  ])
  .description('A text link');

AnchorWithSchema.propTypes = {
  path: PropTypes.string.isRequired.description('React-router path to navigate to when clicked').isMain(true).version('1.0.0'),
  href: PropTypes.string.description('link location').deprecated('use path instead'),
  id: ReactPropTypes.string, // this will be ignored for documentation purposes
  title: PropTypes.custom(() => {}).description('title used for accessibility').format('XXX-XX'),
  target: PropTypes.string.description('target link location').defaultValue('_blank'),
};

export default Anchor;
```

### Accessing documentation

* JSON output

  ```javascript
    import { AnchorWithSchema } from './Anchor';

    const documentation = AnchorWithSchema.toJSON();
  ```

  Expected output:

  ```json
    {
        "name": "Anchor",
        "description": "A text link",
        "properties": [
          {
            "description": "React-router path to navigate to when clicked",
            "name": "path",
            "required": true,
            "format": "string",
            "isMain": true,
            "version": "1.0.0"
          },
          {
            "description": "link location.",
            "name": "href",
            "deprecated": "use path instead",
            "format": "string"
          },
          {
            "description": "title used for accessibility.",
            "name": "title",
            "format": "XXX-XX"
          },
          {
            "description": "target link location.",
            "name": "target",
            "defaultValue": "_blank",
            "format": "string"
          }
        ]
      }
  ```

* Markdown output

  ```javascript
    import Anchor from './Anchor';

    const documentation = Anchor.toMarkdown();
  ```

  Expected output:

  ```markdown
    ## Anchor Component
    A text link

    ### Properties

    | Property | Description | Format | Default Value | Required | Details |
    | ---- | ---- | ---- | ---- | ---- | ---- |
    | **path** | React-router path to navigate to when clicked | string |  | Yes |  |
    | **~~href~~** | link location. | string |  | No | **Deprecated**: use path instead |
    | **title** | title used for accessibility. | XXX-XX |  | No |  |
    | **target** | target link location. | string | _blank | No |  |
  ```

* Typescript output

  Format entry will be a valid typescript definition.

  ```javascript
    import { AnchorWithSchema } from './Anchor';

    const documentation = AnchorWithSchema.toTypescript();
  ```

  Expected output:

  ```json
    {
        "name": "Anchor",
        "description": "A text link",
        "properties": [
          {
            "description": "React-router path to navigate to when clicked",
            "name": "path",
            "required": true,
            "format": "string"
          },
          {
            "description": "link location.",
            "name": "href",
            "deprecated": "use path instead",
            "format": "string"
          },
          {
            "description": "title used for accessibility.",
            "name": "title",
            "format": "any"
          },
          {
            "description": "target link location.",
            "name": "target",
            "defaultValue": "_blank",
            "format": "string"
          }
        ]
      }
  ```

## API

* `describe(component)`

  Creates a proxy to the actual react component with support for the following functions:

    * **availableAt([{ badge: string, url: string }])**: function that receives an object or an array of objects that will render where the component is available.
    * **description(value)**: function that receives a string with the component description.
    * **deprecated(value)**: function that receives a string with the deprecation message.
    * **toJSON()**: function that returns the component schema as a JSON object.
    * **toMarkdown()**: function that returns the component schema as a Markdown string.
    * **usage(value)**: function that receives a string with the component usage example.

* `PropTypes`

  Proxy around the React propTypes, all properties are supported. See all options [here](https://facebook.github.io/react/docs/typechecking-with-proptypes.html).
  This proxy supports the following functions:

    * **defaultValue(value)**: function that receives a value that represents the default prop.
    * **description(value)**: function that receives a string with the PropType description.
    * **deprecated(value)**: function that receives a string with the deprecation message.
    * **format(value)**: function that receives a string with the PropTypex format.
    * **isMain(bool)**: function that receives a bool with the PropTypex isMain.
    * **version(string)**: function that receives a string with the PropTypex version.
