# Deploy Firebase Python Functions

A GitHub Action for deploying Python-based Firebase Cloud Functions. This repository serves as both the action implementation and a live example of its usage.

## Features

- 🐍 Python dependency management from pyproject.toml
- 🔐 Secure handling of service account credentials
- Detailed deployment summaries

## Usage

Basic usage:

```yaml
- uses: digital-wisdom/deploy-firebase-python@v1
  with:
    project_id: my-firebase-project
    service_account_json: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
    deploy_command: "firebase deploy --only functions"
```

## Examples

### Basic Firebase deployment:
```yaml
- uses: digital-wisdom/deploy-firebase-python@v1
  with:
    project_id: my-firebase-project
    service_account_json: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
```

### Deploy specific functions:
```yaml
- uses: digital-wisdom/deploy-firebase-python@v1
  with:
    project_id: my-firebase-project
    service_account_json: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
    deploy_command: "firebase deploy --only functions:api,functions:webhook"
```

### Using make for deployment:
```yaml
- uses: digital-wisdom/deploy-firebase-python@v1
  with:
    service_account_json: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
    deploy_command: "make deploy-staging"
```

### Custom functions directory:
```yaml
- uses: digital-wisdom/deploy-firebase-python@v1
  with:
    service_account_json: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
    functions_dir: "backend/functions"
    project_id: my-firebase-project
```

## Inputs

| Input                 | Description                                       | Required | Default                        |
| --------------------- | ------------------------------------------------- | -------- | ------------------------------ |
| `service_account_json`| Firebase service account JSON                     | Yes      | N/A                            |
| `deploy_command`      | Deployment command to execute                     | No       | `firebase deploy --only functions` |
| `project_id`          | Firebase project ID (runs `firebase use` if specified) | No       | N/A                            |
| `functions_dir`       | Directory with pyproject.toml (creates requirements.txt if missing) | No       | `functions`                    |

## Prerequisites

- Firebase service account key
- requirements.txt or pyproject.toml in functions_dir

## Environment Variables

The action automatically sets up:

- `GOOGLE_APPLICATION_CREDENTIALS`: Points to the service account file
- Firebase project configuration via `firebase use`

## Security

- Service account credentials are securely handled and cleaned up
- Credentials file is automatically removed after deployment
- Uses GitHub's secure token handling

## Versioning

This action follows semantic versioning. You can use it in three ways:

- Major version: `@v1` - Recommended for stability
  ```yaml
  - uses: digital-wisdom/deploy-firebase-python@v1
  ```
- Specific version: `@v1.0.0` - Pinned to exact version
  ```yaml
  - uses: digital-wisdom/deploy-firebase-python@v1.0.0
  ```
- Branch: `@main` - Latest changes (not recommended for production)
  ```yaml
  - uses: digital-wisdom/deploy-firebase-python@main
  ```

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Use [Conventional Commits](https://www.conventionalcommits.org/) for commit messages:

   - `feat: add new feature`
   - `fix: resolve bug`
   - `docs: update documentation`
   - `chore: maintenance tasks`

2. Changes will automatically:
   - Generate release notes
   - Update the changelog
   - Create a new version
   - Update major version tag

## License

This project is licensed under the MIT License - see the LICENSE file for details.
