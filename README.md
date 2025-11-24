# ft_transcendence

A full-stack real-time multiplayer Pong gaming platform with comprehensive user management, statistics tracking, and enterprise-grade monitoring infrastructure.

## 🎯 Overview

ft_transcendence is a modern web application that brings the classic Pong game into the 21st century with real-time multiplayer capabilities, social features, tournament systems, and comprehensive analytics. The platform features a microservices architecture with enterprise-grade monitoring and logging infrastructure.

### Key Highlights

- **Real-time Multiplayer Gaming**: Play Pong in real-time with friends or random opponents
- **Tournament System**: Participate in organized tournaments with bracket-style competition
- **Social Features**: Friend system, chat functionality, and user profiles
- **Comprehensive Analytics**: Detailed statistics and performance tracking for users and games
- **Enterprise Monitoring**: Full observability stack with ELK and Prometheus/Grafana
- **Secure Authentication**: Multi-factor authentication with OAuth2 integration

## 🏗 Architecture

The application follows a microservices architecture with the following components:

```
                    ┌─────────────────────────────────────────┐
                    │         NGINX (Reverse Proxy)           │
                    │         SSL/TLS Termination             │
                    └─────────────────────────────────────────┘
                                      │
        ┌─────────────┬───────────────┼──────────────┬─────────────┬─────────────┐
        │             │               │              │             │             │
        ▼             ▼               ▼              ▼             ▼             ▼
┌───────────┐  ┌────────────┐  ┌────────────┐ ┌────────────┐ ┌─────────┐ ┌─────────┐
│  Next.js  │  │User Service│  │Game Service│ │Chat Service│ │ Grafana │ │ Kibana  │
│ Frontend  │  │ (Fastify)  │  │ (Fastify)  │ │ (Fastify)  │ │         │ │         │
└───────────┘  └─────┬──────┘  └─────┬──────┘ └─────┬──────┘ └─────────┘ └─────────┘
                     │               │              │
                     ▼               ▼              ▼
               ┌───────────┐   ┌───────────┐  ┌───────────┐
               │  SQLite   │   │  SQLite   │  │  SQLite   │
               │    DB     │   │    DB     │  │    DB     │
               └───────────┘   └───────────┘  └───────────┘
```

### Monitoring Stack

```
┌──────────────────────────────────────────────────────────────┐
│                     Monitoring & Logging                     │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ELK Stack                    │    Prometheus Stack          │
│  ├─ Elasticsearch             │    ├─ Prometheus             │
│  ├─ Logstash                  │    ├─ Grafana                │
│  └─ Kibana                    │    ├─ Node Exporter          │
│                               │    ├─ cAdvisor               │
│                               │    ├─ Nginx Exporter         │
│                               │    ├─ Alertmanager           │
│                               │    └─ Thanos                 │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

## 🚀 Getting Started

### Prerequisites
- Docker & Docker Compose
- Make (optional, for using Makefile commands)

### Environment Setup

1. Clone the repository:
```bash
git clone https://github.com/HamzaOuamrhar/ft_transcendence.git
cd ft_transcendence
```

2. Create environment files for each service:

#### `.env` (DevOps/Monitoring Configuration)
```env
# ELK Stack
ELASTIC_PASSWORD=your_elastic_password
KIBANA_PASSWORD=your_kibana_password
LOGSTASH_PASSWORD=your_logstash_password
KIBANA_ENCRYPTION_KEY=your_32_char_encryption_key

# Object Storage - Thanos (Prometheus Metrics Archival)
ACCESS_KEY_ID=your_s3_access_key_for_thanos
SECRET_ACCESS_KEY=your_s3_secret_key_for_thanos

# Object Storage - Elasticsearch (Logs Archival)
ACCESS_KEY_ID2=your_s3_access_key_for_elasticsearch
SECRET_ACCESS_KEY2=your_s3_secret_key_for_elasticsearch

# Monitoring
GRAFANA_USER=admin
GRAFANA_PASSWORD=your_grafana_password
DISCORD_WEBHOOK=your_discord_webhook_url
```

#### `frontend/.env`
```env
# API Endpoints
NEXT_PUBLIC_CHAT_API=https://localhost:8080/api/chat
NEXT_PUBLIC_URL=https://localhost:8080/

# Authentication
JWT_SECRET=pingpongsupersecretkey

# Internal Service Communication
CHAT_API=https://localhost:8080/api/chat
```

#### `services/user-service/.env`
```env
# Application
PORT=5000
HOST=0.0.0.0
NODE_ENV=development

# Authentication
JWT_SECRET=pingpongsupersecretkey
INTERNAL_API_KEY=pingpongsupersecretkey
SESSION_SECRET=your_session_secret_here

# OAuth2 (Google)
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
CALL_BACK_URI=https://localhost:8080/api/auth/google/callback

# URLs
FRONTEND_URL=https://your_frontend_url
BACKEND_BASE_URL=https://localhost:8080

# Email (for verification and password reset)
EMAIL=your_email@gmail.com
PASSWORD=your_email_app_password
```

#### `services/game-service/.env`
```env
# Application
FASTIFY_PORT=4040
SOCKETIO_PORT=4001

# Database
DB_NAME=game.db

# Authentication
JWT_SECRET=pingpongsupersecretkey
INTERNAL_API_KEY=pingpongsupersecretkey

# Service Communication
USER_SERVICE_HOST=http://user-service:5000
```

#### `services/chat-service/.env`
```env
# API Endpoints
NEXT_PUBLIC_CHAT_API=https://localhost:8080/api/chat
NEXT_PUBLIC_API=https://localhost:8080/
NEXT_PUBLIC_WEBSOCKET_URL=https://localhost:8080/

# Authentication
JWT_SECRET=pingpongsupersecretkey
INTERNAL_API_KEY=pingpongsupersecretkey

# Service Communication
CHAT_API=https://localhost:8080/api/chat
USER_SERVICE_URL=http://user-service:5000
```
**Note**: Make sure all JWT_SECRET values are identical across services for proper authentication.

### Running the Application

#### Development Mode
```bash
# Using Docker Compose
docker-compose -f docker-compose.dev.yml up --build

# Using Makefile
make dev
```

#### Production Mode
```bash
# Using Docker Compose
docker-compose -f docker-compose.prod.yml up --build

# Using Makefile
make prod
```

### Access Points

- **Application**: https://localhost
- **Grafana**: https://localhost/grafana
- **Kibana**: https://localhost/kibana

## 📁 Project Structure

```
ft_transcendence/
├── frontend/                  # Next.js frontend application
│   ├── src/
│   │   ├── app/              # Next.js app directory
│   │   ├── components/       # React components
│   │   │   ├── dashboard/    # Dashboard & stats components
│   │   │   ├── leaderboard/  # Leaderboard components
│   │   │   ├── game/         # Game-related components
│   │   │   └── chat/         # Chat components
│   │   ├── hooks/            # Custom React hooks
│   │   ├── context/          # React context providers
│   │   └── utils/            # Utility functions
│   └── public/               # Static assets
│
├── services/
│   ├── user-service/         # User management & authentication
│   ├── game-service/         # Game logic & matchmaking
│   ├── chat-service/         # Real-time chat functionality
│   └── nginx/                # Reverse proxy configuration
│
├── elk-stack/                # Log management infrastructure
│   ├── elasticsearch/        # Search & analytics engine
│   ├── logstash/            # Log processing pipeline
│   ├── kibana/              # Visualization & exploration
│   └── setup/               # Initial setup & dashboards
│
├── monitoring/               # Monitoring & alerting
│   ├── prometheus.yml       # Prometheus configuration
│   ├── alert_rules.yml      # Alerting rules
│   ├── alertmanager.yml     # Alert routing config
│   └── cadvisor/            # Container metrics collector
│
├── cli-client/              # Command-line game client
│
├── docker-compose.yml       # Base Docker Compose config
├── docker-compose.dev.yml   # Development environment
├── docker-compose.prod.yml  # Production environment
└── Makefile                 # Build & deployment commands
```

## 👥 Team Contributions

This project was developed as a collaborative team effort. Below is a detailed breakdown of individual contributions:

### My Contributions

#### 🎨 User and Game Statistics Dashboards

I was responsible for designing and implementing comprehensive statistics dashboards that provide users with deep insights into their gaming performance and history.

**Dashboard Features Implemented:**

1. **User Statistics Dashboard** (`frontend/src/app/(protected)/home/page.tsx`)
   - **Win/Loss Visualization**: 
     - Implemented interactive doughnut chart using Chart.js to display win rate percentage
     - Dynamic color-coded visual representation (wins in purple, losses in gray)
     - Handles edge cases (no games played state)
   
   - **Performance Metrics**:
     - Total games played counter
     - Player level and points tracking
     - Win/loss ratio with percentage calculation
   
   - **Time-Based Statistics**:
     - Total playtime aggregation (days, hours, minutes, seconds)
     - Average game duration analysis
     - Longest game session tracking
     - Custom time conversion utilities for user-friendly display

   - **Weekly Activity Chart**:
     - Bar chart showing games played per week of the current month
     - Dynamic month/year labeling
     - Color-coded weekly performance visualization
     - Responsive chart scaling based on data

2. **Game Session Dashboard**
   - **Match History Table**:
     - Comprehensive game history with infinite scroll pagination
     - Displays: Date/time, game type, scores, duration, result (WIN/LOSS)
     - Color-coded result indicators (green for wins, red for losses)
     - Lazy loading with IntersectionObserver for performance optimization
     - Empty state handling with custom placeholder graphics

3. **Statistics API Integration**
   - Created data fetching utilities (`frontend/src/app/(protected)/lib/dashboard.tsx`):
     - `GetGames()`: Fetches complete match history for user
     - `GetStats()`: Retrieves aggregated statistics (total/avg/longest playtime)
     - `GetWeekStats()`: Pulls weekly activity data for chart visualization
   
4. **Data Visualization Components** (`frontend/src/components/dashboard/`)

---

#### 📊 Infrastructure Setup with ELK Stack for Log Management

I architected and implemented the complete ELK (Elasticsearch, Logstash, Kibana) stack for centralized log management, aggregation, and analysis across all microservices.

**ELK Stack Implementation:**

1. **Elasticsearch Configuration** (`elk-stack/elasticsearch/`)
   - **Setup & Security**:
     - Configured single-node Elasticsearch cluster with TLS/SSL encryption
     - Implemented certificate-based authentication
     - Created automated keystore setup for secure credential management
     - Configured S3 snapshot repository for backup and disaster recovery
   
   - **Docker Configuration**:
     - Custom Dockerfile with health checks
     - Volume management for persistent data storage
     - Certificate sharing with other ELK components
     - Environment-based configuration for passwords and credentials

2. **Logstash Pipeline** (`elk-stack/logstash/`)
   - **Log Collection**:
     - Configured file inputs for all three microservices:
       - `game.log`: Game service events and metrics
       - `user.log`: User authentication and activity logs
       - `chat.log`: Chat service communications
     - JSON codec for structured log parsing
   
   - **Log Processing**:
     - Implemented filters for JSON parsing and field extraction
     - Date/time standardization using ISO8601 format
     - Service field validation (drops logs without service identifier)
     - Enrichment of log data with metadata
   
   - **Output Configuration**:
     - Secure Elasticsearch output with TLS
     - Indexed logs into `transcendence-logs` index
     - Retry logic for failed writes (retry_on_conflict: 3)
     - Debug output to stdout for monitoring

3. **Kibana Visualization** (`elk-stack/kibana/`)
   - **Dashboard Setup**:
     - Configured Kibana with SSL/TLS for secure access
     - Automated dashboard import via setup container
     - Created custom visualizations for application metrics
     - Implemented index patterns for log exploration
   
   - **Security Configuration**:
     - HTTPS enforcement with certificate management
     - Password-based authentication integration
     - Encryption key configuration for secure sessions

4. **Automated Setup Container** (`elk-stack/setup/`)
   - **Certificate Generation**:
     - Automated CA and certificate generation for all components
     - Certificate distribution to Elasticsearch, Logstash, and Kibana
     - Instance configuration via `instances.yml`
   
   - **Bootstrap Process**:
     - Automated user creation (elastic, kibana_system, logstash_author)
     - Role and permission assignment (logstash_writer role for log indexing)
     - Dashboard import automation
     - Health check system for orchestration
   
   - **S3 Snapshot Repository Setup**:
     - Configured S3 repository for Elasticsearch backups
     - Automated snapshot lifecycle management (SLM) policy
     - Daily snapshots scheduled at 23:50 for `transcendence-logs` index
     - Long-term log retention in S3 (separate from Thanos metrics storage)
   
   - **Index Lifecycle Management (ILM)**:
     - Implemented `logs-delete-daily` ILM policy
     - Hot phase with priority 100 for active logs
     - Delete phase after 1 day to manage storage efficiently
     - Applied to `transcendence-logs` index template
     - Automatic log rotation and cleanup

5. **Application-Level Logging Integration**
   - **User Service** (`services/user-service/src/app.js`):
     - Implemented structured logging with Fastify logger
     - Created `logEvent()` utility for consistent log formatting
     - Request/response logging middleware
     - File stream output to `/logs/user.log`
   
   - **Game Service** (`services/game-service/src/server.ts`):
     - TypeScript-based logging implementation
     - Structured log format with timestamps
     - Service identification in log entries
     - File stream output to `/logs/game.log`
   
   - **Log Format Standardization**:
     ```json
     {
       "service": "user|game|chat",
       "event": "event_name",
       "time": "ISO8601_timestamp",
       "level": "info|warn|error|debug",
       ...additional_data
     }
     ```

6. **Data Retention & Management**
   - **Local Storage**:
     - Configured volume persistence for Elasticsearch data
     - Index Lifecycle Management (ILM) for automated cleanup
     - Hot phase optimization for recent logs
     - Automatic deletion after 1 day retention period
   
   - **Long-term Archival**:
     - S3 snapshot repository for Elasticsearch logs backup
     - Daily automated snapshots via Snapshot Lifecycle Management (SLM)
     - Scheduled at 23:50 UTC for off-peak backup operations
     - Separate S3 bucket from Thanos (Prometheus metrics storage)
     - Point-in-time recovery capabilities

**ELK Stack Architecture:**
```
Application Logs → Logstash → Elasticsearch → Kibana
     (JSON)      (Processing)   (Indexing)   (Visualization)
                                     ↓
                                S3 Snapshots
                                 (Backups)
```

**Key Benefits Delivered:**
- Centralized log aggregation from all microservices
- Real-time log analysis and troubleshooting capabilities
- Historical log retention and searchability
- Security event monitoring and audit trails
- Performance issue detection and analysis

---

#### 🔍 Monitoring System with Prometheus & Grafana

I designed and implemented a comprehensive monitoring infrastructure using Prometheus for metrics collection and Grafana for visualization, providing real-time insights into system health and performance.

**Monitoring Stack Implementation:**

1. **Prometheus Configuration** (`monitoring/prometheus.yml`)
   - **Scrape Configurations**:
     - **Host Metrics**: Node Exporter (5s interval) for system-level metrics
       - CPU usage, memory consumption, disk I/O
       - Network statistics
       - File system metrics
     
     - **Container Metrics**: cAdvisor (5s interval)
       - Docker container resource usage
       - Container-level CPU and memory stats
       - Network and disk I/O per container
     
     - **Nginx Metrics**: Nginx Exporter (15s interval)
       - Active connections
       - Request rate and response times
       - HTTP status code distribution
   
   - **Global Configuration**:
     - 10-second default scrape interval
     - External labels for cluster identification
     - Service discovery for dynamic target management

2. **Alert Rules Implementation** (`monitoring/alert_rules.yml`)
   - **Server Health Alerts**:
     - **HighCPUUsage**: Triggers when CPU usage > 90% for 1 minute
       - Severity: Warning
       - Includes instance-specific details
     
     - **HighMemoryUsage**: Triggers when memory usage > 90% for 1 minute
       - Severity: Warning
       - Calculated based on available vs total memory
   
   - **Application Alerts**:
     - **NginxExporterDown**: Detects when Nginx exporter is unreachable
       - Severity: Critical
       - 1-minute detection window
     
     - **NginxHighConnections**: Monitors connection spikes (>100 connections)
       - Severity: Warning
       - 2-minute threshold

3. **Alertmanager Setup** (`monitoring/alertmanager.yml`)
   - Configured alert routing and notification
   - Discord webhook integration for real-time alerts
   - Alert grouping and deduplication
   - Custom alert templates for readable notifications

4. **Grafana Configuration**
   - **Dashboard Setup**:
     - Automated Prometheus datasource provisioning
     - Pre-configured dashboards for:
       - Server health metrics
       - Docker container statistics
       - Nginx performance metrics
     - SSL/TLS configuration for secure access
   
   - **Security Implementation**:
     - HTTPS enforcement with certificate management
     - Admin user configuration via environment variables
     - Cookie security settings
     - Reverse proxy compatibility

5. **Metrics Exporters**
   - **Node Exporter**:
     - Host-level metrics collection
     - Host network mode for accurate system metrics
     - PID namespace sharing for process monitoring
   
   - **cAdvisor** (`monitoring/cadvisor/`):
     - Custom Docker build for container metrics
     - Docker socket mounting for container discovery
     - System directory mounts for accurate resource tracking
     - Resource usage tracking per container
   
   - **Nginx Exporter**:
     - Real-time Nginx metrics scraping
     - Connection and request statistics
     - Integration with Nginx stub_status module

6. **Thanos Integration**
   - **Long-term Metrics Storage**:
     - Thanos sidecar deployment alongside Prometheus
     - S3-compatible object storage for metrics backup
     - Configured retention policies:
       - Local retention: 6 months
       - Max storage size: 50GB
       - Block duration: 30 minutes
   
   - **High Availability**:
     - Automated metrics upload to object storage
     - Query federation capabilities
     - Downsampling for long-term storage efficiency

7. **Docker Integration**
   - Configured all monitoring components in `docker-compose.yml`:
     - Prometheus with persistent volume storage
     - Grafana with SSL certificates and datasource provisioning
     - Alertmanager with Discord integration
     - All exporters with appropriate network access
   
   - **Volume Management**:
     - `prometheus_data`: Persistent metrics storage
     - `grafana_data`: Dashboard and configuration persistence
     - Certificate volumes for secure communication

**Monitoring Architecture:**
```
┌─────────────────────────────────────────────────────────────┐
│                    Metrics Collection                        │
├─────────────────────────────────────────────────────────────┤
│  Node Exporter  →  System Metrics   →                      │
│  cAdvisor       →  Container Stats  →   Prometheus          │
│  Nginx Exporter →  Proxy Metrics    →                      │
└─────────────────────────────────────────────────────────────┘
                              ↓
                   ┌──────────┴──────────┐
                   ↓                     ↓
            Alertmanager           Thanos Sidecar
                   ↓                     ↓
           Discord Alerts          S3 Storage
                                        ↓
                                   Grafana
                                (Visualization)
```

**Key Features Delivered:**

1. **Real-time Monitoring**:
   - Live metrics dashboards with sub-10 second updates
   - Visual indicators for system health
   - Historical trend analysis

2. **Proactive Alerting**:
   - Automated issue detection before user impact
   - Discord notifications for immediate team response
   - Configurable alert thresholds and severity levels

3. **Performance Insights**:
   - Resource utilization tracking (CPU, memory, disk, network)
   - Application-level metrics (Nginx connections, request rates)
   - Container-specific resource consumption

4. **Scalability**:
   - Long-term metrics retention with Thanos
   - Efficient storage with downsampling
   - Query federation for distributed setups

5. **Security**:
   - HTTPS-only access to Grafana
   - Secure credential management
   - Certificate-based authentication

**Technical Skills Demonstrated:**
- Prometheus query language (PromQL)
- Grafana dashboard creation and provisioning
- Container monitoring and resource management
- Alert rule configuration and testing
- Time-series data management
- Infrastructure as Code with Docker Compose
- S3 integration for data persistence
- SSL/TLS certificate management

---

### Impact of My Contributions

My work on the statistics dashboards, ELK stack, and monitoring infrastructure provided:

✅ **Enhanced User Experience**: Users can track their progress, analyze performance, and set goals based on comprehensive statistics

✅ **Operational Excellence**: Complete visibility into system behavior with centralized logging and real-time monitoring

✅ **Proactive Issue Resolution**: Automated alerting enables rapid response to system issues before they impact users

✅ **Data-Driven Decisions**: Rich analytics enable informed decisions about system optimization and feature development

✅ **Scalability Foundation**: Infrastructure capable of supporting growth and increased load

✅ **Security & Compliance**: Comprehensive audit trails and monitoring for security events

---

## 📝 License

This project is part of the 42 Network curriculum.

---

## 🙏 Acknowledgments

- **42 Network** for the project specifications and educational framework
- **Team Members** for collaborative development effort
- **Open Source Community** for the amazing tools and libraries used

---

**Project Status**: ✅ Active Development

**Team**: HamzaOuamrhar and collaborators

**Repository**: [ft_transcendence](https://github.com/HamzaOuamrhar/ft_transcendence)
