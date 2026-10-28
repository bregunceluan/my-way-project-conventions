# Web Platform: Project Structure

## Before starting

Ask about any items not already specified:

- Project name and purpose.
- .NET version.
- Database and persistence tool (for example, EF Core or Dapper).
- Authentication requirements and approach.
- Whether to create test projects.
- Whether to include Docker support and which components it should cover.
- Frontend organization, routing, state management, UI library, and styling.

Ask additional questions when requirements are unclear. Apply the defaults below without reconfirmation; explicit project instructions take precedence.

## Stack and layout

Use .NET for the API and Vue with TypeScript, Vite, and the Composition API for the frontend. Keep `api/` and `web/` at the repository root, without a wrapping `src/` directory.

```text
api/
  ProjectName.sln
  ProjectName.Api/
    Program.cs
    Endpoints.cs
  ProjectName.Core/
    Models/
  ProjectName.Application/
    Services/
    Interfaces/
    DTOs/
    Extensions/
  ProjectName.Infra/
  tests/                         # Only when requested
    ProjectName.UnitTests/
    ProjectName.IntegrationTests/
web/
```

Replace `ProjectName` with the agreed project name. Organize backend code by type, adding folders when needed.

## Backend layers

| Project | Responsibility |
| --- | --- |
| `Core` | Domain entities in `Models/`, domain rules, and domain types. Keep independent of the other projects and infrastructure concerns. |
| `Application` | Use cases and application services, DTOs, extensions, and interfaces for persistence and external integrations, such as `IUserRepository` and `IEmailService`. Reference `Core`. |
| `Infra` | Persistence, database configuration, and concrete implementations of the interfaces in `Application`. Reference `Application` and `Core` as needed. |
| `Api` | HTTP endpoints, application configuration, and dependency injection. Reference `Application`; reference `Infra` to wire implementations. Delegate use cases to application services. |

Keep dependencies one-way. Do not put business logic or persistence implementations in endpoint handlers.

## Endpoints

Use Minimal APIs unless explicitly instructed otherwise. Centralize endpoint mappings in `Endpoints.cs`, in a static `Endpoints` class exposing a `MapEndpoints(this WebApplication app)` extension method. Register the endpoints in `Program.cs` with `app.MapEndpoints()` before `app.Run()`.

## Tests

Ask whether tests are wanted before creating test projects. If approved, create separate `ProjectName.UnitTests` and `ProjectName.IntegrationTests` projects under `api/tests/` and include them in the solution. Confirm the testing framework and relevant test scope before implementation.

## Frontend

- Create the frontend in `web/` using Vue.js with TypeScript and Vite.
- Use the Composition API for Vue components and reusable logic.
- Apply this stack by default unless the client explicitly requests otherwise.
- Ask the client about all other frontend decisions before implementing them, including folder organization, routing, state management, UI components, styling, API communication, and testing. Do not assume libraries or conventions that have not been specified.
- Ask additional questions whenever frontend requirements are unclear.

## Scope

Keep CI/CD and deployment instructions in separate Markdown files. This document defines project structure only.
