# TimeZone Manager: Your Time Buddy

A full-stack web app for scheduling meetings between clients and developers in different time zones. It shows what time it is for everyone across a whole day, converts any time into every zone, and adds the meeting straight to Google Calendar.

Built with the MERN stack (MongoDB, Express, React, Node.js) and the Google Calendar API.

<!-- Add screenshots: save them in a docs/ folder and uncomment.
![Day timeline across time zones](docs/timeline.png)
![Google Calendar event form](docs/calendar.png)
-->

## Features

**Day timeline**
- Pick a date and a reference time zone
- See a 24-hour bar for every saved time zone, lined up hour by hour, including where each zone moves into the previous or next day
- Click an hour to highlight the same moment in every zone, so you can find a slot that works for everyone

**Time converter**
- Enter a date and time in any zone and see the matching local date and time in every other zone

**Google Calendar integration**
- Sign in with Google (OAuth) from inside the app
- Check your calendar for existing events in a time range before booking
- Create an event with a title, location, description, start and end time; the app checks that the end is after the start

**Accounts and admin**
- Register and log in, with JWT authentication stored in a cookie and passwords hashed using bcrypt
- Admins add, update and delete time zones (name and UTC offset) and manage users
- Protected pages for admin and user screens

## Tech stack

| Part | Built with |
|---|---|
| Frontend | React, Vite, React Router, Axios, Material UI, React Bootstrap, Font Awesome |
| Backend | Node.js, Express, Mongoose, JSON Web Tokens, bcryptjs, cookie-parser |
| Database | MongoDB |
| External APIs | Google Calendar API, Google Identity Services (OAuth) |

## Project structure

```
src/
├── backend/
│   ├── controllers/   auth, time zones, users
│   ├── models/        TimeZone, User
│   ├── routes/
│   ├── utils/         error helper, token verification
│   ├── db/            MongoDB connection
│   └── index.js       Express app
└── frontend/
    └── src/
        ├── components/   Bar, Barlist (timeline), Search, GoogleCalendar, NavBar
        ├── pages/        Home, Custom_time, EventPage, Admin, User, Login, Register,
        │                 AddZone, UpdateZone, DeleteZone, UpdateUser
        ├── context/      AuthContext
        └── hooks/api.js  Axios instance
```

## API overview

| Method | Endpoint | Access |
|---|---|---|
| POST | `/auth/register`, `/auth/login` | Public |
| GET | `/timezone` | Public |
| POST | `/timezone` | Admin |
| PUT, DELETE | `/timezone/:id` | Admin |
| GET | `/user` | Admin |
| GET, PUT, DELETE | `/user/:id` | That user or an admin |

## Run it locally

Requires Node.js, a MongoDB database, and a Google Cloud project with the Calendar API enabled.

### 1. Backend

```bash
cd src/backend
npm install
```

Create `src/backend/.env`:

```
Mongo_URL=<your MongoDB connection string, without a database name>
JWT=<a long random secret>
PORT=3001
```

```bash
npm start
```

The API runs on `http://localhost:3001`.

### 2. Frontend

```bash
cd src/frontend
npm install
npm run dev
```

Opens on `http://localhost:5173`.

### 3. Google Calendar

1. In Google Cloud Console, enable the **Google Calendar API**.
2. Create an **OAuth client ID** (web application) and add `http://localhost:5173` as an authorized JavaScript origin.
3. Create an **API key** and restrict it to the Calendar API.
4. Put both values in `src/frontend/src/components/GoogleCalendar/GoogleCalendar.jsx`.

### 4. Make an admin

Register a user, then set `isAdmin: true` on it in the database to unlock the admin pages.

## What I'd improve next

- Ignore the `isAdmin` field on sign-up, so admin access can only be granted from the database
- Use IANA time zones (such as `Asia/Kolkata`) instead of fixed offsets, so daylight saving time is handled automatically
- Move the Google keys into environment variables
- Invite the other meeting attendees from the app when creating the Calendar event
- Deploy the frontend and API
