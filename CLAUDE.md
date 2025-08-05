# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a GitHub Action for deploying Python-based Firebase Cloud Functions. The repository serves dual purposes:
1. GitHub Action implementation (`action.yml`)
2. Live example Firebase function (`functions/`)

## Key Commands

### Development Commands
```bash
# Install Python dependencies (in functions/ directory)
cd functions && uv sync

# Generate requirements.txt for Firebase deployment
cd functions && uv pip freeze > requirements.txt

# Start Firebase emulators for local development
firebase emulators:start

# Deploy to Firebase (default)
firebase deploy --only functions

# Deploy specific functions
firebase deploy --only functions:api,functions:webhook

# Deploy using make
make deploy-staging

# Select Firebase project
firebase use <project_id>
```

### Testing and Validation
```bash
# Test the action locally (requires service account)
firebase deploy --only functions --debug

# List available Firebase projects
firebase projects:list

# Check Firebase emulator status
firebase emulators:start --only functions,auth
```

## Architecture

### GitHub Action Structure
- `action.yml`: Composite action definition with steps for deployment
- Uses `uv` for Python dependency management
- Integrates with `google-github-actions/auth@v2` for authentication
- Supports configurable deployment commands via `deploy_command` input
- Can execute Firebase CLI commands or make targets

### Firebase Function Structure
- `functions/main.py`: Simple Flask-based HTTP function
- `functions/pyproject.toml`: Python dependencies (firebase-admin, firebase-functions, flask)
- `firebase.json`: Firebase configuration with Python 3.11 runtime
- Emulator ports: Functions (5001), Auth (9099)

### Key Dependencies
- Python 3.10-3.11 (Firebase Functions constraint)
- `uv` package manager for dependency management  
- Firebase CLI for deployment
- Node.js 20 for Firebase CLI

## Development Workflow

1. Modify functions in `functions/main.py`
2. Update dependencies in `functions/pyproject.toml` if needed
3. Run `uv sync` to update lockfile
4. Test locally with Firebase emulators
5. Deploy with `firebase deploy --only functions`

## Security Notes

- Service account credentials are handled securely via GitHub Actions
- Credentials are automatically cleaned up after deployment
- Uses Application Default Credentials (ADC) pattern