# Microservice Configuration Repository

A centralized configuration repository for microservices using Spring Cloud Config Server with Eureka service discovery.

## Table of Contents

- [Overview](#overview)
- [File Structure](#file-structure)
- [Configuration Details](#configuration-details)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Environment Configurations](#environment-configurations)
- [Service Discovery](#service-discovery)
- [Contributing](#contributing)

## Overview

This repository contains environment-specific configuration files for microservices in a Spring Cloud ecosystem. It serves as a centralized configuration store that can be used with Spring Cloud Config Server to manage application properties across different environments.

## File Structure

```
├── application.yml          # Base configuration (common settings)
├── application-dev.yml      # Development environment configuration
├── application-prod.yml     # Production environment configuration
└── README.md               # This file
```

## Configuration Details

### Base Configuration (`application.yml`)
Contains common Eureka client configuration that applies to all environments:
- Eureka service discovery settings
- Client registration preferences
- Default service registry URL

### Development Configuration (`application-dev.yml`)
- **Application Name**: `Config-Dev`
- **Environment**: Development
- **Eureka Server**: `http://localhost:8761/eureka`

### Production Configuration (`application-prod.yml`)
- **Application Name**: `Config-Prod`
- **Environment**: Production
- **Eureka Server**: `http://localhost:8761/eureka`

## Prerequisites

Before using these configurations, ensure you have:

1. **Java 8+** installed
2. **Spring Boot 2.x+** application
3. **Eureka Server** running on port 8761
4. **Spring Cloud Config Server** (if using centralized configuration)

### Required Dependencies

Add these dependencies to your `pom.xml` (Maven) or `build.gradle` (Gradle):

**Maven:**
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

**Gradle:**
```gradle
implementation 'org.springframework.cloud:spring-cloud-starter-netflix-eureka-client'
implementation 'org.springframework.cloud:spring-cloud-starter-config'
```

## Usage

### Option 1: Direct File Usage
1. Copy the appropriate configuration file to your microservice's `src/main/resources/` directory
2. Rename it to `application.yml` or `application-{profile}.yml`
3. Start your application with the desired profile:
   ```bash
   java -jar your-app.jar --spring.profiles.active=dev
   ```

### Option 2: Spring Cloud Config Server
1. Set up a Spring Cloud Config Server pointing to this repository
2. Configure your microservices to connect to the Config Server
3. Use the application names defined in the configuration files

**Config Server setup:**
```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/MohitBytes/microservice-configuration
          default-label: main
```

**Client configuration (bootstrap.yml):**
```yaml
spring:
  application:
    name: Config-Dev  # or Config-Prod for production
  cloud:
    config:
      uri: http://localhost:8888  # Config Server URL
```

## Environment Configurations

### Development Environment
- **Purpose**: Local development and testing
- **Eureka Server**: Local instance at `localhost:8761`
- **Features**: 
  - IP-based service registration
  - Automatic service discovery
  - Registry fetching enabled

### Production Environment
- **Purpose**: Live production deployment
- **Eureka Server**: Production instance at `localhost:8761` (update as needed)
- **Features**:
  - IP-based service registration for container environments
  - High availability service discovery
  - Production-ready configurations

## Service Discovery

All configurations include Eureka client settings:

- **prefer-ip-address**: `true` - Uses IP addresses instead of hostnames
- **fetch-registry**: `true` - Downloads service registry information
- **register-with-eureka**: `true` - Registers the service with Eureka
- **defaultZone**: Eureka server endpoint URL

### Customizing Eureka Configuration

To use a different Eureka server, update the `defaultZone` URL in the respective configuration files:

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://your-eureka-server:8761/eureka
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-environment`)
3. Add your configuration changes
4. Test with your microservices
5. Commit your changes (`git commit -am 'Add new environment configuration'`)
6. Push to the branch (`git push origin feature/new-environment`)
7. Create a Pull Request

### Configuration Guidelines

- Follow the existing YAML structure
- Use meaningful application names for different environments
- Ensure Eureka configuration is consistent
- Test configurations with actual microservices before submitting
- Document any new environment-specific settings

## Common Issues

### Eureka Connection Issues
- Ensure Eureka server is running before starting microservices
- Verify the `defaultZone` URL is correct
- Check network connectivity between services and Eureka server

### Configuration Not Loading
- Verify Spring Cloud Config Server is properly configured
- Check that application names match between client and configuration files
- Ensure Config Server can access this repository

---

**Note**: Update the Eureka server URLs in the configuration files to match your actual infrastructure setup before deploying to production.