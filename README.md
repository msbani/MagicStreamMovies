# Magic Stream Movies

Magic Stream Movies is a full-stack web application providing movie streaming and recommendations, featuring user authentication, personalized experiences, and AI-enhanced review ranking. The project leverages a React frontend (Vite + React-Bootstrap UI) and a Go backend (Gin + MongoDB), with robust JWT-based authentication and OpenAI-powered review sentiment analysis.

---

## Features

- **User Authentication**: Secure registration, login, refresh, and logout options with JWT tokens and HTTP-only cookies.
- **Homepage**: Shows a list of all movies fetched from the backend.
- **Streaming**: Users can stream movies directly via embedded YouTube links.
- **Personalized Recommendations**: Authenticated users can view recommended movies based on their favorite genres.
- **Review & Ranking**: Admins can update movie reviews. Rankings are assigned automatically via OpenAI sentiment analysis.
- **Genre Selection**: Users select favorite genres during registration, which tailor recommendations.
- **Responsive UI**: Built with React-Bootstrap for seamless navigation and modern look.
- **Protected Routes**: Auth middleware restricts recommended, review, and streaming endpoints to logged-in users.

---

## Directory Structure

### Client (Frontend)
- **Main Files**:  
  - `App.jsx`, `main.jsx`, Vite-based bootstrapping  
  - React components: `Home.jsx`, `Recommended.jsx`, `Review.jsx`, `StreamMovie.jsx`, `Header.jsx`, `Register.jsx`, `Login.jsx`  
  - Context and hooks: `AuthProvider.jsx`, `useAuth.jsx`, `useAxiosPrivate.jsx`
  - Styles: `.css` files (movie, streaming, etc.)
- **Configuration**:  
  - `.env` for API URL  
  - `vite.config.js` for build options  
  - `.gitignore` for node_modules, builds, env files

### Server (Backend)
- **Controllers**:  
  - `movie_controller.go`, `user_controller.go` (CRUD, auth, reviews, recommendation logic)
- **Models**:  
  - `movie_model.go`, `user_model.go` (data structures for movies, users, genres, rankings)
- **Middleware**:  
  - `auth_middleware.go` (JWT authentication)
- **Routes**:  
  - `protected_routes.go`, `unprotected_routes.go` (division between public/private API endpoints)
- **Database**:  
  - `database_connection.go` (MongoDB connection and collection handling)
- **Utilities**:  
  - `token_util.go` (JWT and refresh tokens)
- **Configuration and Startup**:  
  - `.env` for secrets, keys, database info  
  - `main.go` entrypoint with Gin router and CORS setup  
  - `go.mod` for dependencies  
  - `.gitignore` for environment files and OS assets

---

## Setup Instructions

### Prerequisites

- **Node.js** (for frontend)
- **Go** (version >= 1.25, for backend)
- **MongoDB** (local instance recommended)

### 1. Clone the Repo
```bash
git clone https://github.com/yourusername/MagicStreamMovies.git
```
### 2. Setup Backend

- Install Go modules:
```bash
cd Server/MagicStreamMoviesServer
go mod tidy
```
- Configure environment:
  - `.env` must set `MONGODB_URI`, `DATABASE_NAME`, JWT keys, OpenAI key, allowed origins, and the review prompt template.
- Run MongoDB on your local machine.
- Start the Go server:
```bash
go run main.go
```
### 3. Setup Frontend

- Install dependencies:
```bash
cd Client/magic-stream-client
npm install
```
- Set frontend environment variable for API base URL in `.env` (default: `http://localhost:8080`).
- Start development server:
```bash
npm run dev
```
### 4. Usage

- Navigate to `http://localhost:5173` (or your configured frontend port).
- Register as a user and select favorite genres.
- Login, browse movies, and get personalized recommendations.
- Admins can update reviews; sentiment/ranking is generated using OpenAI.

---

## API Endpoints

| Route                | Method | Auth Required | Description                      |
|----------------------|--------|---------------|----------------------------------|
| `/movies`            | GET    | No            | List all movies                  |
| `/register`          | POST   | No            | Register a new user              |
| `/login`             | POST   | No            | Login and receive tokens         |
| `/logout`            | POST   | No            | Logout and clear tokens          |
| `/genres`            | GET    | No            | Get movie genre list             |
| `/recommendedmovies` | GET    | Yes           | Get recommended movies for user  |
| `/movie/:imdb_id`    | GET    | Yes           | Get movie details by IMDB ID     |
| `/addmovie`          | POST   | Yes           | Add new movie (admin only)       |
| `/updatereview/:id`  | PATCH  | Yes           | Admin updates movie review/rank  |
| `/refresh`           | POST   | No            | Refresh JWT tokens               |

---

## Technologies Used

- **Frontend**: React, Vite, React-Bootstrap, Axios, React Router
- **Backend**: Go (Gin), MongoDB, JWT, OpenAI API, godotenv, bcrypt
- **Dev Tools**: ESLint, Bootstrap, react-player
- **AI Integration**: OpenAI for review sentiment and ranking

---

## Security & Best Practices

- Secrets are managed via `.env` files, never hard-coded.
- CORS is configured per-origin, supporting credentials.
- Refresh tokens and access tokens are stored as HTTP-only cookies.
- Passwords are hashed (bcrypt) before storing in MongoDB.
- API routes are separated between protected (JWT required) and public.

---

## Environment Variables

Backend `.env` example:
```bash
DATABASE_NAME=magic-stream-movies
MONGODB_URI=mongodb://localhost:27017/
SECRET_KEY=your_secret_key
SECRET_REFRESH_KEY=your_refresh_secret_key
BASE_PROMPT_TEMPLATE=Return a response using one of these words: {rankings}.The response should be a single word and should not contain any other text. The response should be based on the following review:
OPENAI_API_KEY=your_openai_api_key
RECOMMENDED_MOVIE_LIMIT=5
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:5173,http://localhost:8080
```
Frontend `.env` example:
```bash
VITE_API_BASE_URL=http://localhost:8080
```
---

## License

This project is for educational and demo purposes. For production deployments, store your OpenAI key and other secrets securely and enforce HTTPS.

---

## Credits

Developed by msbani and contributors.  
Uses [Gin](https://gin-gonic.com/), [MongoDB Go Driver](https://www.mongodb.com/docs/drivers/go/current/), [OpenAI API](https://platform.openai.com/), and [React](https://react.dev/).
