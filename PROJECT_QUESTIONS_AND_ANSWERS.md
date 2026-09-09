# 🎯 Ultimate Project Question Bank & Interview Survival Handbook
> **Target Roles**: Infosys Specialist Programmer (SP ~9.5 LPA) & Digital Specialist Engineer (DSE ~6.25 LPA)  
> **Workspace Location**: `c:\EVENT MANAGEMENT SYSTEM\Eventora-MERN\PROJECT_QUESTIONS_AND_ANSWERS.md`

---

## 📑 TABLE OF CONTENTS
1. **EMERGENCY STRATEGIES: What to do if you don't know the answer**
   - The 4 Golden Diplomatic Scripts (How to pivot without losing marks)
   - How to handle unknown technical concepts gracefully
2. **PART 1: EVENTORA (MERN) — 25 EXHAUSTIVE INTERVIEW QUESTIONS & MODEL ANSWERS**
   - Category A: High-Level & Overview (Q1–Q5)
   - Category B: Authentication, Security & 2FA OTP (Q6–Q10)
   - Category C: Database, Schema & Concurrency (Q11–Q15)
   - Category D: Backend, REST APIs & Middleware (Q16–Q20)
   - Category E: Scalability, Testing & Real-World Edge Cases (Q21–Q25)
3. **PART 2: TRIPMATE — 15 EXHAUSTIVE INTERVIEW QUESTIONS & MODEL ANSWERS**
   - Category A: Overview & Unique Tech Stack (Q26–Q30)
   - Category B: Mapbox Geocoding, Cloudinary & Multer File Uploads (Q31–Q35)
   - Category C: Expense Splitting & Cookie-Based Security (Q36–Q40)
4. **PART 3: CORE WEB DEV QUICK-FIRE QUESTION BANK (Q41–Q50)**

---

# 🚨 EMERGENCY STRATEGIES: WHAT IF YOU DON'T KNOW THE ANSWER?

If the interviewer asks a technical question about a library, tool, or syntax you don't know, **DO NOT PANIC, GUESS, OR MAKE UP FAKE TERMS**. Interviewers immediately spot bluffing.

Use these **4 Diplomatic Master Scripts** to maintain full confidence and high marks:

### 🎭 Script 1: When asked about a specific syntax or library feature you forgot
> **What to say**:  
> *"I haven't memorized the exact library syntax for that specific function off the top of my head, but logically the workflow involves [explain the high-level logic]. In actual development, I consult the official documentation for exact syntax while implementing."*  
> **Why it works**: Shows maturity and practical developer mindset.

### 🎭 Script 2: When asked about an advanced concept you haven't implemented (e.g., Redis, WebSockets)
> **What to say**:  
> *"In our current implementation of Eventora, we handled this using HTTP REST endpoints and MongoDB queries. However, for a high-concurrency production environment, using [e.g., Redis caching / WebSockets / Message Queues] would be the ideal architectural improvement to scale it."*  
> **Why it works**: Converts a limitation into system design knowledge!

### 🎭 Script 3: When asked about Frontend CSS/Framework details you don't know
> **What to say**:  
> *"In our team division of labor, my primary focus was **Backend API development, JWT Authentication, and Database Schema design**. My teammate handled the frontend styling. However, I understand how the backend APIs interface with React state using Axios."*  
> **Why it works**: Completely shields you from frontend questions and pulls the interviewer back to your strong area (Backend/DB)!

### 🎭 Script 4: When you are 100% stuck and don't know the concept at all
> **What to say**:  
> *"I haven't had the opportunity to work with that specific concept yet in my projects, but I am familiar with core Data Structures, Algorithms, and REST API design. I would love to learn how it applies here."*  
> **Why it works**: Shows humility, enthusiasm to learn, and pivots straight to your **DSA strength**!

---

# 📌 PART 1: EVENTORA (MERN) — 25 EXHAUSTIVE QUESTIONS & MODEL ANSWERS

### 1. High-Level & Overview

#### Q1: "Give me a high-level summary of your project Eventora."
* **Model Answer**:
  > *"Eventora is a full-stack MERN (MongoDB, Express, React, Node.js) event management and ticket booking platform. It features a dual-user system: users can search events, register with 2FA email OTP verification, and request ticket bookings; administrators can create events, confirm/cancel pending booking requests, mark payment statuses, and track live platform revenue via an analytics dashboard."*

#### Q2: "What problem does Eventora solve?"
* **Model Answer**:
  > *"Eventora solves ticket overbooking and unverified ticket spamming. Traditional event systems allow bots or fake accounts to reserve seats. Eventora introduces mandatory 2FA Email OTP verification during signup and booking, alongside an Admin Verification Queue to ensure seat allocation is legitimate before confirming bookings."*

#### Q3: "What was your exact role in developing Eventora?"
* **Model Answer**:
  > *"I worked as the **Backend API & Database Engineer**. My main responsibilities were writing Express.js controller logic, defining Mongoose data schemas (`User`, `Event`, `Booking`, `OTP`), implementing JWT authentication middleware, and setting up Nodemailer for 2FA OTP emails."*

#### Q4: "Why did you choose Vite over Create React App (CRA) for the frontend?"
* **Model Answer**:
  > *"Vite utilizes native ES Modules (ESM) in modern browsers and ESBuild under the hood. Unlike CRA (which bundles the entire application using Webpack before serving), Vite provides near-instantaneous server startup and ultra-fast Hot Module Replacement (HMR) during development."*

#### Q5: "What npm packages did you install in the backend and why?"
* **Model Answer**:
  > * `express`: Web server framework for handling REST routes.
  > * `mongoose`: ODM library for MongoDB object modeling and validation.
  > * `jsonwebtoken`: Generating and verifying JWT tokens for stateless auth.
  > * `bcryptjs`: Password hashing algorithm.
  > * `nodemailer`: Transporting transactional email messages (2FA OTPs).
  > * `dotenv`: Loading environment configuration variables from `.env`.

---

### 2. Authentication, Security & 2FA OTP

#### Q6: "How does user registration work end-to-end in Eventora?"
* **Model Answer**:
  > *"When a user signs up, the backend checks if the email already exists. If new, it hashes the password with bcrypt (`saltRounds = 10`) and saves the user with `isVerified: false`. It then generates a random 6-digit OTP string, stores it in MongoDB with an expiration index, and emails it to the user via Nodemailer. Once the user submits the OTP on the frontend, `exports.verifyOTP` sets `isVerified = true` and returns a JWT token."*

#### Q7: "How does JWT Authentication work on protected routes?"
* **Model Answer**:
  > *"When a user logs in, the backend signs a token containing `{ id: user._id, role: user.role }` using `process.env.JWT_SECRET`. The React frontend stores this token and includes it in headers (`Authorization: Bearer <token>`). The backend `authMiddleware` intercepts the request, decodes the token via `jwt.verify()`, and attaches `req.user` to the request object before passing control to the controller."*

#### Q8: "What happens if a user tries to access `/api/events` creation route without being an admin?"
* **Model Answer**:
  > *"The request passes through `adminMiddleware`. It inspects `req.user.role`. If `req.user.role !== 'admin'`, it immediately halts execution and returns an HTTP `403 Forbidden` JSON response (`{ message: 'Access denied. Admin only.' }`), preventing unauthorized database mutations."*

#### Q9: "How do you ensure OTPs expire automatically and cannot be reused?"
* **Model Answer**:
  > *"In the Mongoose `OTP` schema, we define a Time-To-Live (TTL) index: `createdAt: { type: Date, default: Date.now, expires: 600 }`. MongoDB automatically deletes the OTP document after 600 seconds (10 minutes). Additionally, once a user verifies their OTP, `exports.verifyOTP` executes `await OTP.deleteOne({ _id: validOTP._id })` to delete it immediately, preventing replay attacks."*

#### Q10: "Why is storing JWT in `localStorage` vulnerable, and how can it be made more secure?"
* **Model Answer**:
  > *"Tokens stored in `localStorage` can be read by any JavaScript executing on the page, making them vulnerable to Cross-Site Scripting (XSS) attacks. A more secure approach is storing JWTs in `HttpOnly`, `Secure`, `SameSite=Strict` cookies, which browser JavaScript cannot read."*

---

### 3. Database, Schema & Concurrency

#### Q11: "Explain your MongoDB Database Schema design."
* **Model Answer**:
  > *"We have 4 core collections:*
  > 1. `users`: Stores user credentials, role (`user`/`admin`), and `isVerified` status.
  > 2. `events`: Stores event details, total seats, available seats, and `createdBy` (`User` `ObjectId`).
  > 3. `bookings`: Stores references (`userId`, `eventId`), booking status (`pending`/`confirmed`/`cancelled`), and payment status.
  > 4. `otps`: Stores email, hashed 6-digit OTP code, action type, and TTL expiration index."*

#### Q12: "How does Mongoose `.populate()` work? Is it as fast as SQL JOIN?"
* **Model Answer**:
  > *"Mongoose `.populate('eventId')` replaces the `ObjectId` in a booking document with the actual `Event` document. It is **not** a DB-level SQL JOIN. Under the hood, Mongoose executes two separate MongoDB queries (`find()` on bookings, then `$in` query on events) and merges them in Node.js server memory. It is fast for small to medium apps, though for massive datasets, MongoDB `$lookup` aggregation pipeline is preferred."*

#### Q13: "How do you prevent seat overbooking in high concurrency?"
* **Model Answer**:
  > *"We use MongoDB Atomic Updates using `findOneAndUpdate` with query condition `{ availableSeats: { $gt: 0 } }` and operator `{ $inc: { availableSeats: -1 } }`. Because MongoDB executes document-level updates atomically, it guarantees `availableSeats` never drops below 0 even under simultaneous requests."*

#### Q14: "What happens to available seats when an admin confirms or cancels a booking?"
* **Model Answer**:
  > *"When an admin confirms a pending booking, `status` changes to `confirmed` and `availableSeats` is decremented by 1 (`availableSeats -= 1`). If a confirmed booking is cancelled, the system checks `if (wasConfirmed)` and increments `availableSeats += 1` to restore seat inventory."*

#### Q15: "Why did you use `seed.js` in your project?"
* **Model Answer**:
  > *"`seed.js` is a developer utility script. It connects to MongoDB, clears old collections, and populates the database with initial mock events, user accounts, and admin profiles (`admin@eventora.com`) so developers can test application flows without manually creating data every time."*

---

### 4. Backend, REST APIs & Middleware

#### Q16: "What is Express Middleware? Give an example from your project."
* **Model Answer**:
  > *"Express Middleware is a function with access to `(req, res, next)`. It can execute code, modify request objects, or terminate request cycles. In Eventora, `authMiddleware` checks for `req.headers.authorization`, verifies the JWT token, and calls `next()` to proceed to the controller."*

#### Q17: "How do you handle errors in Express controller functions?"
* **Model Answer**:
  > *"Every controller function is wrapped in a `try/catch` block. Database or runtime errors are caught in the `catch` block and returned as JSON responses with HTTP `500 Internal Server Error` and error message details, preventing Node.js server crashes."*

#### Q18: "What is CORS and how did you configure it in Eventora?"
* **Model Answer**:
  > *"CORS (Cross-Origin Resource Sharing) is a browser security policy that restricts web pages from requesting a domain different from their own. Since our React frontend ran on `http://localhost:5173` and Express backend on `http://localhost:5000`, we enabled CORS in Express using `app.use(cors())`."*

#### Q19: "How does Nodemailer send emails in your project?"
* **Model Answer**:
  > *"Nodemailer configures an SMTP transporter ([server/utils/email.js](file:///c:/EVENT%20MANAGEMENT%20SYSTEM/Eventora-MERN/server/utils/email.js)) using Gmail SMTP service and Google App Password (`EMAIL_USER`, `EMAIL_PASS`). The helper function `sendOTPEmail(email, otp)` constructs HTML email templates and dispatches them asynchronously."*

#### Q20: "What is the entry point of your Node server?"
* **Model Answer**:
  > *"`server.js` is the entry point. It loads environment variables with `dotenv.config()`, connects to MongoDB via Mongoose, mounts CORS and `express.json()` middlewares, registers route handlers (`/api/auth`, `/api/events`, `/api/bookings`), and starts the HTTP server listening on PORT 5000."*

---

### 5. Scalability, Testing & Real-World Edge Cases

#### Q21: "If 1,000,000 users visit Eventora at once, how would you scale the system?"
* **Model Answer**:
  > 1. **Caching**: Place a **Redis** cache in front of MongoDB to cache popular event details.
  > 2. **Asynchronous Queues**: Offload Nodemailer email sending to a background job queue (BullMQ / RabbitMQ).
  > 3. **Load Balancing**: Deploy multiple Node.js server instances behind an **Nginx Load Balancer**.
  > 4. **Database Read Replicas**: Use MongoDB Atlas Replica Sets to split read and write queries.

#### Q22: "How would you handle real-time seating updates without refreshing the page?"
* **Model Answer**:
  > *"Currently, React re-fetches data via Axios. To enable real-time live updates, we would integrate **Socket.io (WebSockets)**. Whenever an admin confirms a booking, the server emits an `availableSeatsUpdated` event to all connected React clients."*

#### Q23: "What is the difference between `npm run dev` and `npm start` in your project?"
* **Model Answer**:
  > *"`npm run dev` uses `concurrently` and `nodemon` to start both Express server and Vite frontend in development mode with hot-reloading. `npm start` is used in production to serve built static assets."*

#### Q24: "How did you test your REST APIs during development?"
* **Model Answer**:
  > *"We used **Postman** to test all API endpoints. We saved sample JSON requests in `Eventora_Postman_Collection.json`, testing registration, OTP verification, JWT authorization headers, and admin actions."*

#### Q25: "If you had 2 more weeks to work on Eventora, what features would you add?"
* **Model Answer**:
  > *"I would implement real payment gateway integration using Stripe or Razorpay, interactive seat selection maps (like selecting specific seat numbers), and WebSockets for real-time seat availability notifications."*

---

# 📌 PART 2: TRIPMATE — 15 EXHAUSTIVE QUESTIONS & MODEL ANSWERS

### 1. Overview & Unique Tech Stack

#### Q26: "Explain the main idea behind TripMate."
* **Model Answer**:

  > *"TripMate is a travel destination listing, itinerary planning, and booking web application. Users can search travel listings, view geographic locations on interactive maps, upload destination images, book stays, and manage travel budgets."*

#### Q27: "What is the main difference between Eventora and TripMate?"
* **Model Answer**:
  > *"Eventora focuses on event ticket booking, 2FA email security, and admin approval queues. TripMate focuses on travel listings, **Mapbox geospatial 3D maps**, **Cloudinary/Multer image uploads**, and **Cookie-based JWT authentication**."*

#### Q28: "What tech stack was used for TripMate?"
* **Model Answer**:
  > *"Frontend: React 19, Vite, Mapbox GL (`react-map-gl`), Framer Motion (animations), Axios, React Router v7.  
  > Backend: Node.js, Express.js, MongoDB (Mongoose), Cloudinary SDK, Multer middleware, Cookie-Parser, JsonWebToken."*

---

### 2. Mapbox, Cloudinary & Multer

#### Q29: "How does Mapbox Geocoding work in TripMate?"
* **Model Answer**:
  > *"When a user creates a travel listing with an address like 'Paris, France', the backend sends a request to Mapbox Forward Geocoding API. Mapbox returns longitude and latitude coordinates (`[lng, lat]`). We store these coordinates in MongoDB as a GeoJSON Point object and pass them to `<Map>` components in React."*

#### Q30: "How do image uploads work with Multer and Cloudinary in TripMate?"
* **Model Answer**:
  > *"When a user submits a trip photo, the form uses `multipart/form-data`. **Multer** middleware intercepts the incoming request buffer on the Express server. The backend streams this buffer to **Cloudinary** cloud storage API. Cloudinary responds with a secure HTTPS URL, which we save in the MongoDB `listing` document."*

#### Q31: "Why use Cloudinary instead of storing images directly in MongoDB or local disk?"
* **Model Answer**:
  > *"MongoDB documents have a 16MB size limit, so storing raw binary image data (Base64) bloats the database and drastically slows down queries. Storing on local disk fails when deploying to serverless platforms like Heroku/Vercel. Cloudinary acts as a global CDN, compressing images and serving them fast."*

---

### 3. Expense Splitting & Cookie Security

#### Q32: "How does Cookie-Based Authentication work in TripMate?"
* **Model Answer**:
  > *"In TripMate, after verifying credentials, the server sets the JWT inside an HTTP response cookie using `res.cookie('token', token, { httpOnly: true, secure: true })`. `cookie-parser` middleware automatically parses incoming cookies on subsequent requests, allowing `authMiddleware` to authenticate the user securely without manual header management."*

#### Q33: "What is Framer Motion used for in TripMate?"
* **Model Answer**:
  > *"Framer Motion (`framer-motion`) is a React animation library used to add smooth page transition animations, interactive modal popups, and card hover effects to create a premium UI experience."*

#### Q34: "Explain the MongoDB Listing Schema in TripMate."
* **Model Answer**:
  > *"The `listing.model.js` schema contains: `title` (String), `description` (String), `image` (URL string), `price` (Number), `location` (String), `country` (String), `geometry` (GeoJSON Point: `{ type: 'Point', coordinates: [lng, lat] }`), and `owner` (`ObjectId` -> `User`)."*

#### Q35: "How does React Router v7 handle page navigation in TripMate?"
* **Model Answer**:
  > *"React Router v7 uses `<BrowserRouter>` and `<Routes>` to map browser URL paths (`/`, `/listings/:id`, `/create-listing`) to specific React page components without full page refreshes, maintaining client-side state."*

---

# 📌 PART 3: CORE WEB DEV QUICK-FIRE QUESTION BANK

#### Q36: What is the difference between `useState` and `useRef` in React?
> **Answer**: Updating `useState` triggers a component **re-render** to update UI. Updating `useRef` persists values across renders **without** causing a re-render (ideal for DOM element references or timers).

#### Q37: What is Props Drilling and how do you avoid it?
> **Answer**: Props Drilling occurs when data is passed down through multiple nested component layers that don't need it. It is avoided using **React Context API** (`useContext`) or state management libraries (Redux/Zustand).

#### Q38: What is the difference between `GET` and `POST` HTTP requests?
> **Answer**: `GET` requests append data in the URL query string, are cached by browsers, and should only retrieve data. `POST` requests send data inside the HTTP request body, are not cached, and are used for mutations (creating resources).

#### Q39: What is `process.env` in Node.js?
> **Answer**: `process.env` is a global object in Node.js that injects environment variables defined in system environment or `.env` files using `dotenv`.

#### Q40: What is the difference between `==` and `===` in JavaScript?
> **Answer**: `==` performs loose equality with type coercion (e.g., `'5' == 5` is `true`). `===` performs strict equality checking both value and data type (e.g., `'5' === 5` is `false`).

---

### 🏆 Final Reminder for Your Infosys Interview:
1. **Drive to DSA**: Use your introduction to highlight your LeetCode problem-solving skills!
2. **Claim Backend Role**: State you built Backend REST APIs and MongoDB Schemas.
3. **Use Diplomatic Scripts**: Never guess—use the 4 diplomatic scripts when facing unknown questions!
