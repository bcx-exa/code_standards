# Backend API - Code Standards

Folder Structure

|      Folder      |                  Description                  |
| :--------------: | :-------------------------------------------: |
|      /docs       |            Documentation goes here            |
|       /env       |          Enviroment config goes here          |
|      /cicd       | All files used in CI/CD pipeline goes in here |
|       /src       |                 backend code                  |
| /src/controllers |                 API Endpoints                 |
|   /src/models    |                Database Models                |
|    /src/types    |              non database types               |
|  /src/services   |                   App Logic                   |
|      /tests      |                App Unit Tests                 |

<!-- - env.local - where you store keys
- env.dev.local
- env.uat.local
- env.prod.local
- env
- env.dev
- env.uat
- env.prod -->

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
  BadRequest
  NotFound
  InternalServerError
  Success
  DbConnectionError
  Unauthorized
  ```

- Code should be well documented
  The code should be properly commented for understanding easily. Comments regarding the statements increase the understandability of the code.

- Prefixing your comments with FIXME or TODO helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented.

- Use // FIXME: to annotate problems.
- Use // TODO: to annotate solutions to problems.

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
