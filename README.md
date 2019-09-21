# netlify-redirect-testcase

Test app to reproduce redirect issue in Lambda function

## Config

```
[build]
  command = "# no build command"
  functions = "functions"
  publish = "."

  [[redirects]]
    from = "/z"
    to = ".netlify/functions/analyze"
    status = 200
```

# Basic Function

```
exports.handler = async (event, context) => {
  try {
    const subject = event.queryStringParameters.name || "World";
    return {
      statusCode: 200,
      body: JSON.stringify({ message: `Hello ${subject}` })
      // // more keys you can return:
      // headers: { "headerName": "headerValue", ... },
      // isBase64Encoded: true,
    };
  } catch (err) {
    return { statusCode: 500, body: err.toString() };
  }
};
```

## Steps

- netlify dev
- http://localhost:8888/z -> "Hello, World"
- http://localhost:8888/z?name=dt -> "Hello, dt"
