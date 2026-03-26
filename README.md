# Bloom - Dating App

A full-stack dating application with real-time messaging, user matching, photo management, and an admin panel. Built with Angular 19 and ASP.NET Core 9.

## Features

- **User Profiles** — Register, login, and manage detailed profiles with photos, interests, location, and bio
- **Photo Management** — Upload multiple photos via Cloudinary with automatic cropping, set a main photo, and delete photos
- **Likes & Matching** — Like/unlike other users and browse who has liked you
- **Real-Time Messaging** — Instant messaging powered by SignalR with read receipts and unread counts
- **Online Presence** — See who's currently online with real-time status updates and toast notifications
- **Member Filtering** — Filter members by age range, gender, and sort by last active or newest
- **Pagination** — Server-side pagination across members, messages, and likes
- **Admin Panel** — Manage user roles and moderate photos (approve/reject) with role-based access
- **Role-Based Authorization** — Admin and Moderator roles with route guards and policy-based access
- **Responsive UI** — Bootstrap 5 with the Bootswatch Lux theme

## Tech Stack

### Backend

| Technology | Purpose |
|---|---|
| ASP.NET Core 9 | Web API framework |
| Entity Framework Core 9 | ORM / Data access |
| SQL Server | Production database |
| SQLite | Development database |
| ASP.NET Core Identity | Authentication & role management |
| JWT Bearer Tokens | Stateless authentication |
| SignalR | Real-time WebSocket communication |
| Cloudinary | Cloud-based image storage |
| AutoMapper | Object-to-object mapping |

### Frontend

| Technology | Purpose |
|---|---|
| Angular 19 | SPA framework |
| TypeScript 5.7 | Language |
| Bootstrap 5 / Bootswatch | UI styling |
| Font Awesome 4.7 | Icons |
| @microsoft/signalr | Real-time client |
| ng-gallery / ngx-gallery | Image galleries |
| ng2-file-upload | File uploads |
| ngx-toastr | Toast notifications |
| ngx-spinner | Loading indicators |
| ngx-bootstrap | UI components (dropdowns, modals, tabs) |

## Prerequisites

- [.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)
- [Node.js 22+](https://nodejs.org/)
- [Angular CLI](https://angular.dev/tools/cli) (`npm install -g @angular/cli`)
- SQL Server (or use SQLite for development)
- A [Cloudinary](https://cloudinary.com/) account (free tier works)

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/DatingApp.git
cd DatingApp
```

### 2. Configure the Backend

Update `API/appsettings.Development.json` with your database connection string:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost,1433;Database=DatingDB;User Id=SA;Password=YourPassword;TrustServerCertificate=True"
  }
}
```

Update `API/appsettings.json` with your Cloudinary credentials:

```json
{
  "CloudinarySettings": {
    "CloudName": "your-cloud-name",
    "ApiKey": "your-api-key",
    "ApiSecret": "your-api-secret"
  }
}
```

### 3. Run the Backend

```bash
cd API
dotnet restore
dotnet ef database update
dotnet run
```

The API will be available at `https://localhost:7111`.

### 4. Run the Frontend

```bash
cd client
npm install
ng serve
```

The client will be available at `https://localhost:4200`.

### 5. Seed Data

On first run, the application automatically seeds sample users from `API/Data/UserSeedData.json` and creates the default roles (Admin, Moderator).

## Project Structure

```
DatingApp/
├── API/                          # ASP.NET Core Web API
│   ├── Controllers/              # API endpoints
│   │   ├── AccountController     #   Registration & login
│   │   ├── UsersController       #   User profiles & photos
│   │   ├── MessagesController    #   Messaging
│   │   ├── LikesController       #   Like/unlike users
│   │   └── AdminController       #   User & photo management
│   ├── Data/                     # EF Core DbContext, repositories, migrations, seed data
│   ├── DTOs/                     # Data transfer objects
│   ├── Entities/                 # Domain models (AppUser, Photo, Message, UserLike, etc.)
│   ├── Services/                 # Business logic (Token generation, Photo uploads)
│   ├── Interfaces/               # Repository & service contracts
│   ├── SignalR/                  # Real-time hubs (Presence, Messaging)
│   ├── Extensions/               # Service registration & helper extensions
│   ├── Helpers/                  # AutoMapper profiles, pagination, Cloudinary settings
│   ├── Middleware/               # Global exception handling
│   └── Errors/                   # Error response models
│
├── client/                       # Angular 19 SPA
│   └── src/app/
│       ├── admin/                # Admin panel, photo moderation, user management
│       ├── members/              # Member list, detail, edit, cards, photo editor
│       ├── messages/             # Messaging UI
│       ├── lists/                # Likes list
│       ├── home/                 # Landing page
│       ├── register/             # Registration form
│       ├── nav/                  # Navigation bar
│       ├── _services/            # Angular services (account, members, messages, presence, likes)
│       ├── _models/              # TypeScript interfaces
│       ├── _guards/              # Route guards (auth, admin, unsaved changes)
│       ├── _interceptors/        # HTTP interceptors (JWT, errors, loading)
│       ├── _directives/          # Custom directives (hasRole)
│       ├── _forms/               # Reusable form components
│       ├── _resolvers/           # Route resolvers
│       └── errors/               # Error pages (404, 500)
│
└── .github/workflows/            # CI/CD (Azure deployment)
```

## API Endpoints

### Account
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/account/register` | Register a new user |
| POST | `/api/account/login` | Login and receive JWT token |

### Users
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/users` | Get paginated members (with filters) |
| GET | `/api/users/{username}` | Get a member's profile |
| PUT | `/api/users` | Update your profile |
| POST | `/api/users/add-photo` | Upload a photo |
| PUT | `/api/users/set-main-photo/{photoId}` | Set main profile photo |
| DELETE | `/api/users/delete-photo/{photoId}` | Delete a photo |

### Messages
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/messages` | Send a message |
| GET | `/api/messages` | Get messages (inbox/outbox/unread) |
| GET | `/api/messages/thread/{username}` | Get message thread with a user |
| GET | `/api/messages/unread-count` | Get unread message count |
| DELETE | `/api/messages/{id}` | Delete a message |

### Likes
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/likes/{targetUserId}` | Toggle like on a user |
| GET | `/api/likes/list` | Get IDs of users you've liked |
| GET | `/api/likes` | Get paginated liked users |

### Admin
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/admin/users-with-roles` | Get all users with roles |
| POST | `/api/admin/edit-roles/{username}` | Edit a user's roles |
| GET | `/api/admin/photos-to-moderate` | Get unapproved photos |
| POST | `/api/admin/approve-photo/{id}` | Approve a photo |
| POST | `/api/admin/reject-photo/{id}` | Reject a photo |

### SignalR Hubs
| Hub | Endpoint | Purpose |
|---|---|---|
| Presence | `/hubs/presence` | Online/offline status tracking |
| Message | `/hubs/message` | Real-time messaging |

## Architecture

- **Pattern**: Repository + Unit of Work
- **Backend**: N-tier (Controllers → Services/Repositories → EF Core DbContext)
- **Frontend**: Component-based with Angular Signals for reactive state
- **Authentication**: JWT tokens stored in localStorage, attached via HTTP interceptor
- **Real-Time**: SignalR hubs for presence tracking and instant messaging
- **Image Pipeline**: Client upload → API → Cloudinary (500x500, face-cropped) → URL stored in DB

## Deployment

The project includes a GitHub Actions workflow for CI/CD deployment to **Azure App Service**.

The workflow (`.github/workflows/main_bloom-dating-app.yml`):
1. Builds the Angular client into `API/wwwroot`
2. Builds and publishes the .NET API
3. Deploys to Azure Web Apps

## Usage

1. **Register** an account with your details (username, password, gender, date of birth, city, country)
2. **Browse members** using filters for age, gender, and sorting preferences
3. **Like** members you're interested in and check who has liked you
4. **Message** other members in real-time — see when they're online and when messages are read
5. **Edit your profile** — update your bio, interests, and photos
6. **Admin panel** (if assigned Admin/Moderator role) — manage user roles and moderate photos

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
