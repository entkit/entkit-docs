---
sidebar_position: 1
---

# Keycloak

Keycloak is an open source Identity and Access Management tool with features such as Single-Sign-On (SSO), Identity Brokering and Social Login, User Federation, Client Adapters, an Admin Console, and an Account Management Console.

[Check Keycloak Site](https://www.keycloak.org/)

## How EntKit integrating Keycloak

EntKit generates both backend and frontend clients, complete with authorization resources, permissions, policies, and roles, streamlining user management. To enforce backend security, EntKit utilizes resource and scope verification.

## Keycloak usage example

```go title="entc.go" {3-27}
entkit, err := entkit.NewExtension(
    entkit.WithAuth(
        entkit.AuthWithKeycloak(
            entkit.KeycloakHost("http://localhost:8080"),
            entkit.KeycloakRealm("entkit-demo-3"),
            entkit.KeycloakMasterAdminCredentials("admin", "admin"),
            entkit.KeycloakGeneratedAdminCredentials("entadmin", "entadmin"),
            entkit.KeycloakFrontendClientConfig(gocloak.Client{
                ClientID: gocloak.StringP("frontend"),
                RootURL:  gocloak.StringP("https://demo.entkit.com"),
                RedirectURIs: &[]string{
                    "https://demo.entkit.com/*",
                    "http://localhost:3000/*",
                    "http://localhost/*",
                },
                Attributes: &map[string]string{
                    "post.logout.redirect.uris": "+",
                },
                WebOrigins: &[]string{
                    "+",
                },
            }),
            entkit.KeycloakBackendClientConfig(gocloak.Client{
                ClientID: gocloak.StringP("backend"),
                Secret:   gocloak.StringP("test-secret"),
            }),
        ),
    ),
)
```

## Run Keycloak on local machine

To run Keycloak on your local Docker environment

Create new `docker-compose.yaml` file or add `keycloak` service on your existing file.

```shell title="docker-compose.yaml" {4-13}
version: "3.9"

services:
  keycloak:
    command: [ "start-dev" ]
    image: quay.io/keycloak/keycloak:21.0.0
    volumes:
      - keycloak_data:/opt/keycloak/data/
    environment:
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
    ports:
      - "8080:8080"
volumes:
  keycloak_data:
```

### Run docker compose up command to run service
```shell
docker compose up -d
```

Then try to access keycloak UI on your browser [http://localhost:8080](http://localhost:8080)