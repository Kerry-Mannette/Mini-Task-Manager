```markdown
# Mini Task Manager Application

A lightweight web application built with ASP.NET (targeting .NET 10) that lets users create, manage, and track tasks. This repository is a minimal, production-oriented sample web app using a PostgreSQL database. It's intended as a demo for development workflows, CI/CD, and hosting on Azure.

## Table of contents

- [Overview](#overview)
- [Features](#features)
- [Quick start (development)](#quick-start-development)
- [Configuration](#configuration)
- [Database & Migrations](#database--migrations)
- [Running with Docker](#running-with-docker)
- [Testing](#testing)
- [Deployment & CI/CD](#deployment--cicd)
- [Branching model](#branching-model)
- [Contributing](#contributing)

## Overview

This project demonstrates a simple task management web application with:
- ASP.NET on .NET 10
- Entity Framework Core with the Npgsql provider for PostgreSQL
- A clean separation of code and configuration to support local development and cloud deployment
- Example CI/CD pipeline for automated builds and deployments

## Features

- Task CRUD: create, read, update, delete tasks
- Task fields: Title, Description, Due Date, Status (Pending / Completed)
- Simple responsive UI (works on desktop and mobile)
- Mark tasks complete / incomplete
- Designed to be extended with authentication (ASP.NET Core Identity) if you choose

## Quick start (development)

Prerequisites:
- .NET SDK 10.0 (`dotnet`)
- Git
- PostgreSQL (local, Docker, or Azure Database for PostgreSQL)
- Optional: Docker & Docker Compose (for running app + Postgres locally)
- Optional: `dotnet-ef` tool for migrations: `dotnet tool install --global dotnet-ef`

Clone and run locally:
```bash
git clone https://github.com/Kerry-Mannette/Mini-Task-Manager-Application.git
cd Mini-Task-Manager-Application
dotnet restore

# Configure DB connection (see Configuration section)

# Apply EF Core migrations (if any)
dotnet ef database update

dotnet run
```

Use `dotnet watch run` during development for hot reload.

## Configuration

This project reads configuration from appsettings.*.json and environment variables.

Recommended environment variables / keys:
- ConnectionStrings__Default — Npgsql-style connection string, e.g.
  "Host=localhost;Port=5432;Database=minitasks;Username=postgres;Password=yourpassword"

Environment variable names use the double-underscore syntax for hierarchical configuration when setting them in shell or hosting platforms.

In Azure App Service or other hosting providers, add the same key to application settings (do not commit secrets to source control).

## Database & Migrations

This app uses EF Core with the Npgsql provider.

Create and apply migrations:
```bash
# Add migration
dotnet ef migrations add InitialCreate

# Apply migration to database
dotnet ef database update
```

If you use Docker, ensure Postgres is running and accessible before applying migrations.

## Running with Docker

A sample docker-compose (not included in the repo by default) might look like:

```yaml
version: "3.8"
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_DB: minitasks
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: example
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  web:
    image: mcr.microsoft.com/dotnet/aspnet:8.0 # update if needed
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      ConnectionStrings__Default: "Host=db;Port=5432;Database=minitasks;Username=postgres;Password=example"
    ports:
      - "5000:80"
    depends_on:
      - db

volumes:
  pgdata:
```

After bringing up the services, run migrations against the running `db` then start the `web` service.

## Testing

If test projects are present:
```bash
dotnet test
```
Include, run, and expand tests as you add features.

## CI/CD & Deployment

- Intended CI/CD: GitHub Actions building the app and (optionally) deploying to Azure App Service on push to `main`.
- Do not store secrets in the repo. Use GitHub Secrets or your cloud provider's secret store.
- Typical pipeline steps:
  - Restore & build
  - Run tests
  - Publish artifact
  - Deploy to Azure App Service (or other host)

If you'd like, I can add a sample GitHub Actions workflow to this repo.

## Branching model

- `main` — production-ready code
- `staging` — pre-production integration
- `feature/*` — feature branches (create PRs into `staging`, then merge to `main` for releases)

Pull requests should be opened against `staging` for feature testing before `main` merges.

## Contributing

- Create a branch from `feature` or `feature/<your-feature-name>`
- Open a pull request into `staging`
- Include tests for new logic where appropriate
- Use descriptive commit messages and PR descriptions

## Troubleshooting

- Connection errors: double-check the connection string format and that Postgres is running and reachable
- EF migrations failing: ensure you are targeting the correct project in the solution when running `dotnet ef` (use `--project` and `--startup-project` if needed)
- Ports conflict: change the published ports in your Docker Compose or local launch settings
```