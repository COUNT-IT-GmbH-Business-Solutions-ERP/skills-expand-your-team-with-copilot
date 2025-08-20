# Mergington High School Activities Management System

Always follow these instructions first and only fallback to additional search and context gathering if the information in these instructions is incomplete or found to be in error.

## Project Overview
FastAPI web application for managing high school extracurricular activities. Students can view activities, teachers can log in to manage enrollments. Uses Python 3.12+, FastAPI, MongoDB, and vanilla HTML/CSS/JavaScript frontend.

## Working Effectively

### Bootstrap and Setup (First Time)
Run these commands in order from the repository root:

```bash
# Install Python dependencies - takes ~15 seconds
pip install -r src/requirements.txt

# Start MongoDB using Docker - takes ~10 seconds to download first time, ~3 seconds after
docker run -d --name mergington-mongo -p 27017:27017 mongo:7.0

# Verify MongoDB is running
docker ps | grep mergington-mongo
```

### Run the Application
```bash
# Start the FastAPI application with auto-reload - starts in ~3 seconds
uvicorn src.app:app --reload

# Application will be available at:
# - Main app: http://localhost:8000/static/index.html  
# - API docs: http://localhost:8000/docs
# - API root: http://localhost:8000/activities/
```

### Stop the Application
```bash
# Stop the FastAPI application
# Press Ctrl+C in the terminal running uvicorn

# Stop and clean up MongoDB container
docker stop mergington-mongo && docker rm mergington-mongo
```

## Validation
Always validate your changes using these complete scenarios:

### Scenario 1: Full Application Flow
1. Start MongoDB and application using commands above
2. Navigate to http://localhost:8000/static/index.html
3. Verify activities load (should see 12 activities including Chess Club, Programming Class, Soccer Team)
4. Test filtering: click "Sports" button - should show only 3 sports activities
5. Test search: type "chess" in search box - should filter to chess-related activities
6. Test login: click login button, verify modal appears with username/password fields

### Scenario 2: API Functionality
```bash
# Test activities endpoint - should return JSON with activity data
curl -s http://localhost:8000/activities/ | head -20

# Test API documentation - should return HTML page
curl -s http://localhost:8000/docs | head -10

# Verify redirect works
curl -I http://localhost:8000/activities
```

### Scenario 3: Teacher Login Test
Use these test accounts (from src/backend/database.py):
- Username: `mrodriguez`, Password: `art123` (Ms. Rodriguez)
- Username: `mchen`, Password: `chess456` (Mr. Chen)  
- Username: `principal`, Password: `admin789` (Principal Martinez)

## Critical Dependencies
- **MongoDB is REQUIRED**: Application fails to start without MongoDB connection
- **Docker must be available**: Used for MongoDB in development
- **Python 3.12+**: Required for dependencies
- **Port 8000**: Must be available for the application

## Repository Structure
```
src/
├── app.py              # FastAPI application entry point
├── requirements.txt    # Python dependencies
├── backend/
│   ├── database.py     # MongoDB configuration and sample data
│   └── routers/        # API endpoint definitions
└── static/
    ├── index.html      # Main frontend page
    ├── app.js          # Frontend JavaScript
    └── styles.css      # Frontend styling
```

## Development Notes
- FastAPI auto-reload is enabled - changes to Python files automatically restart the server
- All data is stored in MongoDB - data persists between application restarts but not container restarts
- No test suite exists - validate changes manually using the scenarios above
- No linting configuration found - follow existing code style
- VS Code debug configuration available: "Launch Mergington WebApp"

## Troubleshooting
- **"Connection refused" error**: MongoDB is not running - start with Docker command above
- **"Import error" when running `python src/app.py`**: Use `uvicorn src.app:app --reload` instead
- **Activities not loading in browser**: Check MongoDB is running and application started successfully
- **Login fails with valid credentials**: Normal behavior - authentication system may need debugging

## Common Tasks
- **Add new activity**: Edit `initial_activities` in `src/backend/database.py`
- **Change frontend styling**: Edit `src/static/styles.css`
- **Add new API endpoint**: Create new router in `src/backend/routers/`
- **Modify database**: Edit `src/backend/database.py`

Always test the complete application flow after making changes to ensure functionality is preserved.
