# Concourse Pipeline for Spring Boot Hello World

This directory contains a Concourse CI/CD pipeline configuration to build the Spring Boot application using `mvn clean install`.

## Files

- `pipeline.yml` - Main Concourse pipeline configuration
- `params.yml` - Parameters file with Git repository settings

## Pipeline Features

- **Automatic Triggering**: Builds on every Git commit to the specified branch
- **Java 17 Support**: Uses Maven 3.9.0 with OpenJDK 17
- **Maven Caching**: Caches Maven dependencies for faster builds
- **Artifact Output**: Copies built JAR files and POM to build artifacts

## Setup Instructions

### 1. Update Parameters

Edit `params.yml` and update:
```yaml
git-repo-uri: https://github.com/YOUR-USERNAME/spring-boot-hello-world.git
git-branch: main
```

### 2. Deploy Pipeline

```bash
# Login to Concourse
fly -t your-target login -c http://your-concourse-url

# Set the pipeline
fly -t your-target set-pipeline -p spring-boot-build -c pipeline.yml -l params.yml

# Unpause the pipeline
fly -t your-target unpause-pipeline -p spring-boot-build
```

### 3. Trigger Build

```bash
# Manual trigger (optional)
fly -t your-target trigger-job -j spring-boot-build/build-spring-boot
```

## Pipeline Jobs

### build-spring-boot
- Clones the Git repository
- Runs `mvn clean install -B -DskipTests=false`
- Outputs build artifacts including JAR files
- Uses Maven dependency caching for performance

## Monitoring

View pipeline status at: `http://your-concourse-url/teams/main/pipelines/spring-boot-build`
