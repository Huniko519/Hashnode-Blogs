# Unit testing Next.js API routes

Unit testing [Next.js API routes](https://nextjs.org/docs/api-routes/introduction) is possible with [jest](https://jestjs.io/) and straightforward once you see an example.

Straight from the Next.js docs, API route handlers look like this:

```javascript
// pages/api/user.js
export default (req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'application/json');
  res.end(JSON.stringify({ name: 'John Doe' }));
};
```

> *Hey, that's just a function! Those are easy to test!*

I hear you, internet friend. It is a function indeed and functions are easy to test, but this has a bit more going on than just [adding 2 numbers](https://jestjs.io/docs/en/getting-started).

## Example API route[#](https://seanconnolly.dev/unit-testing-nextjs-api-routes#example-api-route)

This blog is built with Next.js so I added this [/api/animal/\[animal\]](https://seanconnolly.dev/api/animal/cat) route, which accepts a dynamic `[animal]` parameter at the end and returns a JSON object with a dynamic message.

```javascript
// /pages/api/[animal].js
export default function handleAnimal(req, res) {
  const {
    query: { animal },
  } = req;

  res.statusCode = 200;
  res.setHeader('Content-Type', 'application/json');
  res.end(JSON.stringify({ message: `Your favorite animal is ${animal}` }));
}
```

## The unit test[#](https://seanconnolly.dev/unit-testing-nextjs-api-routes#the-unit-test)

The key here is the `createMocks` utility provided by `node-mocks-http`. It creates mock `req` and `res` objects that you can later interrogate after your async route handler executes.

```javascript
// __tests__/animal.test.js
// 🚨 Remember to keep your `*.test.js` files out of your `/pages` directory!
import { createMocks } from 'node-mocks-http';
import handleAnimal from '../pages/api/animal/[animal]';

describe('/api/[animal]', () => {
  test('returns a message with the specified animal', async () => {
    const { req, res } = createMocks({
      method: 'GET',
      query: {
        animal: 'dog',
      },
    });

    await handleAnimal(req, res);

    expect(res._getStatusCode()).toBe(200);
    expect(JSON.parse(res._getData())).toEqual(
      expect.objectContaining({
        message: 'Your favorite animal is dog',
      }),
    );
  });
});
```

That's it!

Thanks for reading.