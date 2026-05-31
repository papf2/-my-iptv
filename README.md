# My IPTV - Personal Legal Canadian IPTV Service

A Gradle-based Spring Boot application for building a personal legal Canadian IPTV service with support for Live TV, VOD, Recording capabilities, and multi-device streaming.

## Features

- 📺 **Live TV Channels** - Stream licensed Canadian content
- 🎬 **Video on Demand (VOD)** - Manage your personal media library with decade filtering
- 📹 **Recording Capabilities** - Schedule and manage channel recordings
- 📱 **Multi-Device Support** - Stream on web, mobile, smart TV, and more
- 👥 **User Management** - Authentication and device management
- 🎛️ **Content Management** - Organize channels, content, and recordings
- 📅 **Decade Filtering** - Browse content by era (2000s, 2010s, 2020s, Present)

## Decade Support

Content can be organized and filtered by decades:
- **2000s** (2000-2009)
- **2010s** (2010-2019)
- **2020s** (2020-2029)
- **Present** (2030+)

## Architecture

```
my-iptv/
├── src/
│   ├── main/
│   │   ├── kotlin/
│   │   │   └── com/iptv/
│   │   │       ├── controller/    # REST API endpoints
│   │   │       ├── service/       # Business logic
│   │   │       ├── repository/    # Data access
│   │   │       └── domain/entity/ # Domain models
│   │   └── resources/
│   │       └── application.yml    # Configuration
│   └── test/
├── build.gradle.kts               # Gradle build configuration
└── README.md
```

## Tech Stack

- **Framework**: Spring Boot 3.1.0
- **Language**: Kotlin 1.9.0
- **Database**: PostgreSQL
- **Build Tool**: Gradle
- **Streaming**: HLS/DASH protocols
- **Authentication**: JWT

## Prerequisites

- Java 17+
- Gradle 8.0+
- PostgreSQL 12+
- Docker (optional)

## Setup

### 1. Database Setup

```bash
# Create database
createdb iptv_db

# Create user
psql -d iptv_db -c "CREATE USER iptv_user WITH PASSWORD 'iptv_password';"
psql -d iptv_db -c "GRANT ALL PRIVILEGES ON DATABASE iptv_db TO iptv_user;"
```

### 2. Environment Variables

```bash
export DB_PASSWORD=your_secure_password
export JWT_SECRET=your_jwt_secret_key
```

### 3. Build and Run

```bash
# Build project
./gradlew build

# Run application
./gradlew bootRun
```

The API will be available at `http://localhost:8080/api`

## API Endpoints

### Channels
- `GET /api/channels` - List all active channels
- `GET /api/channels/{id}` - Get channel details
- `GET /api/channels/category/{category}` - Get channels by category
- `POST /api/channels` - Create new channel (admin)
- `PUT /api/channels/{id}` - Update channel (admin)
- `DELETE /api/channels/{id}` - Delete channel (admin)

### Content (VOD) - With Decade Filtering
- `GET /api/content` - List all available VOD content
- `GET /api/content/{id}` - Get content details
- `GET /api/content/category/{category}` - Get content by category
- `GET /api/content/decade/{decade}` - Get content by decade
  - Available decades: `DECADE_2000S`, `DECADE_2010S`, `DECADE_2020S`, `PRESENT`
- `GET /api/content/category/{category}/decade/{decade}` - Filter by category and decade
- `GET /api/content/search?title={title}` - Search content by title
- `GET /api/content/search/{decade}?title={title}` - Search content by title within a decade
- `GET /api/content/decades` - List all decades with content counts
- `POST /api/content` - Upload new content with decade
- `PUT /api/content/{id}` - Update content
- `DELETE /api/content/{id}` - Remove content

### Recordings
- `GET /api/recordings` - List all recordings
- `POST /api/recordings` - Schedule new recording
- `DELETE /api/recordings/{id}` - Cancel recording

### Users & Devices
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `GET /api/devices` - List user devices
- `POST /api/devices` - Register new device

## Example API Requests

### Create Content with Decade
```bash
curl -X POST http://localhost:8080/api/content \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Breaking Bad",
    "description": "Crime thriller series",
    "contentType": "SERIES",
    "filePath": "/media/breaking-bad.mp4",
    "thumbnailUrl": "https://example.com/breaking-bad.jpg",
    "durationSeconds": 3600,
    "category": "Drama",
    "decade": "DECADE_2010S",
    "rating": "TV-MA"
  }'
```

### Get Content by Decade
```bash
# Get all 2010s content
curl http://localhost:8080/api/content/decade/DECADE_2010S

# Get drama from 2020s
curl http://localhost:8080/api/content/category/Drama/decade/DECADE_2020S

# Search for "Friends" in 2000s
curl "http://localhost:8080/api/content/search/DECADE_2000S?title=Friends"
```

### Get All Decades with Statistics
```bash
curl http://localhost:8080/api/content/decades
```

Response:
```json
[
  {
    "decade": "DECADE_2000S",
    "displayName": "2000s",
    "startYear": 2000,
    "endYear": 2009,
    "contentCount": 45
  },
  {
    "decade": "DECADE_2010S",
    "displayName": "2010s",
    "startYear": 2010,
    "endYear": 2019,
    "contentCount": 67
  },
  {
    "decade": "DECADE_2020S",
    "displayName": "2020s",
    "startYear": 2020,
    "endYear": 2029,
    "contentCount": 32
  },
  {
    "decade": "PRESENT",
    "displayName": "Present",
    "startYear": 2030,
    "endYear": 9999,
    "contentCount": 12
  }
]
```

## Legal Compliance

This IPTV service is designed for **personal use only** with licensed content:
- Canadian broadcast content you're legally authorized to stream
- Licensed streaming partnerships
- Content you personally own or have rights to distribute

**Important**: Ensure all content distributed through this service is legally licensed and complies with Canadian copyright laws and regulations.

## Development Roadmap

- [x] Content decade filtering
- [ ] HLS/DASH streaming implementation
- [ ] Web player UI (Vue.js/React)
- [ ] Mobile apps (iOS/Android)
- [ ] EPG (Electronic Program Guide) integration
- [ ] DRM (Digital Rights Management) support
- [ ] Advanced recording scheduler
- [ ] Billing/subscription system

## Contributing

Feel free to submit issues and enhancement requests!

## License

MIT License - See LICENSE file for details