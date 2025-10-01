
---

# Mini Postman Clone 

This project is a **basic API client** built with **HTML + JS **.
Currently it allows:

* Selecting **GET/POST** methods
* Entering **URL**
* Sending requests via `fetch()`
* Displaying **JSON response** dynamically

But real Postman does a lot more. Here’s a complete list of Postman’s features and **how we could implement them in this project**.

---

## 1️⃣ HTTP Methods

**Postman:** Supports GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD.
**Mini Clone:** Only GET/POST currently.
**How to improve:**

* Extend `<select>` to include all HTTP methods.
* `fetch(inputUrl.value, { method: inputMethod.value })` already supports them.

---

## 2️⃣ Request Body

**Postman:** Allows sending data in different formats:

* **raw JSON**
* **form-data** (for file uploads)
* **x-www-form-urlencoded**
* **GraphQL queries**

**Mini Clone:** No body support yet.
**How to implement:**

* Add a `<textarea>` for request body input.
* Use `fetch` body:

```js
const bodyData = inputMethod.value !== "GET" ? bodyTextarea.value : undefined;
const response = await fetch(inputUrl.value, {
  method: inputMethod.value,
  headers: { "Content-Type": "application/json" },
  body: bodyData
});
```

* Optionally let users select content type (`application/json`, `application/x-www-form-urlencoded`, `multipart/form-data`).

---

## 3️⃣ Headers

**Postman:** User can add custom headers (Authorization, Content-Type, tokens, etc.)

**Mini Clone:** No headers yet.
**How to implement:**

* Add a dynamic table with `key` and `value` inputs for headers.
* Build a JS object from user input:

```js
const headers = {};
headerInputs.forEach(h => headers[h.key] = h.value);

fetch(inputUrl.value, {
  method: inputMethod.value,
  headers: headers,
  body: bodyData
});
```

---

## 4️⃣ Query Parameters

**Postman:** Supports dynamic query parameters in the URL.

**Mini Clone:** Currently users have to manually type `?key=value`.
**How to implement:**

* Add a small form/table for query key-value pairs.
* Convert to query string:

```js
const queryString = new URLSearchParams(queryParams).toString();
const fullUrl = inputUrl.value + "?" + queryString;
```

* Use `fullUrl` in `fetch`.

---

## 5️⃣ Environments & Variables

**Postman:** Variables like `{{baseUrl}}`, `{{token}}` allow dynamic substitution.

**Mini Clone:** None yet.
**How to implement:**

* Maintain a JS object of variables:

```js
const env = { baseUrl: "https://api.example.com", token: "ABC123" };
const finalUrl = inputUrl.value.replace(/{{(\w+)}}/g, (_, v) => env[v]);
```

* Replace variables in headers, query params, and body.

---

## 6️⃣ Response Viewer

**Postman:** Shows response in multiple formats:

* Pretty JSON
* Raw
* Preview (HTML)
* Visualize charts

**Mini Clone:** Only pretty JSON with `<div>` currently.
**How to improve:**

* Wrap response in `<pre>` for spacing.
* Add a toggle button for **Raw / Pretty / HTML** views.
* Optional: libraries like [highlight.js](https://highlightjs.org/) for syntax coloring.

---

## 7️⃣ Tests & Automation

**Postman:** Users can write JS tests on response to validate automatically.

**Mini Clone:** None yet.
**How to implement:**

* Add a `<textarea>` for test scripts.
* After `fetch` completes, run:

```js
const tests = new Function("response", testScript);
tests(resultedData);
```

* Show pass/fail results in a div.

---

## 8️⃣ Collections & History

**Postman:** Saves requests, organizes into folders.

**Mini Clone:** Nothing yet.
**How to implement:**

* Store requests in **localStorage**:

```js
localStorage.setItem("history", JSON.stringify(requestHistory));
```

* Render saved requests in a sidebar, allow re-click to resend.

---

## 9️⃣ Authentication Support

**Postman:** Supports:

* Basic Auth
* Bearer Token
* OAuth 2.0
* API keys

**Mini Clone:** None.
**How to implement:**

* Add dropdown to select auth type.
* Auto-inject headers or URL params based on selection.

---

## 🔟 File Uploads / Form-Data

**Postman:** Can send files with POST requests.

**Mini Clone:** Not supported.
**How to implement:**

* Use `<input type="file">`
* Send using `FormData`:

```js
const formData = new FormData();
formData.append("file", fileInput.files[0]);
fetch(inputUrl.value, { method: "POST", body: formData });
```

---

## 1️⃣1️⃣ Multi-Protocol Support

**Postman:** GraphQL, WebSocket, SOAP, gRPC.
**Mini Clone:** Only REST over HTTP.
**How to implement:**

* GraphQL: send POST with JSON `{ query: "..." }`
* WebSocket: use `WebSocket` API in JS
* SOAP: send XML body with correct headers

---

## 1️⃣2️⃣ Error Handling & Status Info

**Postman:** Shows status code, response time, size.

**Mini Clone:** Only response body.
**How to implement:**

```js
const startTime = Date.now();
const response = await fetch(...);
const endTime = Date.now();

console.log("Status:", response.status);
console.log("Time:", endTime - startTime, "ms");
console.log("Size:", new Blob([await response.text()]).size, "bytes");
```

---

