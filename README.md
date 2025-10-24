# Configuration Service

This directory contains centralized configuration files for all microservices in the Farmatodo project. Each service has separate configuration files for development and production environments.

## Directory Structure

```
config-service/
├── api-gateway-dev.properties       # API Gateway - Development
├── api-gateway-prod.properties      # API Gateway - Production
├── client-service-dev.yml           # Client Service - Development
├── client-service-prod.yml          # Client Service - Production
├── token-service-dev.yml            # Token Service - Development
├── token-service-prod.yml           # Token Service - Production
├── product-service-dev.yml          # Product Service - Development
├── product-service-prod.yml         # Product Service - Production
├── cart-service-dev.yml             # Cart Service - Development
├── cart-service-prod.yml            # Cart Service - Production
└── README.md                        # This file
```

## Configuration Files

### API Gateway
- **Port:** 8080
- **Dev Config:** `api-gateway-dev.properties`
- **Prod Config:** `api-gateway-prod.properties`
- **Format:** Properties file

### Client Service
- **Port:** 8081
- **Database:** clientdb (PostgreSQL on port 5432)
- **Dev Config:** `client-service-dev.yml`
- **Prod Config:** `client-service-prod.yml`
- **Format:** YAML

### Token Service
- **Port:** 8082
- **Database:** tokendb (PostgreSQL on port 5433)
- **Dev Config:** `token-service-dev.yml`
- **Prod Config:** `token-service-prod.yml`
- **Format:** YAML

### Product Service
- **Port:** 8083
- **Database:** productdb (PostgreSQL on port 5434)
- **Dev Config:** `product-service-dev.yml`
- **Prod Config:** `product-service-prod.yml`
- **Format:** YAML

### Cart Service
- **Port:** 8084
- **Database:** cartdb (PostgreSQL on port 5434)
- **Dev Config:** `cart-service-dev.yml`
- **Prod Config:** `cart-service-prod.yml`
- **Format:** YAML

## Usage

### Option 1: Manual Copy

Copy the appropriate configuration file to the service's resources directory:

```bash
# For development
cp config-service/client-service-dev.yml client-service/src/main/resources/application.yml

# For production
cp config-service/client-service-prod.yml client-service/src/main/resources/application.yml
```

### Option 2: Spring Profiles

Use Spring profiles to load the appropriate configuration:

1. Copy both dev and prod configs to the service's resources directory
2. Rename them to use Spring profile naming:
   - `application-dev.yml`
   - `application-prod.yml`
3. Run with the appropriate profile:

```bash
# Development
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev

# Production
./mvnw spring-boot:run -Dspring-boot.run.profiles=prod
```

### Option 3: External Configuration

Reference the configuration files from their current location:

```bash
./mvnw spring-boot:run -Dspring.config.location=file:../config-service/client-service-dev.yml
```

### Option 4: Spring Cloud Config Server

For a more advanced setup, implement a Spring Cloud Config Server that serves these configurations to all microservices dynamically.

## Environment-Specific Settings

### Development Environment
- **Logging:** DEBUG level for application code
- **Database:** DDL auto-update enabled
- **SQL Logging:** Enabled with formatting
- **Health Endpoints:** Full details exposed
- **Database Hosts:** localhost with various ports
- **API Keys:** Simple development keys

### Production Environment
- **Logging:** INFO/WARN levels, file-based logging
- **Database:** DDL set to validate only (no auto-changes)
- **SQL Logging:** Disabled
- **Health Endpoints:** Limited details (when-authorized)
- **Database Hosts:** Service names for Docker/Kubernetes
- **API Keys:** Environment variables (must be configured)
- **Connection Pooling:** Configured for production load
- **Log Files:** `/var/log/<service-name>/application.log`

## Security Notes

### Production Secrets

**IMPORTANT:** The production configuration files use environment variables for sensitive data. Before deploying to production, ensure these environment variables are set:

**Client Service:**
- `DB_USERNAME` - Database username
- `DB_PASSWORD` - Database password
- `CLIENT_SERVICE_API_KEY` - API authentication key

**Token Service:**
- `DB_USERNAME` - Database username
- `DB_PASSWORD` - Database password
- `TOKEN_SERVICE_API_KEY` - API authentication key
- `ENCRYPTION_SECRET_KEY` - 256-bit encryption key

**Product Service:**
- `DB_USERNAME` - Database username
- `DB_PASSWORD` - Database password
- `PRODUCT_SERVICE_API_KEY` - API authentication key

**Cart Service:**
- `DB_USERNAME` - Database username
- `DB_PASSWORD` - Database password
- `CART_SERVICE_API_KEY` - API authentication key

### Generating Secure Keys

For production encryption keys (256-bit):
```bash
# Generate a random 256-bit key (32 characters)
openssl rand -base64 32
```

## Database Configuration

Each service uses its own PostgreSQL database:

| Service | Database | Port (Dev) | Port (Docker) |
|---------|----------|------------|---------------|
| client-service | clientdb | 5432 | 5432 |
| token-service | tokendb | 5433 | 5433 |
| product-service | productdb | 5434 | 5434 |
| cart-service | cartdb | 5434 | 5434 |

## Migration from Current Setup

To migrate from your current configuration:

1. Backup current configuration files
2. Choose your preferred configuration strategy (see Usage options above)
3. Update service-specific settings if needed
4. Test in development environment first
5. Deploy to production with proper environment variables

## Maintenance

When adding new services or modifying configurations:

1. Create both dev and prod configuration files
2. Use environment variables for sensitive data in production
3. Document any new environment variables in this README
4. Test configurations in both environments
5. Update the service URLs in api-gateway configurations if needed
