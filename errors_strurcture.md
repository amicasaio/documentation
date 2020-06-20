# Error structure

## Introduction

When we write services, we need to handle errors (all good code has to be able to handle its errors). To introduce the problem, we are going to explain our **auth** service.

Our auth service returns validation errors of the form _array of objects_:

```js
[
  {
    location: 'body',
    msg: 'El correo tiene que ser válido.',
    param: 'email',
  },
  {
    location: 'body',
    msg: 'La contraseña debe medir entre 7 y 20 caracteres.',
    param: 'password',
    value: '',
  },
];
```

> This is a thrown error for the signup function when email and password are being provided incorrectly.

The problem comes when we have other services that also throw errors but using other data structure.

```js
// example 1
{
    status: 400,
    err: {
        message: Invalid operation
    }
}

// example 2
{
    statusCode: 400,
    errorDetails: [
        ...
    ]
}
```

We need to consider that all our errors are going to go to the same React frontend application and in this case, the frontend engineers will need to have knowledge of how to parse 30 different kinds of error responses!

It is not the responsability of the frontend engineers to have the knowledge of how to parse all of this different error structures.

So, our solution to this, is that as we start to build different services we need to make absolutely sure to have the same error structure. Our React App has to only have knowledge of how to parse 1 error response.

## Difficulty in Error Handling

The problem introduced on the above section is just one of several. There may be errors in our code not only from validating data, but errors may come from many sources.

From what was written above, we may concludo 2 things:

1. We must have a consistently structured response from all servers, no matter what went wrong.
2. A billion things can go wrong, not just validation of inputs to a request handler. Each of these need to handled consistently.

## Solutions

### Middleware

To address our first problem, we have a error handling middleware to process errors, give them a consitent structure, and send it back to the browser.

We also make sure we capture all possible errors using Express's error handling mechanism called the `next` function. To learn more about how express handles errors, here is a link to its official documentation: [https://expressjs.com/en/guide/error-handling.html](https://expressjs.com/en/guide/error-handling.html)

### Amicasa Error Strucure

We are going to use an array of objects of the following form:

```js
{
  errors: [{
    message: string, field?: string
  }]
}
```

We already have a method for a class `AmicasaError` which validates with typescript that a thrown error is of the following structure.
