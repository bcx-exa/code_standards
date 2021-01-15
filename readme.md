# Backend API - Code Standards

## Backend Folder Structure

|      Folder/File      |                                                              Description                                                               |
| :-------------------: | :------------------------------------------------------------------------------------------------------------------------------------: |
|         /docs         |                                                        Documentation goes here                                                         |
|         /env          |                                                      Enviroment config goes here                                                       |
|       /env/.env       |                                     variables that are shared across all enviroments. ie app name                                      |
|    /env/.env.{env}    |                          variables that are not a secret. ie location of s3 bucket, url to other mircoservice                          |
| /env/.env.{env}.local | [***Don't Checked In***:no_entry_sign:] Where you store local secret keys. ie Database Password, AWS Credentials for local development |
|         /iac          |                                  All files used in Infrasturce as code and CICD pipeline goes in here                                  |
|         /src          |                                                              backend code                                                              |
|   /src/controllers    |                                                           Rest API Endpoints                                                           |
|     /src/helpers      |                                                            Helper functions                                                            |
|       /src/jobs       |                                                               Cron Jobs                                                                |
|      /src/models      |                                                            Database Models                                                             |
|      /src/types       |                                                           non database types                                                           |
|     /src/services     |                                                               App Logic                                                                |
|        /tests         |                                                             App Unit Tests                                                             |

## General

- Wrap common utilities into the bcx shared libary npm package, ie encrytion,database connections

- Code should be well documented
  The code should be properly commented for understanding easily. Comments regarding the statements increase the understandability of the code.

- No unused variables.

- Require or import statements at the beginning of each file, before and outside of any functions.

- There should types for every variable created.

  ```
  // bad
  let foo = 0;
  foo = "String" //will throw only on complie time

  // good
  let foo: number = 0;
  foo = "String" //will throw in editor
  ```

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

- Use the === operator
  Prefer the strict equality operator === over the weaker abstract equality operator ==. == will compare two variables after converting them to a common type. There is no type conversion in ===, and both variables must be of the same type to be equal.

```
4 == "4" // will return true // ✗ avoid

4 === "4" // will return false // ✓ ok

```

- Use Async Await, avoid callbacks

```
TODO: add example

```

- Be stateless – Save no data locally on a specific web server

- Extract secrets from enviroment local file then when deployed from the parameter store

- No console.logs

- Guard Rails

```

if(!consumer) return new BadRequest('No conumser found')
if(!consumer) return new InternalServerError('No consumer found')


try{

} catch(e){

}

```


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
## Style Guide

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
- Commas must be placed at the end of the current line.

  ```
    const obj = {
      foo: 'foo'
      ,bar: 'bar'   // ✗ avoid
    }

    const obj = {
      foo: 'foo',
      bar: 'bar',   // ✓ ok
    }
  ```

- Trailing commas need to be at the end of a object/array

  ```
  const godzilla = {
    teeth: 213,
    kind: 'giant monster' // <- no comma before the closing brace // ✗ avoid
  };

  const godzilla = {
    teeth: 213,
    kind: 'giant monster', // <- comma before the closing brace // ✓ ok
  };
  ```

- Dot should be on the same line as property.

  ```
  const advert = await advert_details. // ✗ avoid
      createQueryBuilder('adSubmitted'). // ✗ avoid
      select() // ✗ avoid

  const advert = await advert_details // ✓ ok
      .createQueryBuilder('adSubmitted') // ✓ ok
      .select() // ✓ ok
  ```



### Naming conventions

- Use descriptive names, but try to keep them short.
- Prefer const over let. no var

  - lowerCamelCase when naming constants, variables, and functions
    ```
    let someVariableExample = 'value';
    function doSomething() {}
    ```
  - UpperCamelCase (capital first letter as well) when naming classes.
    ```
    class SomeClassExample {}
    ```

## Database Models

- Needs to have

  - uuid
  - dateCreated
  - dateModified
  - isDeleted

  ```
  @Entity({ name: !TableName! })
  export class !TableName! {
    @PrimaryGeneratedColumn("uuid")
    uuid: string;

    @Column({ type: "timestamp", default: () => "CURRENT_TIMESTAMP" })
    dateCreated: string;

    @Column({ type: "timestamp", default: () => "CURRENT_TIMESTAMP" })
    dateModified: string;

    @Column({ type: "boolean", default: () => false })
    isDeleted: boolean;
  }
  ```

- Database record should never be deleted, only flagged as deleted (use the isDeleted Field), exceptions are allowed but needs to be discussed.
  ```
    try {
      const connect = await auroraConnectApi(databases);
      const bundle = await connect.getRepository(dataBundles);

      const advert = await bundle.findOne({ id: _bundleId })
      if (!advert) return new NotFound('Bundle does not Exists');

      advert.isDeleted = true;
      await bundle.save(advert);
      return new SuccessResponse("Bundle Deleted Successfully");
    } catch (e) {
      return new InternalServerError("Throw Error" + e, e)
    }

  ```

- Distinguish operational vs programmer errors
  Operational errors (e.g. API received an invalid input) refer to known cases where the error impact is fully understood and can be handled thoughtfully. On the other hand, programmer error (e.g. trying to read an undefined variable) refers to unknown code failures that dictate to gracefully restart the application.

  ```
  async function getUser(userID) {
    try{

      const repository = connection.getRepository(User);
      const userData = await repository.find(userID);

      if(!userData) return new NotFound("Unable to find user");

      if(!userData.isProducer) return new BadRequest("The logged in user is not a producer");

      ......

      return new SuccessResponse("Got user",userData);

    }catch (e) {
      throw new InternalServerError("Throw Error",e)
    }
  }
  ```

