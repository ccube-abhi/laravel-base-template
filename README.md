# 🧬 Biolyer App - Laravel 11 + Docker + Redis + MySQL

A modern Laravel 11 application pre-configured with Docker, Redis, MySQL, and essential development tools like Larastan, Laravel Pint, Scramble, and Git Hooks.

---

## 🚀 Prerequisites

- Docker & Docker Compose
- Git
---

## 📦 Project Setup (Using Docker)

### 1. Clone Repository
git clone https://github.com/ccube-abhi/laravel-base-template
cd laravel-base-template

### 2. Environment Setup
cp .env.example .env

DB_CONNECTION=mysql
DB_HOST=
DB_PORT=3306
DB_DATABASE=
DB_USERNAME=
DB_PASSWORD=


### 3. Build and Start Docker Containers
docker compose up -d --build

### 4. Laravel Setup (Inside Docker Container, for localhost setup, we can move it into Dockerfile)

# Run migrations and seeders
docker compose exec base-temp php artisan migrate
docker compose exec base-temp php artisan db:seed

### 5. Test Endpoints
Redis Test
GET http://localhost:8000/redis-test

Register User
POST http://localhost:8000/api/register

### 6. Run Unit Tests
docker compose exec base-temp php artisan test

### 7. Code Quality Tools
Laravel Pint (PSR-12 Formatter)
# Auto-format your code
# Optionally configure .pint.json for custom rules.
vendor/bin/pint

### 8. Larastan (Static Analysis)
# Analyze your Laravel app
vendor/bin/phpstan analyse --memory-limit=512M app


### 11. API Documentation using Scramble
# Usage Instructions
# Add comments using PHPDoc format above your controller methods.
# Scramble will auto-generate OpenAPI spec and UI.
Example:
/**
 * Register a new user.
 */
public function register(RegisterRequest $request)
{
    //code...
}

### 12. Generate API Docs
    php artisan scramble:analyze
    php artisan scramble:generate
    Hit on local URL Example http://localhost/docs/api#/

# Export OpenAPI schema to a JSON api.json file:
# php artisan scramble:export

### 13. Git Hooks with igorsgm/laravel-git-hooks
This project uses igorsgm/laravel-git-hooks to automatically enforce code standards and quality checks before every commit.
🔧 What Happens on Every Commit
When you run git commit, the following actions are automatically triggered:
✅ Laravel Pint — auto-formats code
✅ Optional: Add your own custom scripts in config/git-hooks.php
To manually test the hook logic:
 ./vendor/bin/git-hooks run
