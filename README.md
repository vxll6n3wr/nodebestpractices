# Separate Express 'app' and 'server'

### 1.1 Separate Express 'app' and 'server'

**TL;DR:** Separate your Express definition into at least two files: `app.js` (or `app.ts`) which defines the API/router logic without listening to a port, and `server.js` (or `index.js`) which imports `app` and binds it to a port. This makes API integration testing fast and clean using libraries like `supertest` without starting an actual HTTP server.

---

### Code Example

```javascript
// app.js - API definition
const express = require('express');
const app = express();

app.use(express.json());

app.get('/api/v1/health', (req, res) => {
  res.status(200).json({ status: 'ok' });
});

module.exports = app;
```

```javascript
// server.js - Server listener
const app = require('./app');
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`);
});
```

### Why it helps

Testing the API using tools like `supertest` allows issuing HTTP calls directly against the Express `app` instance without binding to network ports. This prevents port collision issues during parallel test executions and speeds up test suites significantly.