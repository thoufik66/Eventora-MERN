# 🏆 Master Project & Web Development Interview Guide
> **Target Roles**: Infosys Specialist Programmer (SP ~9.5 LPA) & Digital Specialist Engineer (DSE ~6.25 LPA)  
> **Projects Covered**:  
> 1. **Eventora** (`Eventora-MERN`) — MERN Stack Event Management & Ticket Booking System  
> 2. **TripMate** (`TripMate`) — Travel Destination Listing, Geospatial Map & Booking Platform  

---

## 📑 TABLE OF CONTENTS
1. **Web Development Fundamentals Simplified (Must Know)**
   - What is MERN Stack?
   - What is a REST API? (HTTP Methods, Request/Response, Status Codes)
   - React Core Concepts (Components, JSX, State `useState`, Effects `useEffect`, Props)
   - Backend Core Concepts (Node.js, Express, Middleware, Controllers, Routes, Models)
   - Database Concepts (MongoDB, Collections, Documents, Schemas, ORM/Mongoose)
2. **Project 1: Eventora — Complete Architecture & Structure**
   - Folder & File Tree Explained
   - Backend Breakdown (Server, Auth, Booking, Event, OTP)
   - Frontend Breakdown (Vite, Pages, Components, State Flow)
   - 10 Technical Questions Interviewers Will Ask on Eventora
3. **Project 2: TripMate — Complete Architecture & Structure**
   - Folder & File Tree Explained
   - Tech Stack Highlights (Mapbox GL, Cloudinary, Multer, Cookie-Parser, Framer Motion)
   - Backend Breakdown (Auth, Listings, Bookings, User Models & Controllers)
   - Frontend Breakdown (React 19, Mapbox Integration, Toast Notifications)
   - 10 Technical Questions Interviewers Will Ask on TripMate
4. **How Frontend & Backend Communicate (The Full API Request Lifecycle)**
5. **Top 20 Technical Q&A Cheat-Sheet (Web Dev & System Design for SP/DSE)**

---

# 1. WEB DEVELOPMENT FUNDAMENTALS SIMPLIFIED

If the interviewer asks basic or deep questions about Web Development, use these exact simple explanations.

### A. What is MERN Stack?
* **M** = **MongoDB**: NoSQL database that stores data in JSON-like documents.
* **E** = **Express.js**: Lightweight web server framework for Node.js to handle API routes.
* **R** = **React.js**: Frontend JavaScript library for building interactive user interfaces.
* **N** = **Node.js**: JavaScript runtime environment that lets you execute JS code on the backend server.

---

### B. What is a REST API?
A **REST API** (Representational State Transfer Application Programming Interface) is a set of rules that allows the frontend (React) and backend (Express) to communicate over HTTP using JSON data format.

#### 1. HTTP Methods (Verbs):
* `GET`: Fetch data from server (e.g., `GET /api/events` - Get list of events).
* `POST`: Send new data to server (e.g., `POST /api/auth/register` - Create new user).
* `PUT` / `PATCH`: Update existing data (e.g., `PUT /api/bookings/123` - Update booking status).
* `DELETE`: Remove data from server (e.g., `DELETE /api/events/123` - Delete an event).

#### 2. Anatomy of an API Request:
* **URL / Endpoint**: The address (e.g., `http://localhost:5000/api/bookings`).
* **Headers**: Metadata sent with request (e.g., `Content-Type: application/json`, `Authorization: Bearer <token>`).
* **Body**: JSON payload sent during `POST`/`PUT` (e.g., `{ "eventId": "65a...", "otp": "123456" }`).

#### 3. Common HTTP Status Codes:
* `200 OK`: Request succeeded.
* `201 Created`: New resource successfully created.
* `400 Bad Request`: Client error (invalid input data or missing fields).
* `401 Unauthorized`: User is not logged in / missing JWT token.
* `403 Forbidden`: User is logged in but lacks admin permissions.
* `404 Not Found`: Requested route or DB document does not exist.
* `500 Internal Server Error`: Server code crashed or database failed.

---

### C. React Core Concepts (Frontend)

#### 1. What is a Component?
A React Component is a reusable piece of UI (like a Button, Navbar, or Event Card). Components return HTML-like code called **JSX** (JavaScript XML).

#### 2. What is `State` vs `Props`?
* **State (`useState`)**: Internal data owned and managed by a component that can change over time (e.g., `const [user, setUser] = useState(null)`). When state updates, React **re-renders** the UI.
* **Props (Properties)**: Read-only data passed down from a parent component to a child component (e.g., `<EventCard title="Music Fest" price={100} />`).

#### 3. What is `useEffect`?
`useEffect` is a React Hook used to perform **side effects** in components, such as fetching data from an API when the page loads:
```javascript
useEffect(() => {
  // Runs once when component mounts on screen
  fetchEventsFromAPI();
}, []); // Empty dependency array means run on page load
```

#### 4. What is React Router (`react-router-dom`)?
React Router allows navigation between different pages in a Single Page Application (SPA) without reloading the browser (e.g., navigating from `/` to `/dashboard`).

---

### D. Express & Node.js Core Concepts (Backend)

#### 1. Middleware:
Functions that sit between the incoming request and the final controller handler. Used for logging, CORS, body parsing, and authentication verification.
```javascript
// Middleware checks JWT token before allowing access
const protect = (req, res, next) => {
   if (!token) return res.status(401).json({ message: 'Unauthorized' });
   next(); // Pass control to the controller function
};
```

#### 2. Controller:
The function containing the core business logic (e.g., calculating prices, verifying OTPs, updating DB).

#### 3. Route:
Maps an HTTP method + URL path to a controller function (e.g., `router.post('/login', authController.login)`).

---

# 2. PROJECT 1: EVENTORA — COMPLETE STRUCTURE & EXPLANATION

### A. Folder & File Directory Tree
```
Eventora-MERN/
├── server/                    # Node.js + Express Backend
│   ├── config/                # Environment & Database setup
│   ├── controllers/           # Business Logic
│   │   ├── authController.js    # Register, Login, 2FA OTP verification
│   │   ├── bookingController.js # Send OTP, Create Booking, Admin Confirm/Cancel
│   │   └── eventController.js   # Create, Edit, Delete, Fetch Events
│   ├── middleware/            # JWT Auth & Admin Protection
│   │   └── authMiddleware.js    # Protect routes & verify user role
│   ├── models/                # MongoDB Mongoose Schemas
│   │   ├── User.js              # User schema (name, email, password, isVerified, role)
│   │   ├── Event.js             # Event schema (title, seats, price, createdBy)
│   │   ├── Booking.js           # Booking schema (userId, eventId, status, paymentStatus)
│   │   └── OTP.js               # 2FA OTP schema with TTL expiry index
│   ├── routes/                # API Endpoints definition
│   │   ├── authRoutes.js        # /api/auth routes
│   │   ├── bookingRoutes.js     # /api/bookings routes
│   │   └── eventRoutes.js       # /api/events routes
│   ├── utils/                 # Utility functions
│   │   └── email.js             # Nodemailer configuration for sending emails
│   ├── .env                   # DB Secrets, JWT_SECRET, Email credentials
│   ├── seed.js                # Database initialization script with mock data
│   └── server.js              # Entry point of Express app
└── client/                    # React 18 + Vite Frontend
    ├── src/
    │   ├── components/        # Reusable UI components (Navbar, EventCard, Modal)
    │   ├── pages/             # Pages (Home, Login, Register, AdminDashboard, UserDashboard)
    │   ├── context/           # AuthContext (Global state for logged-in user token)
    │   ├── App.jsx            # Main App Router setup
    │   └── main.jsx           # Entry point rendering React DOM
    └── package.json           # Frontend dependencies
```

---

### B. Eventora API Endpoints Table

| Method | Endpoint | Access | Purpose |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Public | Registers user & sends 2FA OTP via Nodemailer |
| `POST` | `/api/auth/verify-otp` | Public | Verifies account activation OTP & returns JWT token |
| `POST` | `/api/auth/login` | Public | Authenticates user & returns JWT token |
| `GET` | `/api/events` | Public | Fetches all available events |
| `POST` | `/api/events` | Admin Only | Creates a new event |
| `POST` | `/api/bookings/send-otp` | Protected User | Sends OTP for event booking |
| `POST` | `/api/bookings/book` | Protected User | Validates booking OTP & creates `pending` booking |
| `PUT` | `/api/bookings/confirm/:id`| Admin Only | Confirms pending booking & deducts available seat |
| `PUT` | `/api/bookings/cancel/:id` | Protected User/Admin | Cancels booking & restores available seat |

---

# 3. PROJECT 2: TRIPMATE — COMPLETE STRUCTURE & EXPLANATION

### A. Folder & File Directory Tree
```
TripMate/
├── backend/                    # Express.js REST API Server
│   ├── config/                # DB Connection setup
│   │   └── db.js              # Mongoose connect setup
│   ├── controllers/           # Business Controllers
│   │   ├── auth.controller.js   # User Register, Login, Cookie Management
│   │   ├── listing.controller.js# Travel Destinations CRUD, Mapbox Geocoding, Cloudinary Upload
│   │   ├── booking.controller.js# Reservation & Booking system
│   │   └── user.controller.js   # User Profile Management
│   ├── model/                 # MongoDB Data Schemas
│   │   ├── user.model.js        # User model (name, email, password, avatar)
│   │   ├── listing.model.js     # Travel Destination (title, location, geometry, price, image)
│   │   └── booking.model.js     # Booking model (listingId, userId, checkIn, checkOut, price)
│   ├── middleware/            # Security & File Uploads
│   │   ├── auth.middleware.js   # Verify JWT from HttpOnly Cookie
│   │   └── multer.middleware.js # File upload handling for images
│   ├── routes/                # Route definitions (/api/auth, /api/listings, /api/bookings)
│   ├── index.js               # Main server entry point
│   └── package.json           # Dependencies (express, jsonwebtoken, cloudinary, mapbox, cookie-parser)
└── frontend/                  # React 19 + Vite Frontend
    ├── src/
    │   ├── components/        # Mapbox Interactive Maps, Navbar, Footer, ListingCard
    │   ├── pages/             # Home, ListingDetails, CreateListing, BookingsPage
    │   ├── App.jsx            # React Router v7 Routing Setup
    │   └── main.jsx           # Vite Mount point
    └── package.json           # Dependencies (react 19, mapbox-gl, react-map-gl, framer-motion, axios)
```

---

### B. Special Technical Highlights of TripMate

1. **Mapbox GL Integration (`mapbox-gl` & `react-map-gl`)**:
   * Converts address text (e.g., "Paris, France") into geographic coordinates (`latitude`, `longitude`) using Mapbox Forward Geocoding API.
   * Renders interactive 3D maps on the frontend with custom destination pins.
2. **Cloudinary & Multer File Uploads**:
   * Uses **Multer** as backend middleware to receive image files from HTTP `multipart/form-data` uploads.
   * Uploads images to **Cloudinary** (Cloud Storage CDN) and stores the HTTPS image URL in MongoDB.
3. **Cookie-Based JWT Auth (`cookie-parser`)**:
   * Unlike Eventora (which sends JWT in headers), TripMate stores the JWT inside a secure `HttpOnly` browser cookie, preventing XSS attacks.

---

# 4. HOW FRONTEND & BACKEND COMMUNICATE (FULL CYCLE)

When a user performs an action (e.g., clicking "Book Event" or "Book Trip"):

```
1. User clicks "Submit" in React Component (Frontend)
                         │
                         ▼
2. Axios makes an HTTP POST Request to backend:
   axios.post('http://localhost:5000/api/bookings/book', { eventId, otp }, {
     headers: { Authorization: `Bearer ${token}` }
   })
                         │
                         ▼
3. Express Server receives request at `/api/bookings/book` (Backend)
                         │
                         ▼
4. Auth Middleware runs:
   - Extracts JWT token from header
   - Calls `jwt.verify(token, process.env.JWT_SECRET)`
   - Attaches user data (`req.user`) to request
                         │
                         ▼
5. Controller Function executes:
   - Validates OTP in MongoDB (`OTP.findOne(...)`)
   - Checks seat availability (`Event.findById(...)`)
   - Creates new document (`Booking.create(...)`)
                         │
                         ▼
6. Controller sends HTTP JSON Response back to React:
   res.status(201).json({ message: "Booking created", booking })
                         │
                         ▼
7. React state updates (`setBookings([...])`), triggering UI re-render on user's screen!
```

---

# 5. TOP 20 TECHNICAL Q&A CHEAT-SHEET FOR INTERVIEWS

### Q1: What is the role of `package.json`?
> **Answer**: `package.json` is the manifest file of a Node.js project. It lists project metadata, custom npm scripts (`npm run dev`), and dependencies (installed packages like Express, Mongoose, JWT).

### Q2: What is the purpose of `.env` file and why is it added to `.gitignore`?
> **Answer**: `.env` stores sensitive configuration variables like Database Connection URIs (`MONGO_URI`), secret keys (`JWT_SECRET`), and API keys. It is added to `.gitignore` so secrets are never pushed to public GitHub repositories.

### Q3: What is the Virtual DOM in React and why is it fast?
> **Answer**: The Virtual DOM is a lightweight in-memory copy of the real browser DOM. When component state changes, React updates the Virtual DOM first, compares it with the previous version using a "diffing algorithm", and updates ONLY the modified elements in the real DOM, avoiding expensive full-page browser re-renders.

### Q4: Explain `async/await` vs `Promises` in JavaScript.
> **Answer**: `async/await` is syntactic sugar built on top of JavaScript Promises. It allows writing asynchronous code (like database queries or API calls) in a clean, synchronous-looking style using `try/catch` blocks for error handling.

### Q5: What is CORS (Cross-Origin Resource Sharing)?
> **Answer**: CORS is a security mechanism enforced by web browsers. It blocks web pages from making API requests to a different domain/port than the one that served the web page (e.g., React on `localhost:5173` calling Express on `localhost:5000`). We resolve this by enabling the `cors()` middleware in Express.

### Q6: What is the difference between `SQL` (Relational) and `NoSQL` (MongoDB)?
> **Answer**: SQL databases store data in rigid tables with rows and columns enforced by foreign keys (great for complex joins). NoSQL databases like MongoDB store data in flexible, dynamic JSON-like documents (BSON format), allowing rapid scaling and easy schema evolution.

### Q7: What is an ORM/ODM (like Mongoose)?
> **Answer**: An ODM (Object Data Modeling) library like Mongoose provides a structured interface to interact with MongoDB from Node.js. It allows defining strict schemas, validating input data, and chaining helper methods like `.find()`, `.create()`, and `.populate()`.

### Q8: How do you handle password security in your database?
> **Answer**: Passwords should **NEVER** be stored in plain text. In both Eventora and TripMate, we hash passwords using `bcrypt` with a salt round of 10 (`bcrypt.hash(password, 10)`). Even if the database is breached, the original passwords cannot be decrypted.

### Q9: How does Cloudinary & Multer work in TripMate?
> **Answer**: **Multer** is Express middleware that parses incoming file uploads (`multipart/form-data`) from HTTP requests and holds the file buffer in memory or temporary storage. **Cloudinary** is a cloud storage service where Multer streams the file. Cloudinary returns a secure CDN image URL which is stored in MongoDB.

### Q10: How does Mapbox Geocoding work in TripMate?
> **Answer**: When a user inputs a location name (e.g., "Miami"), TripMate sends an HTTP request to Mapbox Forward Geocoding API, which returns geographic coordinates (`[longitude, latitude]`). These coordinates are stored in MongoDB as a GeoJSON Point object and rendered on the Mapbox interactive map component.

---

### 🛡️ Your Final 3 Rules for the Interview:
1. **Your Role**: *"I worked as the Backend API and Database Schema Developer."* (Protects you from deep CSS/React questions).
2. **Your Focus**: *"I specialized in REST API endpoints, JWT security, 2FA OTP validation, and MongoDB data modeling."*
3. **Your Strategy**: Drive the conversation towards **Live DSA Coding**, where your problem-solving skills will shine!
