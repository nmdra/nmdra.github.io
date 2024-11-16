---
author: nimendra
title: Goodbye Postman?👋 CLI Tools That Will Change Your API Testing Forever
description: "A comprehensive guide to command-line HTTP clients(curl, HTTPie, and xh)."
date: 2024-11-07
lastmod: 2024-11-15
tags: ["curl","xh","httpie","postman","api"]
categories: ["api","devops","devtools"]
showtoc: true
TocOpen: true
ShowReadingTime: true
ShowShareButtons: true
ShowPostNavLinks: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true
editPost:
  URL: "https://github.com/nmdra/nmdra.github.io/tree/main/content"
  Text: "Suggest edit"
  appendFilePath: true
---

You know how we developers spend a lot of time working with APIs, right? While tools like Postman are great, sometimes they can feel a bit heavy-handed - you have to launch the app, navigate through the UI, and deal with occasional crashes or sync issues. Not to mention those times when you're SSH'd into a server and need to quickly test an endpoint!

This is where command-line tools like [`curl`(short for "Client URL")](https://curl.se/
) and jq come in as your trusty sidekicks! 🦸‍♂️ Think of curl as your Swiss Army knife for making HTTP requests - it's always there in your terminal, ready to help. And [jq](https://jqlang.github.io/jq)? It's like having a magic wand ✨ that turns messy JSON responses into beautifully formatted, readable data that you can actually work with.

Let me break down why these command-line tools are so valuable:

1. Speed and Efficiency ⚡
- No need to switch contexts or launch apps
- Lightning-fast execution 🏃‍♀️
- Perfect for quick API checks during development

2. Automation-Friendly 🤖
- Easily integrate into shell scripts
- Perfect for CI/CD pipelines 🔄
- Great for automated testing ✅

Now, about Postman... While it's an amazing tool, it does have some pain points: 🤔
- Takes up significant system resources 🐌
- Can be sluggish to start up
- Sync issues between devices can be frustrating 😫
- Not always available when you need it (like on production servers) 🚫
- Collections can get messy and outdated if not maintained well 🗑️

---

We'll walk through practical examples using a [Fake Store API](https://fakestoreapi.com/docs) &  [dummyjson](https://dummyjson.com/docs), demonstrating the power and flexibility of these tools for effective API testing.

## Curl + jq 🔧

### Get Request

```bash
curl 'https://dummyjson.com/quotes?limit=2&skip=2' | jq
```
***Output:***
> ```json
> {
>   "quotes": [
>     {
>       "id": 3,
>       "quote": "Thinking is the capital, Enterprise is the way, Hard Work is the solution.",
>       "author": "Abdul Kalam"
>     },
>     {
>       "id": 4,
>       "quote": "If You Can'T Make It Good, At Least Make It Look Good.",
>       "author": "Bill Gates"
>     }
>   ],
>   "total": 1454,
>   "skip": 2,
>   "limit": 2
> }
> ```

#### Get verbose output with Additional Details

- `-sS`  → Combines `-s` (silent) and `-S` (show errors) flags, hiding the progress but showing errors if they occur.
- -X → Specifies the `GET` method.
- -v → Display Verbose Output.

```bash
curl -sS -X GET -v 'https://dummyjson.com/quotes?limit=2&skip=2' | jq
```

***Output:***
{{< details >}}

> ```bash
> * Host dummyjson.com:443 was resolved.
> * IPv6: (none)
> * IPv4: 52.223.46.195, 15.197.246.237, 3.33.193.101, 99.83.183.127
> *   Trying 52.223.46.195:443...
> * ALPN: curl offers h2,http/1.1
> } [5 bytes data]
> * TLSv1.3 (OUT), TLS handshake, Client hello (1):
> } [512 bytes data]
> *  CAfile: /etc/ssl/certs/ca-certificates.crt
> *  CApath: none
> { [5 bytes data]
> * TLSv1.3 (IN), TLS handshake, Server hello (2):
> { [122 bytes data]
> * TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
> { [6 bytes data]
> * TLSv1.3 (IN), TLS handshake, Certificate (11):
> { [2568 bytes data]
> * TLSv1.3 (IN), TLS handshake, CERT verify (15):
> { [264 bytes data]
> * TLSv1.3 (IN), TLS handshake, Finished (20):
> { [36 bytes data]
> * TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
> } [1 bytes data]
> * TLSv1.3 (OUT), TLS handshake, Finished (20):
> } [36 bytes data]
> * SSL connection using TLSv1.3 / TLS_AES_128_GCM_SHA256 / x25519 / RSASSA-PSS
> * ALPN: server did not agree on a protocol. Uses default.
> * Server certificate:
> *  subject: CN=dummyjson.com
> *  start date: Oct 23 23:06:05 2024 GMT
> *  expire date: Jan 21 23:06:04 2025 GMT
> *  subjectAltName: host "dummyjson.com" matched cert's "dummyjson.com"
> *  issuer: C=US; O=Let's Encrypt; CN=R10
> *  SSL certificate verify ok.
> *   Certificate level 0: Public key type RSA (2048/112 Bits/secBits), signed using sha256WithRSAEncryption
> *   Certificate level 1: Public key type RSA (2048/112 Bits/secBits), signed using sha256WithRSAEncryption
> *   Certificate level 2: Public key type RSA (4096/152 Bits/secBits), signed using sha256WithRSAEncryption
> * Connected to dummyjson.com (52.223.46.195) port 443
> * using HTTP/1.x
> } [5 bytes data]
> > GET /quotes?limit=2&skip=2 HTTP/1.1
> > Host: dummyjson.com
> > User-Agent: curl/8.10.1
> > Accept: */*
> >
> { [5 bytes data]
> * TLSv1.3 (IN), TLS handshake, Newsession Ticket (4):
> { [122 bytes data]
> * Request completely sent off
> { [5 bytes data]
> < HTTP/1.1 200 OK
> < Report-To: {"group":"heroku-nel","max_age":3600,"endpoints":[{"url":"https://nel.heroku.com/reports?ts=1731089248&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=%2BtT7h9s4wR4dYZSpB3E7HlIX8pjNxpIWMhxgIssNMNY%3D"}]}
> < Reporting-Endpoints: heroku-nel=https://nel.heroku.com/reports?ts=1731089248&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=%2BtT7h9s4wR4dYZSpB3E7HlIX8pjNxpIWMhxgIssNMNY%3D
> < Nel: {"report_to":"heroku-nel","max_age":3600,"success_fraction":0.005,"failure_fraction":0.05,"response_headers":["Via"]}
> < Connection: keep-alive
> < Access-Control-Allow-Origin: *
> < X-Dns-Prefetch-Control: off
> < X-Frame-Options: SAMEORIGIN
> < Strict-Transport-Security: max-age=15552000; includeSubDomains
> < X-Download-Options: noopen
> < X-Content-Type-Options: nosniff
> < X-Xss-Protection: 1; mode=block
> < X-Powered-By: Cats on Keyboards
> < Server: BobTheBuilder
> < X-Ratelimit-Limit: 100
> < X-Ratelimit-Remaining: 99
> < Date: Fri, 08 Nov 2024 18:07:28 GMT
> < X-Ratelimit-Reset: 1731089253
> < Content-Type: application/json; charset=utf-8
> < Content-Length: 257
> < Etag: W/"101-NDFwgjO+/Waza1qMZvvDgnRceXc"
> < Vary: Accept-Encoding
> < Via: 1.1 vegur
> <
> { [65 bytes data]
> * Connection #0 to host dummyjson.com left intact
> {
>   "quotes": [
>     {
>       "id": 3,
>       "quote": "Thinking is the capital, Enterprise is the way, Hard Work is the solution.",
>       "author": "Abdul Kalam"
>     },
>     {
>       "id": 4,
>       "quote": "If You Can'T Make It Good, At Least Make It Look Good.",
>       "author": "Bill Gates"
>     }
>   ],
>   "total": 1454,
>   "skip": 2,
>   "limit": 2
> }
> ```
{{< /details >}}


### POST Request

- -H → sets the content-type
- -d → sends the JSON payload

```bash
curl -sS-X POST https://fakestoreapi.com/products \
-H "Content-Type: application/json" \
-d '{
    "title": "Raspberry Pi 4",
    "price": 13.5,
    "description": "lorem ipsum",
    "image": "https://i.pravatar.cc",
    "category": "electronic"
}' | jq
```
***Output:***

> ```json
> {
>   "id": 21,
>   "title": "Raspberry Pi 4",
>   "price": 13.5,
>   "description": "lorem ipsum",
>   "image": "https://i.pravatar.cc",
>   "category": "electronic"
> }
> ```

### PUT & PATCH Requests

```bash
curl -sS -X PUT https://fakestoreapi.com/products/21 \
-H "Content-Type: application/json" \
-d '{
    "title": "Raspberry Pi 5",
    "price": 11.1,
    "description": "Raspberry Pi 4 ....",
    "image": "https://i.pravatar.cc",
    "category": "electronic"
}' | jq
```

***Output***:
```json
{
  "id": 21,
  "title": "Raspberry Pi 5",
  "price": 11.1,
  "description": "Raspberry Pi 4 ...",
  "image": "https://i.pravatar.cc",
  "category": "electronic"
}
```

#### Use with JSON file

```bash
curl -X PATCH https://fakestoreapi.com/products/21 \
-H "Content-Type: application/json" \
-d @data.json
```

```json
// data.json
{ 
	"title": "test product", 
	"price": 13.5, 
	"description": "lorem ipsum set", 
	"image": "https://i.pravatar.cc", 
	"category": "electronic" 
}
```

### Authentication

#### Get Token

- `--cookie-jar cookies.txt`: Saves cookies (e.g., access tokens) to `cookies.txt` after the request.

```bash
curl -sS -X POST https://dummyjson.com/auth/login \
-H "Content-Type: application/json" \
--cookie-jar cookies.txt \
-d '{
    "username": "emilys",
    "password": "emilyspass",
    "expiresInMins": 30
}' | jq
```

***Output:***

> ```json
> {
>   "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwidXNlcm5hbWUiOiJlbWlseXMiLCJlbWFpbCI6ImVtaWx5LmpvaG5zb25AeC5kdW1teWpzb24uY29tIiwiZmlyc3ROYW1lIjoiRW1pbHkiLCJsYXN0TmFtZSI6IkpvaG5zb24iLCJnZW5kZXIiOiJmZW1hbGUiLCJpbWFnZSI6Imh0dHBzOi8vZHVtbXlqc29uLmNvbS9pY29uL2VtaWx5cy8xMjgiLCJpYXQiOjE3MzEwOTM0MTIsImV4cCI6MTczMTA5NTIxMn0.LpHYf8Zc7EvVzOkSPkvtQncpdWgIKq7-r9lg6_tszzg",
>   "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwidXNlcm5hbWUiOiJlbWlseXMiLCJlbWFpbCI6ImVtaWx5LmpvaG5zb25AeC5kdW1teWpzb24uY29tIiwiZmlyc3ROYW1lIjoiRW1pbHkiLCJsYXN0TmFtZSI6IkpvaG5zb24iLCJnZW5kZXIiOiJmZW1hbGUiLCJpbWFnZSI6Imh0dHBzOi8vZHVtbXlqc29uLmNvbS9pY29uL2VtaWx5cy8xMjgiLCJpYXQiOjE3MzEwOTM0MTIsImV4cCI6MTczMzY4NTQxMn0.onheFA-QlZwgMbhfoxG4zoFBFBOqGeH9j9jmU89cmRY",
>   "id": 1,
>   "username": "emilys",
>   "email": "emily.johnson@x.dummyjson.com",
>   "firstName": "Emily",
>   "lastName": "Johnson",
>   "gender": "female",
>   "image": "https://dummyjson.com/icon/emilys/128"
> ```

#### Refresh Token

```bash
curl -sS -X POST https://dummyjson.com/auth/refresh \
-H "Content-Type: application/json" \
--cookie cookies.txt \
-d '{
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwidXNlcm5hbWUiOiJlbWlseXMiLCJlbWFpbCI6ImVtaWx5LmpvaG5zb25AeC5kdW1teWpzb24uY29tIiwiZmlyc3ROYW1lIjoiRW1pbHkiLCJsYXN0TmFtZSI6IkpvaG5zb24iLCJnZW5kZXIiOiJmZW1hbGUiLCJpbWFnZSI6Imh0dHBzOi8vZHVtbXlqc29uLmNvbS9pY29uL2VtaWx5cy8xMjgiLCJpYXQiOjE3MzEwOTMzMTgsImV4cCI6MTczMzY4NTMxOH0.rCS4sa4AjKA1buLAqNeOBL7aFBj3Z5ZQPO3ZZIUpLp4",
    "expiresInMins": 30
}' | jq
```

Or

```bash
curl -sS -X POST https://dummyjson.com/auth/refresh \
-H "Content-Type: application/json" \
--cookie cookies.txt \
-d '{
    "expiresInMins": 30
}' | jq
```

***Output:***

> ```json
> { 
> 	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...", // new accessToken (returned in both response and cookies) 
> 	"refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." // new refreshToken (returned in both response and cookies) 
> }
> ```

---

## Modern HTTP Clients

### xh - The Modern HTTP Swiss Army Knife 🚀

🔗 https://github.com/ducaale/xh

#### What makes xh special? ✨

- 🎯 **Developer-Friendly Syntax**
  - No more wrestling with complex flags
  - Commands read like plain English
  - Color-coded output that's easy on the eyes 👀

- ⚡ **Blazingly Fast**
  - Built in Rust for maximum performance

- 🎪 **Features**
  - JSON highlighting right out of the box
  - Built-in authentication helpers
  - HTTPS, custom headers

Here's a quick taste of how friendly it is:

*Let's Post a New Product!* 🛍️

##### Using curl (The Classic Way) 🔧
```bash
curl -sS -X POST https://fakestoreapi.com/products \
-H "Content-Type: application/json" \
-d '{
    "title": "Raspberry Pi 4",
    "price": 13.5,
    "description": "lorem ipsum",
    "image": "https://i.pravatar.cc",
    "category": "electronic"
}' | jq
```

##### Using xh (The Modern Way) ✨
```bash
xh POST https://fakestoreapi.com/products \
    title="Raspberry Pi 4" \
    price:=13.5 \
    description="lorem ipsum" \
    image="https://i.pravatar.cc" \
    category="electronic"
```

#### What's Different? 👀

1. **Syntax Simplicity** 🎯
   - curl: Needs `-X POST`, `-H` for headers, `-d` for data
   - xh: Just `POST` and your data - that's it!

2. **JSON Handling** 📦
   - curl: Requires manual JSON formatting and `| jq` for pretty output
   - xh: Automatically formats JSON and handles content types

3. **Type Inference** 🧠
   - curl: Everything is a string in JSON
   - xh: Smart type handling (notice `:=` for numbers)

4. **Headers** 📝
   - curl: Must explicitly set `Content-Type`
   - xh: Automatically handles common headers

{{< notice tip >}}

With xh, you can even preview your request before sending:
```bash
xh --print=HB POST https://fakestoreapi.com/products \
    title="Raspberry Pi 4"
```
(H for headers, B for body)

{{< /notice >}}

### HTTPie

Started as a command-line marvel, it's now evolved into a full-featured suite.

🔗 https://httpie.io/cli


##### Example
```bash
http POST https://fakestoreapi.com/products \
    title="Raspberry Pi 4" \
    price:=13.5 \
    description="lorem ipsum" \
    image="https://i.pravatar.cc" \
    category="electronic"
```
{{< notice tip >}}
you can try above command using httpie's online terminal.  
🔗 https://httpie.io/cli/run
{{< /notice >}}

{{< notice info >}}
Use HTTPie Desktop for visual debugging and team sharing, but keep the CLI handy for quick checks and automation scripts!   
Try web app:- https://httpie.io/app
{{< /notice >}}

---


## Feature Comparison 📊

| Feature | HTTPie 🌈 | xh ⚡ | curl 🐎 |
|---------|-----------|-------|----------|
| Syntax | Modern & Friendly | Modern & Friendly | Traditional |
| Speed | Fast | Very Fast (Rust) | Fast |
| JSON Handling | Auto | Auto | Manual (needs jq) |
| Colors | Built-in | Built-in | None |
| Learning Curve | Easy | Easy | Steeper |
| Platform Support | Excellent | Good | Excellent |
| Memory Usage | Medium | Low | Low |

