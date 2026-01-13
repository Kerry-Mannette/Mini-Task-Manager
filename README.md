# Mini Mini Task Manager Application

A Mini Task Manager is a lightweight web application built with ASP.NET MVC on .NET 10 that lets users create, manage, and track tasks. This repository demonstrates a minimal, production-oriented sample with database integration (EF Core + PostgreSQL), an example CI/CD deployment to Azure, and a responsive CRUD UI.

## Overview

The project implements a simple task management MVP and includes examples for deploying the web app to Azure App Service and using Azure Database for PostgreSQL.

## Getting started

### Prerequisites

- .NET SDK 10.0 (`dotnet`)
- Visual Studio 2022/2023 or Visual Studio Code (C# extension)
- PostgreSQL (local, Docker, or Azure Database for PostgreSQL)
- (Optional) EF Core CLI: `dotnet tool install --global dotnet-ef`
- Git
- (Optional) Azure CLI (`az`) and an Azure subscription

### Quick start (development)

```powershell
git clone https://github.com/Kerry-Mannette/Mini-Task-Manager-Application.git
cd Mini-Task-Manager-Application
dotnet restore
# Configure your connection string in appsettings.Development.json or user-secrets
dotnet ef database update
dotnet run
```

Use `dotnet watch run` for iterative development.

## Configuration

- Use `appsettings.Development.json`, environment variables, or `dotnet user-secrets` to supply a `ConnectionStrings:DefaultConnection` value when developing locally.
- In Azure App Service, put the production connection string into **Configuration > Application settings** (do not commit secrets to the repo).

## Features (MVP)

- Dashboard with a list of tasks
- Add / Edit / Delete tasks and mark complete
- Each task has Title, Description, Due Date, and Status
- Tasks stored with EF Core in a relational database (Postgres in production, SQLite/in-memory in development)

## Database & Migrations

This project uses Entity Framework Core. Apply migrations with:

```powershell
dotnet ef database update
```

When running in Development without a connection string, the app will use a local SQLite file database (see `appsettings.Development.json`).

## Deployment

- Host on Azure App Service and use Azure Database for PostgreSQL for production.
- Use GitHub Actions to automate build and deployment on push to `main`.

## Branching model

- `main` — production-ready
- `staging` — integration testing
- `feature` — active development

## Contributing

Create feature branches from `feature`, open PRs into `staging`, and merge to `main` for releases.

## Notable files

- `Program.cs` — app startup and EF Core configuration
- `Data/ApplicationDbContext.cs` — EF Core DbContext and model configuration
- `Models/TaskItem.cs` — Task entity

---
