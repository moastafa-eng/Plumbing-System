# 🚰 Plumbers911 – Dynamic Plumbing Business CMS

### A Dynamic Content Management System Built with ASP.NET Core 8 MVC

<p align="center">
  A scalable, database-driven business website with a powerful administration dashboard, custom authentication, and role-based access control.
</p>

<p align="center">
  ![.NET 8](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet)
  ![ASP.NET Core](https://img.shields.io/badge/ASP.NET%20Core-MVC-512BD4?logo=dotnet)
  ![SQL Server](https://img.shields.io/badge/Database-SQL%20Server-CC2927?logo=microsoftsqlserver)
  ![EF Core](https://img.shields.io/badge/ORM-Entity%20Framework%20Core-512BD4)
  ![Architecture](https://img.shields.io/badge/Architecture-N--Tier-blue)
</p>

---

## 📌 Overview

**Plumbers911** is a dynamic Content Management System (CMS) developed using ASP.NET Core 8 MVC.

The project provides a complete solution for managing a plumbing business website through a database-driven architecture and an administration dashboard.

Instead of relying on hardcoded website content, the application allows authorized administrators to dynamically manage website sections, including services, portfolio projects, team members, testimonials, categories, and other business information.

The system also implements a customized authentication and authorization workflow using ASP.NET Core Identity, including hierarchical administrative permissions, temporary administrative access, account security, and concurrency control.

Although initially designed for a plumbing business, its modular CMS architecture makes it adaptable to other business websites.

---

## ✨ Key Features

### 🌐 Dynamic Content Management System

Manage website content dynamically through the Admin Dashboard.

- Manage About Us information.
- Manage website homepage content.
- Create, update, and delete services.
- Manage portfolio projects and categories.
- Manage team members.
- Manage customer testimonials.
- Manage contact information.
- Manage social media links.

All major website sections are backed by database entities and rendered dynamically using ASP.NET Core MVC View Components.

### 🔐 Authentication & Account Management

Built on ASP.NET Core Identity with customized authentication workflows.

- User registration and login.
- Secure password hashing through ASP.NET Core Identity.
- Custom password and username validation.
- Email-based password recovery.
- Password reset using Identity tokens.
- Remember Me functionality.
- Account lockout after repeated failed login attempts.
- User profile management.
- Profile picture upload.
- Custom authentication error messages.

### 🛡️ Advanced Authorization & Role Management

The application implements a hierarchical authorization model.

**SuperAdmin**
- Access to administrative functionality.
- Manage users and administrative permissions.
- Grant temporary administrative access.
- Perform protected management operations.

**AdminObserver**
- Access the administrative dashboard.
- View and manage website content.
- Update existing content.
- Create new content.
- Cannot delete protected content.
- Cannot grant administrative permissions.

**Member**
- Access standard user functionality.
- Manage personal account information.

#### Temporary Administrative Access

The system implements a custom `AdminObserver` authorization policy using claims.

- SuperAdmins can extend a user's administrative access.
- Temporary administrative claims have an expiration date.
- The current implementation grants a five-day extension.
- Authorization checks the validity of the claim.
- Expired access is denied unless the database contains a newer valid expiration date.

This approach provides more granular administrative permissions than relying exclusively on static roles.

### ⚔️ Security & Concurrency

**Security Stamp Validation**

A custom middleware checks the authenticated user's security stamp against the database.

When a user's security stamp changes, the existing authentication cookie is invalidated and the user is redirected to the login page.

**Optimistic Concurrency Control**

Entities inherit from a shared `BaseEntity` containing a `RowVersion` property.

Entity Framework Core uses concurrency tracking to detect conflicting updates.

If multiple administrators attempt to modify the same record, conflicting changes can be detected instead of silently overwriting previously saved data.

### 🖼️ Image Management

A centralized image helper manages uploaded website images.

- Upload images for different content sections.
- Organize files into dedicated directories.
- Support JPG, JPEG, and PNG extensions.
- Delete previously stored images.
- Store image paths and content types in database entities.

### ✅ Validation & User Feedback

- FluentValidation for application input validation.
- Separate validation classes for individual features.
- Custom MVC filters.
- Generic not-found handling.
- Prevention of duplicate records for single-instance content sections.
- NToastNotify for interactive user notifications.

### 📝 Logging & Monitoring

NLog is integrated into the application for runtime logging and error diagnostics.

A centralized logging configuration is provided through `nlog.config`.

---

## 🏗️ Architecture

The application follows a layered N-Tier Architecture with separation of concerns.

```text
                  Plumbing.MVC
                 Presentation Layer
                         |
                         v
                  ServieceLayer
                  Business Logic
                         |
                         v
                  RepositoryLayer
                  Data Access Layer
                         |
                         v
                   EntityLayer
                  Domain Models
                         |
                         v
                    CoreLayer
                Shared Abstractions
```

### 1. CoreLayer

Contains shared abstractions and base entity contracts.

- BaseEntity.
- IBaseEntity.
- Shared properties and contracts.
- RowVersion concurrency property.

### 2. EntityLayer

Contains the application's data models and ViewModels.

- Website entities.
- Identity entities.
- Application ViewModels.
- Authentication ViewModels.
- Upload models.
- Enumerations.

### 3. RepositoryLayer

Responsible for database access and persistence.

- AppDbContext.
- Entity Framework Core.
- SQL Server integration.
- Entity configurations.
- Database migrations.
- Generic Repository Pattern.
- Unit of Work Pattern.

### 4. ServieceLayer

Contains business logic and application services.

- Authentication services.
- Website management services.
- AutoMapper profiles.
- FluentValidation validators.
- Custom Identity validators.
- Authorization requirements.
- Middleware.
- Image and email helpers.
- Custom MVC filters.
- Application extensions.

### 5. Plumbing.MVC

The presentation layer and application entry point.

- MVC Controllers.
- Razor Views.
- Admin Area.
- User Area.
- View Components.
- Authentication pages.
- Static assets.
- Dependency injection configuration.
- Application middleware pipeline.

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| C# | Backend development |
| .NET 8 | Application framework |
| ASP.NET Core MVC | Web application framework |
| Razor Views | Server-side rendering |
| Entity Framework Core 8 | ORM |
| SQL Server | Relational database |
| ASP.NET Core Identity | Authentication and user management |
| AutoMapper | Object mapping |
| FluentValidation | Input validation |
| NLog | Application logging |
| NToastNotify | User notifications |
| Scrutor | Dependency injection registration |
| Bootstrap | Frontend styling |
| jQuery | Frontend interactions |

---

## 📂 Project Structure

```text
Plumbing-System/
│
├── CoreLayer/
│   └── BaseEntities/
│       ├── BaseEntity.cs
│       └── IBaseEntity.cs
│
├── EntityLayer/
│   ├── Identity/
│   │   ├── Entites/
│   │   └── ViewModels/
│   │
│   ├── WebApplication/
│   │   ├── Entites/
│   │   └── ViewModels/
│   │
│   ├── Enumerates/
│   └── Models/
│
├── RepositoryLayer/
│   ├── Configurations/
│   ├── Context/
│   ├── Extensions/
│   ├── Migrations/
│   ├── Repositories/
│   │   ├── Abstract/
│   │   └── Concrete/
│   └── UnitOfWorks/
│       ├── Abstract/
│       └── Concrete/
│
├── ServieceLayer/
│   ├── AutoMapper/
│   ├── Customization/
│   ├── Exceptions/
│   ├── Extension/
│   ├── Filters/
│   ├── FluentValidation/
│   ├── Helpers/
│   ├── Middlewares/
│   ├── Requirements/
│   ├── Serviecs/
│   └── Messages/
│
└── Plumbing.MVC/
    ├── Areas/
    │   ├── Admin/
    │   │   ├── Controllers/
    │   │   └── Views/
    │   │
    │   └── User/
    │       ├── Controllers/
    │       └── Views/
    │
    ├── Components/
    ├── Controllers/
    ├── Views/
    ├── wwwroot/
    ├── Program.cs
    ├── appsettings.json
    └── nlog.config
```

---

## 🚀 Getting Started

### Prerequisites

Make sure the following tools are installed:

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [SQL Server](https://www.microsoft.com/en-us/sql-server)
- [Visual Studio 2022](https://visualstudio.microsoft.com/)
- [Entity Framework Core CLI](https://learn.microsoft.com/en-us/ef/core/cli/dotnet)

### 1. Clone the Repository

```bash
git clone https://github.com/moastafa-eng/Plumbing-System.git

cd Plumbing-System
```

### 2. Restore Dependencies

```bash
dotnet restore
```

### 3. Configure the Database

Open:

```text
Plumbing.MVC/appsettings.json
```

Update the database connection string:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=.;Database=PlumbingDB;Trusted_Connection=True;TrustServerCertificate=true;"
  }
}
```

Make sure your SQL Server instance is running and accessible.

### 4. Configure Email Settings

The application uses SMTP for password recovery emails.

Configure the following settings:

```json
{
  "EmailSettings": {
    "Email": "your-email@example.com",
    "Password": "YOUR_APP_PASSWORD",
    "Host": "smtp.gmail.com",
    "Port": 587
  }
}
```

Use a valid SMTP configuration and keep credentials outside source control in production environments.

### 5. Apply Database Migrations

Install the EF Core CLI if it is not already installed:

```bash
dotnet tool install --global dotnet-ef
```

Apply the existing migrations:

```bash
dotnet ef database update --project RepositoryLayer --startup-project Plumbing.MVC
```

This creates the database schema using the migrations included in the repository.

### 6. Run the Application

```bash
dotnet run --project Plumbing.MVC
```

Open the local URL displayed in the terminal to access the application.

---

## 🖥️ Application Areas

The application uses ASP.NET Core MVC Areas to separate administrative and user functionality.

| Area | Responsibility |
|---|---|
| Public Website | Displays dynamically managed business content |
| Admin Area | Website content and administrative management |
| User Area | User dashboard and profile management |

### Admin Dashboard Modules

- Dashboard statistics.
- About management.
- Homepage management.
- Services management.
- Categories management.
- Portfolio management.
- Team management.
- Testimonials management.
- Contact management.
- Social media management.
- User management.

---

## 🔍 Engineering Highlights

The project demonstrates practical implementation of several backend development concepts:

- N-Tier Architecture and Separation of Concerns.
- Repository Pattern and Unit of Work.
- Dependency Injection.
- Generic Repository Abstraction.
- Custom ASP.NET Core Identity.
- Claims-Based Authorization.
- Policy-Based Authorization.
- Custom Middleware.
- Optimistic Concurrency Control.
- Entity Framework Core Fluent Configurations.
- AutoMapper Profiles.
- FluentValidation.
- MVC Filters.
- View Components.
- SMTP Email Integration.
- Centralized Logging.

---

## 🔮 Potential Future Improvements

- Automated unit and integration testing.
- CI/CD pipeline integration.
- Containerized deployment using Docker.
- Enhanced image upload validation and file-size restrictions.
- Audit logging for administrative operations.
- Additional reporting and analytics.
- Improved automated database initialization and role seeding.

---

## 👨‍💻 Author

**Mostafa Engineering**

GitHub: [@moastafa-eng](https://github.com/moastafa-eng)

Project Repository: [Plumbing-System](https://github.com/moastafa-eng/Plumbing-System)

---

<p align="center">
  <strong>Built with ASP.NET Core 8 and a focus on maintainability, security, and clean separation of concerns.</strong>
</p>
