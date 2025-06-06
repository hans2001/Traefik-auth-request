# Traefik Auth Request Demo

This project demonstrates how to use Traefik's ForwardAuth middleware to authenticate requests based on HTTP headers, replacing the previous Nginx auth_request implementation.

## Architecture

The setup consists of two services:

1. **Auth**: A Flask service that validates the `x-pretest` header.
2. **Traefik**: Acts as a gateway, using ForwardAuth middleware to validate incoming requests.

## How it works

1. The client sends requests to Traefik with or without the `x-pretest` header
2. Traefik forwards the request headers to the auth service via ForwardAuth middleware
3. The auth service checks if the `x-pretest` header contains a valid token
4. If authentication succeeds (200 response), Traefik routes to the backend service; otherwise, it returns a 401 error

## Configuration Approach

Following production Kubernetes standards:
- File-based YAML configuration (not Docker labels)
- IngressRoute-style structure for easy migration to Kubernetes
- Dynamic configuration without restart requirements

## Valid Authentication

A valid request must include the header: `x-pretest: valid-token`

## Running the Demo

```bash
docker-compose up --build
```

## Testing 

You can test manually:

```bash
# Valid request
curl -H "x-pretest: valid-token" http://localhost:8080

# Invalid request
curl -H "x-pretest: wrong-token" http://localhost:8080

# Missing header
curl http://localhost:8080
``` 