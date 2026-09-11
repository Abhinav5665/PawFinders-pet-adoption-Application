# PawFinders 🐾
### Find a pet. Give a home.

A full-stack, marketplace-style pet adoption platform where anyone can list a pet for adoption, browse listings, and submit adoption requests — modeled after the Depop/Etsy UX pattern. Browsing is public, actions require an account.

---

## ✨ Features

- **Browse & Search** — filter pets by species, breed, age, and location with server-side pagination
- **List a Pet** — any registered user can post a pet with photos, tags, and a description
- **Adoption Requests** — send a request with a personal message; track status in real time (Pending / Approved / Rejected)
- **Request-scoped Messaging** — every conversation is tied to a specific adoption request, not a generic inbox
- **Approve & Share Contact** — on approval, an automated email shares the lister's contact details with the adopter to coordinate pickup
- **Image Upload** — drag and drop up to 6 photos per listing via Cloudinary
- **JWT Authentication** — secure login with httpOnly cookies, protected routes, no role separation needed
- **Responsive UI** — marketplace-style layout that works across all screen sizes

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Backend | Node.js, Express, TypeScript |
| Database | PostgreSQL (Neon) |
| ORM | Prisma 7 (with @prisma/adapter-pg) |
| Auth | JWT (httpOnly cookies), bcrypt |
| Validation | Zod (client + server) |
| Image Upload | Multer 
| Email | Nodemailer |
| Deployment | Vercel (frontend), Render (backend), Neon (database) |

---

## 🗄️ Database Schema

```
User              — id, name, email, password, createdAt
Pet               — id, name, species, breed, age, location, description, images[], tags[], status, listedById
AdoptionRequest   — id, petId, requesterId, message, status, createdAt
Message           — id, requestId, senderId, text, createdAt
```

**Key design decision:** No roles on User — any user can both list a pet and request to adopt one. Ownership is enforced in controllers, not at the schema level.

---

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- A [Neon](https://neon.tech) account (free) — or any PostgreSQL instance
- A Gmail account or SMTP credentials — for Nodemailer email notifications

---

### 1. Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/petadoption.git
cd petadoption
```

---

### 2. Backend setup

```bash
cd backend
npm install
```

Create `backend/.env`:

```env
DATABASE_URL="your-neon-connection-string-here"
JWT_SECRET="your-super-secret-key"
CLIENT_URL="http://localhost:5173"
NODE_ENV="development"
PORT=5000


EMAIL_USER="your-gmail@gmail.com"
EMAIL_PASS="your-gmail-app-password"
```

Generate the Prisma client and run migrations:

```bash
npx prisma generate
npx prisma migrate dev --name init
```

Start the backend:

```bash
npm run dev
```

---

### 3. Frontend setup

```bash
cd ../frontend
npm install
```

Create `frontend/.env`:

```env
VITE_API_URL="http://localhost:5000"
```

Start the frontend:

```bash
npm run dev
```

| Service | URL |
|---|---|
| Frontend | http://localhost:5173 |
| Backend API | http://localhost:5000 |

---

## 📖 How It Works

```
1. Browse Listings   — find pets near you, filter by species, breed, age, location
2. Send a Request    — introduce yourself and start a conversation
3. Get Approved      — once approved, contact info is shared via email
4. Meet & Adopt      — arrange pickup and give them a forever home
```

---

## 📁 Project Structure

```
petadoption/
├── backend/
│   ├── prisma/
│   │   └── schema.prisma
│   ├── src/
│   │   ├── config/          # Prisma client singleton
│   │   ├── controllers/     # auth, pets, requests, messages
│   │   ├── middleware/       # auth (JWT verify), errorHandler, validate
│   │   ├── routes/          # authRoutes, petRoutes, requestRoutes, messageRoutes
│   │   ├── utils/           # sendEmail (Nodemailer), cloudinary config
│   │   ├── validators/      # Zod schemas
│   │   └── server.ts
│   └── .env.example
│
└── frontend/
    └── src/
        ├── components/      # Navbar, PetCard, ChatThread, StatusPill, etc.
        ├── pages/           # Home, PetDetail, ListPet, Account pages
        └── lib/             # API client, types
```

---

## 🔌 API Routes

```
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
GET    /api/auth/me

GET    /api/pets                    # browse with filters + pagination
GET    /api/pets/:id
POST   /api/pets                    # create listing (auth required)
PATCH  /api/pets/:id                # edit listing (owner only)
DELETE /api/pets/:id                # delete listing (owner only)

POST   /api/requests                # send adoption request (auth required)
GET    /api/requests/mine           # requests I sent
GET    /api/requests/received       # requests on my listings
PATCH  /api/requests/:id/approve    # approve + trigger email (owner only)
PATCH  /api/requests/:id/reject     # reject (owner only)

GET    /api/messages/:requestId     # fetch thread
POST   /api/messages/:requestId     # send message
```

---

## ⚠️ Important Notes

- Never commit your `.env` file — a `.env.example` is provided with placeholder keys
- After cloning, always run `npx prisma generate` before starting the backend — the Prisma client is gitignored and needs to be regenerated locally
- Gmail users: use an **App Password** (not your regular password) for Nodemailer — generate one at myaccount.google.com → Security → App Passwords
- Cloudinary free tier is generous enough for development — no credit card required

---

## 🔮 Planned Enhancements

- **AI pet description generator** — Claude API generates adoption descriptions from basic pet details
- **AI recommendation engine** — describe your lifestyle in plain English, get matched to real pets from the database
- **Real-time notifications** — Socket.io/websockets for live request status updates
- **Favourites / Wishlist** — save pets you're interested in
- **Lister public profile** — view all listings by a specific user

---


