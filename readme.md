# Backend API - Code Standards

Folder Structure

|      Folder/File      |                                                              Description                                                               |
| :-------------------: | :------------------------------------------------------------------------------------------------------------------------------------: |
|         /docs         |                                                        Documentation goes here                                                         |
|         /env          |                                                      Enviroment config goes here                                                       |
|       /env/.env       |                                      varibles that are shared across all enviroments. ie app name                                      |
|    /env/.env.{env}    |                          varibles that are not a secret. ie location of s3 bucket, url to other mircoservice                           |
| /env/.env.{env}.local | [***Don't Checked In***:no_entry_sign:] Where you store local secret keys. ie Database Password, AWS Credentials for local development |
|         /iac          |                                  All files used in Infrasturce as code and CICD pipeline goes in here                                  |
|         /src          |                                                              backend code                                                              |
|   /src/controllers    |                                                           Rest API Endpoints                                                           |
|     /src/helpers      |                                                            Helper functions                                                            |
|      /src/models      |                                                            Database Models                                                             |
|      /src/types       |                                                           non database types                                                           |
|     /src/services     |                                                               App Logic                                                                |
|        /tests         |                                                             App Unit Tests                                                             |

- Wrap common utilities as npm packages

- Use naming conventions

  - lowerCamelCase when naming constants, variables, and functions
    ```
    let someVariableExample = 'value';
    function doSomething() {}
    ```
  - UpperCamelCase (capital first letter as well) when naming classes.
    ```
    class SomeClassExample {}
    ```
  - Use descriptive names, but try to keep them short.

- Prefer const over let. no var

- No unused variables.

- Commas must be placed at the end of the current line.

  ```
    var obj = {
      foo: 'foo'
      ,bar: 'bar'   // ✗ avoid
    }

    var obj = {
      foo: 'foo',
      bar: 'bar'   // ✓ ok
    }

  ```

- Dot should be on the same line as property.

  ```
    console.
      log('hello')  // ✗ avoid

    console
      .log('hello') // ✓ ok
  ```

- Require or import statements at the beginning of each file, before and outside of any functions.

- Distinguish operational vs programmer errors
  Operational errors (e.g. API received an invalid input) refer to known cases where the error impact is fully understood and can be handled thoughtfully. On the other hand, programmer error (e.g. trying to read an undefined variable) refers to unknown code failures that dictate to gracefully restart the application.

- Use array spreads ... to copy arrays.

  ```
  // bad
  const len = items.length;
  const itemsCopy = [];
  let i;

  for (i = 0; i < len; i += 1) {
    itemsCopy[i] = items[i];
  }

  // good
  const itemsCopy = [...items];
  ```

- Do not include JavaScript filename extensions eslint: import/extensions

```
// bad
import foo from './foo.js';

// good
import foo from './foo';
```

- Start a Codeblock’s Curly Braces on the Same Line

  ```
  Code Example
  // Do
  function someFunction() {
    // code block
  }

  // Avoid
  function someFunction()
  {
    // code block
  }
  ```

- Use the === operator
  Prefer the strict equality operator === over the weaker abstract equality operator ==. == will compare two variables after converting them to a common type. There is no type conversion in ===, and both variables must be of the same type to be equal.

- Use Async Await, avoid callbacks

- Be stateless – Save no data locally on a specific web server (see separate bullet – ‘Be Stateless’)

- Extract secrets from enviroment local file then when deployed from the parameter store

- No console.logs

- Guard Rails

  ```
  if(!consumer) return new BadRequest('No conumser found')
  if(!consumer) return new InternalServerError('No consumer found')
  ```

  ```
  try{

  } catch(){

  }
  ```

- Return Types

  ```
  SuccessResponse = {
    statusCode: 200,
    name: 'String',
    message: 'String',
    data: {},
  }
  BadRequest = {
    statusCode: 400,
    name: 'String',
    message: 'String',
    data: {},
  }
  NotFound = {
    statusCode: 404,
    name: 'String',
    message: 'String',
    data: {},
    }
  InternalServerError = {
    statusCode: 500,
    name: 'String',
    message: 'String',
    data: {},
  }
  DbConnectionError = {
    statusCode: 500,
    name: 'String',
    message: 'String',
    data: {},
  }
  Unauthorized = {
    statusCode: 404,
    name: 'String',
    message: 'String',
    data: {},
  }
  ```

- Code should be well documented
  The code should be properly commented for understanding easily. Comments regarding the statements increase the understandability of the code.

- Prefixing your comments with FIXME or TODO helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented.

  ```
    class Calculator extends Abacus {
    constructor() {
    super();
        // TODO: total should be configurable by an options param
        // FIXME: shouldn’t use a global here
        this.total = 0;
    }
    }
  ```
