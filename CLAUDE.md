# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About Nacos

Nacos (Dynamic **Na**ming and **Co**nfiguration **S**ervice) is an easy-to-use platform designed for dynamic service discovery, configuration management, and service management. It helps build cloud native applications and microservices platforms. This is a Java-based enterprise service registry and configuration center.

## Technology Stack

- **Language**: Java 17+ (JDK 17 required for compilation and runtime)
- **Build Tool**: Maven 3.6.3+ (uses Maven multi-module project structure)
- **Framework**: Spring Boot 3.4.4
- **Database**: MySQL (primary), Derby (embedded for testing)
- **Communication**: gRPC, HTTP/REST APIs
- **Consensus**: JRaft for distributed consistency
- **Frontend**: React-based console-ui (separate build process)

## Architecture Overview

Nacos is a multi-module Maven project with the following key architectural components:

### Core Modules
- **core**: Central coordination, cluster management, gRPC server, distributed protocols
- **naming**: Service discovery and health checking functionality  
- **config**: Configuration management and distribution
- **api**: Client API definitions and protocol buffers
- **client**: Client SDK for service registration and configuration consumption

### Supporting Modules
- **console**: Web admin interface backend
- **console-ui**: React-based frontend admin dashboard
- **auth**: Authentication and authorization
- **persistence**: Database abstraction layer
- **common**: Shared utilities and base classes

### Plugin Architecture
- **plugin**: Core plugin interfaces
- **plugin-default-impl**: Default implementations for auth, control, etc.

### New AI Features
- **ai**: MCP (Model Context Protocol) server management and tooling
- **mcp-registry-adaptor**: Integration adapter for MCP registry functionality

## Development Commands

### Java Backend

```bash
# Build entire project (skip tests)
mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U

# Alternative using Maven wrapper
./mvnw -Prelease-nacos -Dmaven.test.skip=true clean install -U

# Run tests
mvn test

# Run specific integration tests
mvn test -Pcit-test    # Configuration integration tests
mvn test -Pnit-test    # Naming integration tests

# Code quality checks
mvn checkstyle:check   # CheckStyle validation
mvn pmd:check         # PMD static analysis
```

### Frontend Console UI

Navigate to `console-ui/` directory:

```bash
# Install dependencies
npm install

# Development server
npm start

# Production build
npm run build

# Linting
npm run eslint
npm run eslint-fix
```

## Code Style and Quality

- Follows **Alibaba Java Coding Guidelines**
- Uses CheckStyle configuration in `style/NacosCheckStyle.xml`
- PMD rules configured for Alibaba P3C standards
- IntelliJ code style available in `style/nacos-code-style-for-idea.xml`

## Key Development Patterns

### Service Registration Architecture
- V2 architecture uses client-based management with connection lifecycle tracking
- Distro protocol handles data synchronization between cluster nodes
- Health checking supports multiple protocols (HTTP, TCP, MySQL)

### Configuration Management  
- Supports multiple data formats (properties, JSON, YAML, XML)
- Gray release capabilities for configuration rollout
- Built-in configuration encryption and access control

### Persistence Layer
- Abstract persistence service supports multiple databases
- Embedded Derby for standalone mode, external MySQL for cluster mode
- Snapshot operations for data consistency

### Plugin System
- SPI-based plugin architecture for extensibility
- Default implementations provided for core functionality
- Custom plugins supported for auth, datasource, environment, etc.

## Testing Strategy

- Unit tests for individual components
- Integration tests for cross-module functionality  
- CIT (Configuration Integration Tests) and NIT (Naming Integration Tests) profiles
- E2E tests in separate test modules

## Important Notes

- Always use the `develop` branch for new features and bug fixes
- JDK 17+ required for development
- Maven 3.6.3+ required due to enforcer plugin requirements  
- Frontend and backend have separate build processes
- Test coverage target is 80% for new code