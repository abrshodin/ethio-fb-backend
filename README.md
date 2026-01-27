# Ethiopian Football Backend API

A Go-based backend API service for Ethiopian football data, providing real-time match information, standings, news, and AI-powered natural language query processing for football enthusiasts.

## 🚀 Features

- **Live Match Data**: Real-time fixtures, scores, and match events
- **League Standings**: Up-to-date league tables and team rankings
- **Match History**: Access to previous fixtures and results
- **News & Updates**: Latest football news from various sources
- **AI-Powered Queries**: Natural language processing for user queries using Google's Gemini AI
- **Intent Recognition**: Smart interpretation of user requests about fixtures, standings, teams, and news
- **Redis Caching**: High-performance caching for faster response times
- **RESTful API**: Clean and well-structured API endpoints

## 🛠️ Tech Stack

- **Language**: Go 1.24
- **Web Framework**: [Gin](https://github.com/gin-gonic/gin) - Fast HTTP web framework
- **Database/Cache**: [Redis](https://github.com/redis/go-redis) - In-memory data store
- **AI Integration**: [Google Gemini AI](https://ai.google.dev/) - Natural language processing and intent parsing
- **External API**: [API-Football](https://www.api-football.com/) - Football data provider
- **Environment Config**: [godotenv](https://github.com/joho/godotenv) - Environment variable management

## 📋 Prerequisites

- Go 1.24 or higher
- Redis server (local or remote)
- API-Football API key ([Get one here](https://www.api-football.com/))
- Google Gemini API key ([Get one here](https://ai.google.dev/))

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/abrshodin/ethio-fb-backend.git
   cd ethio-fb-backend
   ```

2. **Install dependencies**
   ```bash
   go mod download
   ```

3. **Set up environment variables**
   
   Create a `.env` file in the root directory:
   ```env
   # Redis Configuration
   REDIS_ADDRESS=localhost:6379
   REDIS_USERNAME=
   REDIS_PASSWORD=

   # API Keys
   API_FOOTBALL_KEY=your_api_football_key_here
   GEMINI_API_KEY=your_gemini_api_key_here
   ```

4. **Run the application**
   ```bash
   go run Delivery/main.go
   ```

   The server will start on `http://localhost:8080` by default.

## 🏗️ Architecture

This project follows **Clean Architecture** principles with clear separation of concerns:

```
ethio-fb-backend/
├── Domain/          # Business entities and interfaces
├── Usecase/         # Business logic and use cases
├── Repository/      # Data access layer (Redis, external APIs)
├── Infrastructure/  # External services (AI, API clients, Redis)
└── Delivery/        # HTTP handlers and routing
    ├── Controller/  # Request handlers
    └── Router/      # Route definitions
```

### Layer Responsibilities

- **Domain**: Core business models (Team, Fixture, Standing, NewsItem, Intent, etc.)
- **Usecase**: Application business logic (parse intents, fetch fixtures, compose answers)
- **Repository**: Data persistence and external API interactions
- **Infrastructure**: Third-party integrations (Gemini AI, Redis, API-Football)
- **Delivery**: HTTP layer (Gin controllers and routers)

## 🔌 API Endpoints

### Health Check
- `GET /ping` - Server health check

### Fixtures
- `GET /fixtures` - Get fixtures with optional filters
  - Query params: `league`, `team`, `season`, `from`, `to`

### News
- `GET /news/pastMatches` - News about past matches
- `GET /news/standings` - Standing-related news
- `GET /news/futureMatches` - Upcoming match news
- `GET /news/liveScores` - Live score updates

### Standings
- `GET /api/standings` - League standings

### Teams
- `GET /team/:id/bio` - Get team information
- `POST /team/create` - Create new team

### API Services
- `GET /api/previous-fixtures` - Previous match history
- `GET /api/live` - Live fixtures

### AI-Powered Endpoints
- `POST /intent/parse` - Parse natural language queries
  - Understands queries about fixtures, standings, teams, news
  - Extracts intent, teams, leagues, and dates from user text
- `POST /answer` - Generate AI-powered responses

### Example Requests

```bash
# Get fixtures
curl "http://localhost:8080/fixtures?league=EPL&season=2024"

# Get standings
curl "http://localhost:8080/api/standings?league=39&season=2024"

# Parse intent (AI)
curl -X POST http://localhost:8080/intent/parse \
  -H "Content-Type: application/json" \
  -d '{"query": "What are the fixtures for Arsenal this weekend?"}'

# Get news
curl http://localhost:8080/news/liveScores
```

## 🗂️ Project Structure Details

```
.
├── Delivery/
│   ├── Controller/          # HTTP request handlers
│   │   ├── answer_controller.go
│   │   ├── fixture_controller.go
│   │   ├── intent_controller.go
│   │   ├── news_controller.go
│   │   └── standings_controller.go
│   ├── Router/              # Route configurations
│   │   └── route.go
│   └── main.go              # Application entry point
│
├── Domain/                  # Core business models
│   ├── domain.go            # Team, Fixture, Standing, etc.
│   ├── intent.go            # Intent parsing models
│   ├── answer.go            # AI answer models
│   ├── errors.go            # Domain errors
│   └── repo_interfaces.go   # Repository interfaces
│
├── Infrastructure/          # External integrations
│   ├── infra.go             # Redis connection, API client
│   ├── ai_intent_parser.go  # Gemini AI intent parsing
│   ├── ai_answer_composer.go # Gemini AI answer generation
│   └── api_service.go       # External API service
│
├── Repository/              # Data access implementations
│   ├── fixtures_repo.go     # Fixtures repository
│   ├── standings_repo.go    # Standings repository
│   ├── event_repository.go  # Event/news repository
│   └── repo.go              # Common repository logic
│
├── Usecase/                 # Business logic
│   ├── fixtures_usecase.go  # Fixture-related logic
│   ├── standings_usecase.go # Standings logic
│   ├── news_usecase.go      # News logic
│   ├── parse_intent.go      # Intent parsing logic
│   ├── compose_answer.go    # Answer composition logic
│   └── errors.go            # Use case errors
│
├── go.mod                   # Go module dependencies
├── go.sum                   # Dependency checksums
├── .gitignore              # Git ignore rules
└── README.md               # This file
```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `REDIS_ADDRESS` | Redis server address | Yes | `localhost:6379` |
| `REDIS_USERNAME` | Redis username | No | `admin` |
| `REDIS_PASSWORD` | Redis password | No | `your_password` |
| `API_FOOTBALL_KEY` | API-Football API key | Yes | `your_api_key` |
| `GEMINI_API_KEY` | Google Gemini API key | Yes | `your_gemini_key` |

## 🧪 Development

### Running the server
```bash
go run Delivery/main.go
```

### Building the application
```bash
go build -o ethio-fb-backend Delivery/main.go
```

### Running the built binary
```bash
./ethio-fb-backend
```

## 🤖 AI Features

The application uses Google's Gemini AI for two main purposes:

1. **Intent Parsing**: Converts natural language queries into structured intents
   - Identifies the topic (fixture, table, compare, news, fact)
   - Extracts team names, league information, and dates
   - Determines the language and follow-up context

2. **Answer Composition**: Generates natural language responses
   - Creates contextual, conversational answers
   - Supports multiple languages
   - Provides detailed football information in an accessible format

## 📊 Data Flow

1. User sends a request to an API endpoint
2. Controller receives and validates the request
3. Usecase processes the business logic
4. Repository fetches data from cache (Redis) or external APIs
5. Infrastructure handles external service calls (AI, API-Football)
6. Response flows back through the layers to the user

## 🔄 Caching Strategy

The application uses Redis for caching:
- **Fixtures**: Cached to reduce API calls
- **Standings**: Cached with freshness timestamps
- **Team Data**: Stored for quick access
- **Cache Invalidation**: Time-based expiration

## 🙏 Acknowledgments

- [API-Football](https://www.api-football.com/) for providing comprehensive football data
- [Google Gemini AI](https://ai.google.dev/) for natural language processing capabilities
- The Go community for excellent libraries and frameworks


