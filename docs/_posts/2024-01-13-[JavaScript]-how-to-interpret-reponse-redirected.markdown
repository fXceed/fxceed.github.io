---
layout: post
title:  "[JavaScript] How to Interpret 'response.redirected'"
date:   2024-01-13 16:27:42 -0500
categories: javascript development web
---
### What `response.redirected == true` Means

When `response.redirected` is `true` in the context of a `fetch` API response, it means:

1. **The Fetch Request Encountered a Redirect:** The server responded to the original request with a redirection status (like 301, 302, 303, etc.).

2. **The Fetch API Followed the Redirect:** The `fetch` API automatically followed the redirect to the new URL specified by the server. This is the default behavior unless you have specified `redirect: 'manual'`.

3. **The Response is From the Redirected URL:** The response you are now handling in JavaScript is from the final URL after the redirection. However, it does not mean that the browser's window location (the URL in the address bar) has been updated to the redirected URL.

### How to Interpret `response.redirected`

- **As a Signal to Update Browser's Location:** When `response.redirected` is `true`, and you have a specific reason to navigate the user to this new URL (such as mimicking a full-page redirect), you should then manually update `window.location.href` to `response.url`.
- **Not a Browser Navigation Event:** It's important to note that `response.redirected == true` does not automatically change the page the user is currently viewing. To change the page, you must manually set `window.location.href`.

### Example Usage

In a scenario where you want the user's browser to navigate to the URL that the server has redirected to, you would use `window.location.href`:

```javascript
if (response.redirected) {
    // The fetch API has followed a redirect, and now you can manually navigate the browser to that URL
    window.location.href = response.url;
}
```

In this code snippet, when `response.redirected` is `true`, it indicates that the server has sent a redirect response and `fetch` has followed it. By setting `window.location.href` to `response.url`, you are manually navigating the browser to the redirected URL, effectively completing the redirection process on the frontend.
