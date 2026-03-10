# Infrastructure

Infrastructure repository for the Buried Marks application. Contains Docker Compose configurations and nginx routing.

## Repository Structure

```
buried-marks-infrastructure/

├── docker-compose.yml          # Production setup with ECR images
├── docker-compose-local.yml    # Local development setup with local builds
├── nginx/
│   └── nginx.conf              # Nginx reverse proxy configuration
├── secrets/
│   ├── jwt_public_key/         # EC public key for JWT verification
│   └── jwt_private_key/        # EC private key for JWT signing
├── .env.auth                   # Auth service environment variables
├── .env.map                    # Map service environment variables
├── .env.mail                   # Mail service environment variables
└── .env.voting                 # Voting service environment variables
```

## Environment Variables

Each service has its own .env file located in the root of this repository. These files are not committed to git - request them from a team member.

| File        | Service                     |
|-------------|-----------------------------|
| .env.auth   | auth-service, auth-db       |
| .env.map    | map-service, map-service-db |
| .env.mail   | mail-service                |
| .env.voting | voting-service, voting-db   |

Copy the example files and fill in the values:

```bash
cp .env.auth.example .env.auth
cp .env.map.example .env.map
cp .env.mail.example .env.mail
cp .env.voting.example .env.voting
```

## Secrets

Place the JWT key files in the secrets/ directory before starting the application:
```
secrets
├── jwt_private_key
│   └── ec_private.key
└── jwt_public_key
    └── ec_public.key
```

## Running the Application

- **Local development** (builds images from source)
    Requires all microservice repositories to be cloned alongside this repository.

    ```
    docker compose -f docker-compose-local.yml up
    ```


- **Production** (pulls images from AWS ECR)
Requires AWS ECR authentication before starting:

    ```
    aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 531331080468.dkr.ecr.eu-north-1.amazonaws.com
    ```

    ```
    docker compose -f docker-compose.yml up
    ```

## Nginx

All traffic enters through nginx on port 80 and is routed to the appropriate service based on the URL path.
