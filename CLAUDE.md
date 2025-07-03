# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Matecat is an enterprise-level, web-based Computer-Assisted Translation (CAT) tool. It's a complex application with PHP backend, React frontend, and extensive translation workflow features.

## Development Commands

### JavaScript/Frontend Development
```bash
# Run all tests in watch mode
npm test

# Run tests with coverage
npm run coverage

# Lint JavaScript files
npm run lint

# Format all files with Prettier
npm run format

# Build for development with file watching
npm run watch

# Build for development (single run)
npm run build:dev

# Build for production
npm run build:production
```

### PHP/Backend Development
```bash
# Run all PHP unit tests
vendor/bin/phpunit

# Run specific PHP test
vendor/bin/phpunit --filter=TestName

# Run tests with coverage
vendor/bin/phpunit --coverage-html coverage/

# Run database migrations
vendor/bin/phinx migrate

# Install PHP dependencies
composer install
```

### Database Management
```bash
# Run Phinx migrations
vendor/bin/phinx migrate

# Create new migration
vendor/bin/phinx create MigrationName

# Rollback migration
vendor/bin/phinx rollback
```

## Architecture Overview

### Core Components

**Backend (PHP)**:
- **Routing**: Klein-based routing with versioned APIs (v1, v2, v3)
- **Database**: MySQL with PDO, Redis for caching
- **Queue System**: ActiveMQ for background processing
- **Authentication**: JWT tokens, OAuth integration
- **File Storage**: AWS S3 or filesystem
- **Translation Engines**: Google Translate, DeepL, Microsoft Hub, MMT, etc.

**Frontend (JavaScript/React)**:
- **Framework**: React 18 with modern hooks
- **UI Library**: Semantic UI React
- **Build Tool**: Webpack with extensive configuration
- **State Management**: Flux pattern
- **Real-time**: Socket.io for WebSocket connections

**Key Services**:
- **Node.js Server**: Socket.io server for real-time updates (`nodejs/`)
- **File Processing**: External filters service integration
- **Quality Assurance**: LexiQA integration and custom QA models

### Directory Structure

- `lib/` - Core PHP application code
  - `Controller/` - API and view controllers
  - `Model/` - Database models and DAOs
  - `Utils/` - Utility classes and helpers
  - `Routes/` - API route definitions
  - `View/` - Template files
- `public/` - Frontend assets and entry points
- `plugins/` - Plugin architecture for extended functionality
- `tests/` - PHP unit tests
- `nodejs/` - Node.js Socket.io server
- `migrations/` - Database migration files
- `inc/` - Configuration and initialization files

## Key Features to Understand

### Translation Workflow
- **Segments**: Text is broken into translatable segments
- **Jobs**: Groups of segments assigned to translators
- **Projects**: Collections of jobs with metadata
- **Translation Memory (TM)**: Reusable translation database
- **Quality Assurance**: Multi-step review process

### File Processing
- Supports 40+ file formats (XLIFF, TMX, Office docs, etc.)
- External conversion pipeline via RapidAPI
- Metadata extraction and reference file handling

### User Management
- Teams and organizations structure
- Role-based permissions
- OAuth integration with major providers

### API Structure
- RESTful API with three major versions
- Comprehensive CRUD operations
- Rate limiting and authentication
- JSON validation using schemas

## Testing Strategy

### PHP Testing
- **PHPUnit** for backend testing
- Tests located in `tests/unit/`
- Database fixtures in `tests/resources/fixtures/`
- Abstract base class: `TestHelpers\AbstractTest`
- Coverage reporting available

### JavaScript Testing
- **Jest** with React Testing Library
- Tests co-located with source files
- Mock Service Worker (MSW) for API mocking
- Coverage reporting with Jest

### Test Data
- Extensive fixtures for database testing
- Sample files for file processing tests
- Mock data for frontend development

## Configuration

### Environment Setup
- Copy `inc/config.ini.sample` to `inc/config.ini`
- Configure database, Redis, and external services
- Set up OAuth providers in `inc/oauth_config.ini`

### Development vs Production
- Use `ENV` setting in config to control behavior
- Different configurations for development/staging/production
- Asset compilation differs between environments

## Database Schema

### Core Tables
- `projects` - Project management
- `jobs` - Translation jobs
- `segments` - Text segments for translation
- `segment_translations` - Translation data
- `users` - User management
- `teams` - Team collaboration
- `files` - File metadata
- `qa_entries` - Quality assurance issues

### Template System
- `project_templates` - Reusable project configurations
- `qa_model_templates` - QA rule templates
- `payable_rate_templates` - Pricing templates
- `mt_qe_templates` - MTQE workflow templates

## Plugin Architecture

The application supports plugins for extended functionality:
- Plugins located in `plugins/` directory
- Each plugin has its own test suite
- Feature flags control plugin activation
- Examples: ReviewExtended, ProjectCompletion, TranslationVersions

## Real-time Features

### Node.js Server
- Socket.io server in `nodejs/`
- Real-time segment updates
- User presence indicators
- Collaborative editing support

### Queue System
- ActiveMQ for background processing
- File conversion jobs
- Analysis and word count calculations
- Email notifications

## Security Considerations

- JWT-based authentication
- OAuth integration with sanitized tokens
- Input validation using JSON schemas
- File upload security measures
- API rate limiting
- CORS configuration

## Performance Optimization

- Redis caching for frequently accessed data
- Database query optimization
- Asset compilation and minification
- CDN integration for static assets
- Background job processing for heavy operations

## Debugging and Logging

- Monolog for structured logging
- Debug mode configuration
- Error reporting levels
- Performance monitoring capabilities

## External Integrations

### Translation Services
- Google Translate, DeepL, Microsoft Hub
- ModernMT, Apertium, Yandex
- Custom engine integration support

### File Processing
- External filters service via RapidAPI
- XLIFF processing and validation
- Metadata extraction

### Quality Assurance
- LexiQA integration
- Custom QA rule engine
- Review workflow automation

## Common Development Patterns

### Database Access
- Use DAO pattern for database operations
- Extend `AbstractDao` for new entities
- Use transactions for multi-table operations

### API Development
- Follow RESTful conventions
- Use JSON schema validation
- Implement proper error handling
- Add rate limiting for public endpoints

### Frontend Development
- Use React hooks pattern
- Follow Semantic UI React conventions
- Implement proper state management
- Add Jest tests for new components

### File Processing
- Use existing file utility classes
- Implement proper error handling
- Support metadata extraction
- Handle large file uploads efficiently

## Migration and Deployment

### Database Migrations
- Use Phinx for database schema changes
- Include rollback methods
- Test migrations thoroughly
- Document schema changes

### Asset Deployment
- Use webpack for asset compilation
- Different configs for dev/prod
- Optimize for performance
- Handle cache busting

This documentation should help you understand the codebase structure and development workflows for effective contributions to the Matecat translation platform.