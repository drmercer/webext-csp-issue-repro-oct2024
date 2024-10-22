> ![NOTE]
>
> A Chromium issue was filed for this problem here: https://issues.chromium.org/issues/363027634

This reproduces an issue that started happening in Chromium 130, I believe.

1. Load this extension unpacked
2. Go to https://example.com/
3. Note the errors in the console:

> content.js:1 Refused to load the script 'chrome-extension://94d40efc-bd8d-4ff7-afe0-0292a450109b/module.js' because it violates the following Content Security Policy directive: "script-src 'self' 'wasm-unsafe-eval' 'inline-speculation-rules' http://localhost:* http://127.0.0.1:*". Note that 'script-src-elem' was not explicitly set, so 'script-src' is used as a fallback.
>
> example.com/:1 Uncaught (in promise) TypeError: Failed to fetch dynamically imported module: chrome-extension://94d40efc-bd8d-4ff7-afe0-0292a450109b/module.js

**Workaround:** Remove `"use_dynamic_url": true` from the entry in `web_accessible_resources`.

What's weird:

* This didn't happen in Chromium 129 as far as I know, but started happening in 130. (I first observed this in Edge, but it also happens in Brave 1.71 and Chromium 130.0.6723.58.)
* There's no CSP set anywhere in my manifest, and the [default CSPs](https://developer.chrome.com/docs/extensions/reference/manifest/content-security-policy#default_policy) for web extensions don't look like the one mentioned in this error (no `localhost`, for example).
