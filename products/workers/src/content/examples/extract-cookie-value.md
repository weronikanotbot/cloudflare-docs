---
order: 1000
type: example
summary: Given the cookie name, get the value of a cookie. You can also use cookies for A/B testing.
tags:
  - Headers
pcx-content-type: configuration
---

# Cookie parsing

<ContentColumn>
  <p>{props.frontmatter.summary}</p>
</ContentColumn>

```js
import { parse } from "cookie"

// The name of the cookie
const COOKIE_NAME = "__uid"

function handleRequest(request) {
  const cookie = parse(request.headers.get("Cookie") || "")

  if (cookie[COOKIE_NAME] != null) {
    // Respond with the cookie value
    return new Response(cookie[COOKIE_NAME])
  }

  return new Response("No cookie with name: " + COOKIE_NAME)
}

addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request))
})
```

<Aside type="note" header="External Dependencies">

This example requires the npm package [`cookie`](https://www.npmjs.com/package/cookie) to be installed in your JavaScript project.

</Aside>
