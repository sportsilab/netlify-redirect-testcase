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

### Local

- netlify dev
- http://localhost:8888/z -> "Hello, World"
- http://localhost:8888/z?name=dt -> "Hello, dt"

### Live

- netlify dev --live
- https://inspiring-ptolemy-fc3065-36f454.netlify.live/z -> "Hello, World"
- https://inspiring-ptolemy-fc3065-36f454.netlify.live/z?name=dt-> "Hello, dt"

### Deploy

- netlify deploy

```
Deploying to draft URL...
✔ Finished hashing 3 files and 1 functions
✔ CDN requesting 1 files and 1 functions
✔ Finished uploading 2 assets
✔ Draft deploy is live!

Logs:           https://app.netlify.com/sites/inspiring-ptolemy-fc3065/deploys/5d86141b607c85c12644fcb3
Live Draft URL: https://5d86141b607c85c12644fcb3--inspiring-ptolemy-fc3065.netlify.com

If everything looks good on your draft URL, take it live with the --prod flag.
netlify deploy --prod
```

- https://5d86141b607c85c12644fcb3--inspiring-ptolemy-fc3065.netlify.com/z -> 404 not found
- https://5d86141b607c85c12644fcb3--inspiring-ptolemy-fc3065.netlify.com/z?name=dt -> 404 not found

### Prod

`Live URL: https://inspiring-ptolemy-fc3065.netlify.com`

- https://inspiring-ptolemy-fc3065.netlify.com/z -> 404 not found
- https://inspiring-ptolemy-fc3065.netlify.com/z?name=dt -> 404 not found
