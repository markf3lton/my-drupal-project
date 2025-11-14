### Cloudflare CDN

It can helpful to test CDN behavior with a custom domain (preview.example.com). 

You can do this with a Cloudflare Worker.


```
// Update TARGET_URL when you have a new Tugboat preview ID

const TARGET_URL = 'https://preview-poh3zyl1_PREVIEW_ID.tugboatqa.com';

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url);

  // Build target URL with the original path and query string
  const targetUrl = TARGET_URL + url.pathname + url.search;

  // Forward the request to Tugboat preview
  const response = await fetch(targetUrl, {
    method: request.method,
    headers: request.headers,
    body: request.body
  });

  // Return the response
  return response;
}
```

#### Prerequisites

- Your domain uses Cloudflare nameservers
- Add a subdomain DNS record, e.g. 
```
preview   CNAME   previews.tugboatqa.com
```
