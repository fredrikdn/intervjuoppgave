# Flight Planner Backend (Java)

A Spring Boot backend solution template for the Flight Planner application.

## Prerequisites

- Java 21+
- Maven 3.8+
- Docker and Docker Compose (for PostgreSQL)

## Quick Start

### 1. Start the database

```bash
docker-compose up -d
```

This starts a PostgreSQL 16 container with:
- Database: `flightplanner`
- User: `flightplanner`
- Password: `localdev123`
- Port: `5432`

The database schema and seed data are automatically applied from `db/init/`.

### 2. Run the application

```bash
mvn spring-boot:run -DskipTests
```

Or build and run:

```bash
mvn clean package
java -jar target/backend-1.0.0-SNAPSHOT.jar
```

The server runs on `http://localhost:3001` by default.

## Available Commands

| Command | Description |
|---------|-------------|
| `mvn spring-boot:run` | Run in development mode |
| `mvn clean package` | Build JAR file |
| `mvn test` | Run tests |
| `mvn clean package -DskipTests` | Build without tests |
| `docker-compose up -d` | Start PostgreSQL |
| `docker-compose down` | Stop PostgreSQL |
| `docker-compose down -v` | Stop and remove data |

## Project Structure

```
Backend/
├── db/
│   └── init/                          # SQL files run on database init
│       ├── 001_schema.sql             # Database schema
│       └── 002_seed.sql               # Sample data
├── src/
│   ├── main/
│   │   ├── java/com/flightplanner/
│   │   │   ├── config/                # Configuration classes
│   │   │   ├── controller/            # REST controllers (TODO)
│   │   │   ├── dto/                   # Data Transfer Objects
│   │   │   ├── entity/                # JPA entities
│   │   │   ├── exception/             # Exception handling
│   │   │   ├── repository/            # JPA repositories
│   │   │   └── Application.java       # Main class
│   │   └── resources/
│   │       └── application.yml        # Configuration
│   └── test/
│       └── java/com/flightplanner/    # Test classes
├── docker-compose.yml                 # PostgreSQL container
├── pom.xml                            # Maven configuration
└── README.md
```

## Database Schema

### Tables

- **employees** - Employee records
- **itineraries** - Travel itineraries linked to employees
- **flight_segments** - Individual flight segments within itineraries
- **groups** - Travel groups
- **group_members** - Junction table for group membership
- **group_itineraries** - Junction table for group itineraries

### Entity Relationships

```
employees 1──∞ itineraries 1──∞ flight_segments
    │
    ∞
    │
group_members ∞──1 groups 1──∞ group_itineraries
```

## API Endpoints to Implement

### Employees
- `GET /api/employees` - List all employees
- `GET /api/employees/{id}` - Get employee by ID
- `POST /api/employees` - Create employee
- `PATCH /api/employees/{id}` - Update employee
- `DELETE /api/employees/{id}` - Delete employee

### Itineraries
- `GET /api/itineraries` - List itineraries (supports `?employeeId=` and `?status=` filters)
- `GET /api/itineraries/{id}` - Get itinerary by ID
- `POST /api/itineraries` - Create itinerary with segments
- `PATCH /api/itineraries/{id}` - Update itinerary
- `DELETE /api/itineraries/{id}` - Delete itinerary

### Groups
- `GET /api/groups` - List all groups
- `GET /api/groups/{id}` - Get group by ID
- `POST /api/groups` - Create group
- `PATCH /api/groups/{id}` - Update group
- `DELETE /api/groups/{id}` - Delete group

## Configuration

Environment variables (or application.yml):

| Variable | Description | Default |
|----------|-------------|---------|
| `SERVER_PORT` | Server port | `3001` |
| `POSTGRES_HOST` | Database host | `localhost` |
| `POSTGRES_PORT` | Database port | `5432` |
| `POSTGRES_DB` | Database name | `flightplanner` |
| `POSTGRES_USER` | Database user | `flightplanner` |
| `POSTGRES_PASSWORD` | Database password | `localdev123` |
| `CORS_ORIGINS` | Allowed CORS origins | `http://localhost:5173` |
| `API_KEY` | API authentication key | `dev-key` |

## What's Provided

- ✅ JPA Entities (`Employee`, `Itinerary`, `FlightSegment`, `Group`)
- ✅ JPA Repositories with basic queries
- ✅ Error handling with consistent response format
- ✅ CORS configuration
- ✅ Health check endpoint (`/health`)
- ✅ Database schema and seed data

## What to Implement

- ⬜ Complete `EmployeeController` with CRUD operations
- ⬜ Complete `ItineraryController` with CRUD operations
- ⬜ Complete `GroupController` with CRUD operations
- ⬜ DTOs for request/response mapping
- ⬜ Service layer (optional but recommended)
- ⬜ Input validation
- ⬜ API key authentication (optional)

## Tips

1. **Use the provided repositories** - They already have useful query methods
2. **Error responses** - Use `ErrorResponse.of()` helpers for consistent format
3. **Validation** - Add `@Valid` and Jakarta validation annotations to DTOs
4. **Entity mapping** - Consider using MapStruct or manual mapping for DTO conversion
5. **JSON format** - The frontend expects camelCase property names (already configured)
