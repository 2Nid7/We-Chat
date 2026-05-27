## Realtime Chat Application

A full‑stack chat application with user authentication, avatars, contacts list, and one‑to‑one messaging.

### Features
- **User auth**: Register, login, persisted session
- **Profile avatars**: Set and update profile avatar
- **Contacts**: View all other users and select a chat
- **Chat**: Send and view messages in real time-ready structure
- **Notifications & UI**: Toast notifications, responsive React UI

### Tech Stack
- **Frontend**: React 18, React Router, CSS Modules, React Toastify, Emoji Picker
- **Backend**: Node.js, Express, Mongoose (MongoDB)
- **Tooling**: CRA (react-scripts), Nodemon

---

### Monorepo Structure

```
Chat-Application/
├─ Public/              # React frontend (CRA)
│  ├─ package.json
│  └─ src/
│     ├─ Pages/
│     │  ├─ Chat/
│     │  ├─ ChatContainer/
│     │  ├─ ChatInput/
│     │  ├─ Contacts/
│     │  ├─ Login/
│     │  ├─ Logout/
│     │  ├─ Register/
│     │  ├─ setAvatar/
│     │  └─ Welcome/
│     └─ utils/apiRoutes.js
└─ Server/              # Express + MongoDB backend
   ├─ index.js
   ├─ connection.js
   ├─ Routers/
   ├─ controllers/
   └─ Models/
```

---

### Prerequisites
- Node.js 18+ and npm or yarn
- MongoDB database (Atlas or local)

---

### Backend Configuration (Server)

The repository currently has the MongoDB connection string hardcoded in `Server/connection.js`. For security, move secrets to environment variables before deploying.

1) Create a `.env` file in `Server/` and add:

```env
MONGODB_URI="your-mongodb-connection-string"
PORT=5000
CLIENT_ORIGIN=http://localhost:3000
```

2) Update `Server/connection.js` to read from env:

```js
const mongoose = require("mongoose");
const URL = process.env.MONGODB_URI;
mongoose.connect(URL)
  .then(() => console.log("Connected to MongoDB"))
  .catch((err) => console.log(err.message));
module.exports = mongoose;
```

3) Update `Server/index.js` to use env and not require admin port 80:

```js
const PORT = process.env.PORT || 5000;
const CLIENT_ORIGIN = process.env.CLIENT_ORIGIN || "http://localhost:3000";
// ...
res.setHeader("Access-Control-Allow-Origin", CLIENT_ORIGIN);
```

Note: The current code listens on port `80` which may require elevated permissions. Using `5000` during development is recommended.

---

### Frontend Configuration (Public)

The frontend targets the backend at `http://localhost:80` in `Public/src/utils/apiRoutes.js`. If you changed the backend port, update this file accordingly:

```js
const host = "http://localhost:5000"; // or your deployed URL
```

---

### Installation

Install dependencies for both frontend and backend:

```bash
# From repo root
cd Server && npm install
cd ../Public && npm install
```

If you prefer yarn:

```bash
cd Server && yarn
cd ../Public && yarn
```

---

### Running the App (Development)

Run backend and frontend in two terminals:

```bash
# Terminal 1 – Backend
cd Server
npm run start

# Terminal 2 – Frontend
cd Public
npm start
```

Default URLs:
- Frontend: `http://localhost:3000`
- Backend: `http://localhost:5000` (if you updated from 80 as recommended)

---

### Available Scripts

Backend (`Server/package.json`):
- `npm start`: Start Express with Nodemon

Frontend (`Public/package.json`):
- `npm start`: Start React dev server
- `npm run build`: Production build
- `npm test`: Run tests (CRA)
- `npm run eject`: Eject CRA config

---

### REST API Overview

Base URL: `http://localhost:{PORT}` (defaults to `80` in current code; recommended `5000`)

- `POST /api/auth/register` – Register a new user
- `POST /api/auth/login` – Login and get user data
- `POST /api/auth/setAvatar/:id` – Set avatar for user by id
- `GET  /api/auth/allUsers/:id` – Get list of users excluding id
- `POST /api/messages/addMessages` – Add a message to a conversation
- `POST /api/messages/getAllMessages` – Get all messages for a conversation

Request/response contracts are defined in `Server/controllers/` and Mongoose models in `Server/Models/`.

---

### Environment Variables Summary
- `MONGODB_URI`: MongoDB connection string
- `PORT`: Backend server port (use 5000 in dev)
- `CLIENT_ORIGIN`: Frontend origin for CORS

You can use a `.env` file in `Server/` with these keys.

---

### Building for Production

```bash
cd Public
npm run build
```

This creates an optimized production build in `Public/build`. Serve the frontend with a static host (Netlify, Vercel, S3, etc.) and deploy the backend separately (Render, Railway, Heroku, VPS). Make sure `apiRoutes.js` points to your deployed backend URL.

---

### Security Notes
- Do not commit real MongoDB credentials. Use environment variables.
- Restrict CORS to trusted origins only in production.

---

### Troubleshooting
- Backend not starting on port 80 on Windows: change `PORT` to `5000` and update `apiRoutes.js`.
- CORS errors: ensure `CLIENT_ORIGIN` matches your frontend URL.
- Cannot connect to MongoDB: verify `MONGODB_URI` and network allowlist (Atlas IP access).

---

### Contact
Have questions or feedback? Reach out:

```markdown
## Contact

- Email: adarshnampalli71@gmail.com

