---
title: API Keys
excerpt: >-
  Get a free Sefaria API key, send it with your requests, and limit it to your
  website.
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    How to get a free Sefaria API key in Developer Settings, send it in the
    x-api-key header, limit it to your website, and read the responses that
    mention your key.
  robots: index
next:
  description: ''
---
## What an API key is for

An API key tells Sefaria which of your projects a request comes from. Keys are free.

- **Most endpoints still work without a key.** You can keep calling them as before.
- **Some endpoints need a key.** Today that is semantic search (`/api/knn-search`). Without a valid key it returns `401`.
- **Requests with a key are counted for your project,** rather than mixed in with anonymous traffic.

We recommend getting a key for any project that calls the API.

## Get a key

1. Sign in to [sefaria.org](https://www.sefaria.org), or create a free account.
2. Open **Settings**, then the **Developer** tab ([sefaria.org/settings/developer](https://www.sefaria.org/settings/developer)), and choose **Get started**.
3. If you signed up with an email address, confirm it. Accounts that use Google or Apple sign-in skip this step.
4. Tell us a little about yourself, then add your project: a name, what you are building, and a short description. The website is optional.
5. Create a key for the project, and give it a name you will recognise (for example, "Production").

Your keys appear under **Projects and API keys**. Use **Show** or **Copy** to see a key's full value. An account can have up to 10 projects, and each project up to 5 keys.

<Callout icon="🔒" theme="warn">
  ### Keep your key private

  Don't commit a key to a public repository or paste it into a public page. If a key leaks, delete it in Developer Settings and create a new one. A deleted key stops working right away.
</Callout>

## Send your key

Send the key in the `x-api-key` request header. Keys start with `sfr_`.

```shell
curl https://www.sefaria.org/api/v3/texts/Genesis.1.1 \
  -H "x-api-key: sfr_your_key_here"
```

```javascript
const response = await fetch("https://www.sefaria.org/api/v3/texts/Genesis.1.1", {
  headers: { "x-api-key": "sfr_your_key_here" },
});
```

```python
import requests

response = requests.get(
    "https://www.sefaria.org/api/v3/texts/Genesis.1.1",
    headers={"x-api-key": "sfr_your_key_here"},
)
```

<Callout icon="❗️" theme="error">
  ### Always use the header, never the URL

  Don't put your key in the query string (`?api_key=...`). URLs end up in logs, browser history and analytics tools, so a key in the URL is easy to leak. Headers are not recorded that way.
</Callout>

## Limit a key to your website

If your key goes in a web page's code, anyone can read it there. To stop it from being reused on other sites, turn on **Limit to website** for that key in Developer Settings. The limit uses your project's website.

- **The key works on your website's domain and all its subdomains.** A key limited to `example.com` works on `example.com`, `www.example.com` and `app.example.com`, but not on `badexample.com` or `example.com.other-site.net`. The scheme (`http` or `https`) and the port don't matter.
- **A limited key only works from web pages on that domain.** It refuses requests from any other site, and requests with no `Origin` header at all: servers, scripts, mobile apps and AI tools. Leave the limit off for keys used there.
- **The limit is set per key.** A project can have a limited key for its live site next to an unlimited key for local development, staging or a server.
- **Shared hosting addresses can't be used as the website.** A domain that many unrelated sites share, such as `vercel.app`, `github.io` or `sites.google.com`, would let every site on it use your key, so it can't be the target of a limit. Your own subdomain on such a host, such as `my-project.vercel.app`, is fine: the key then works only on that subdomain and its own subdomains.
- **Removing the project's website turns the limit off** for its keys, so they then work from anywhere. Developer Settings warns you first.

This limit relies on the `Origin` header that browsers send. It protects a key that sits in a public web page; it doesn't protect a key used outside a browser, so keep server-side keys private.

## Responses about your key

When something is wrong with a key on an endpoint that needs one, the response is JSON with three fields: a message, a code, and a link to help.

```json
{
  "error": "This route requires an x-api-key header.",
  "code": "api_key_required",
  "help": "https://developers.sefaria.org/#api_key_required"
}
```

| Status | `code` | What it means | What to do |
| :----- | :----- | :------------ | :--------- |
| `401` | `api_key_required` | The endpoint needs a key and the request had none. | Send your key in the `x-api-key` header. |
| `401` | `api_key_invalid` | The key isn't recognised, or it was deleted. | Copy the key again from Developer Settings, or create a new one. |
| `403` | `origin_not_allowed` | The key is limited to a website, and the request came from another site or had no `Origin` header. | Call from your website, or use a key without the limit for servers, scripts, apps and AI tools. |

### The `x-sefaria-key-status` header

On endpoints that don't need a key, a key that isn't recognised doesn't block the request. The request is answered as if it had no key, and the response carries this header:

```
x-sefaria-key-status: not_accepted
```

If you see it, your key was not used. Check that it was copied correctly and hasn't been deleted.

## Bulk downloads

The API is for live use in apps, sites and tools. If you need texts in bulk, use the free [Sefaria data export](https://github.com/Sefaria/Sefaria-Export) instead. Scripts that systematically download texts through the website or the API, or that copy content to rebuild it elsewhere, are not allowed.

Each text on Sefaria has its own licence, listed with each version in the text's table of contents. Sefaria follows each text's licence, and anyone who gets a text from Sefaria must follow it too.

## Questions

Email [developers@sefaria.org](mailto:developers@sefaria.org) or [contact us](page:contact-us).
