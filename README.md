# TP 19 - Orchestration de microservices avec Spring Cloud Eureka, Gateway et OpenFeign

## Architecture
- **Eureka Server** (port 8761) - Service Discovery Registry
- **SERVICE-CLIENT** (port 8088) - Client microservice with H2 database
- **SERVICE-VOITURE** (port 8089) - Car microservice with OpenFeign integration
- **Gateway** (port 8888) - API Gateway with dynamic routing

## Démarrage des services

### 1. Démarrer Eureka Server
```bash
cd eureka-server
mvn spring-boot:run
```
Accéder à: http://localhost:8761/

### 2. Démarrer SERVICE-CLIENT
```bash
cd service-client
mvn spring-boot:run
```

### 3. Démarrer SERVICE-VOITURE
```bash
cd service-voiture
mvn spring-boot:run
```

### 4. Démarrer la Gateway
```bash
cd gateway
mvn spring-boot:run
```

## Tests des endpoints

### Via Gateway (port 8888)
- **Clients**: http://localhost:8888/clients
- **Client by ID**: http://localhost:8888/client/1
- **Voitures**: http://localhost:8888/voitures
- **Voiture by ID**: http://localhost:8888/voitures/1
- **Voitures by Client**: http://localhost:8888/voitures/client/1

### Directement sur les microservices
- **SERVICE-CLIENT**: http://localhost:8088/clients
- **SERVICE-VOITURE**: http://localhost:8089/voitures

## Vérification Eureka
Accéder à http://localhost:8761/ pour vérifier que:
- SERVICE-CLIENT est enregistré
- SERVICE-VOITURE est enregistré
- Gateway est enregistrée

## Base de données H2
- **SERVICE-CLIENT**: http://localhost:8088/h2-console
  - JDBC URL: jdbc:h2:mem:testdb
  - Username: sa
  - Password: (vide)

- **SERVICE-VOITURE**: http://localhost:8089/h2-console
  - JDBC URL: jdbc:h2:mem:testdb
  - Username: sa
  - Password: (vide)

## Données initiales
### Clients (SERVICE-CLIENT)
1. Rabab SELIMANI (23 ans)
2. Amal RAMI (22 ans)
3. Samir SAFI (22 ans)

### Voitures (SERVICE-VOITURE)
1. Toyota Corolla (A 25 333) - Client: Amal RAMI
2. Renault Megane (B 6 3456) - Client: Amal RAMI
3. Peugeot 301 (A 55 4444) - Client: Rabab SELIMANI

## Exemples de requêtes

### GET all voitures with client data
```bash
curl http://localhost:8888/voitures
```

### GET voiture by ID with client data
```bash
curl http://localhost:8888/voitures/1
```

### POST new voiture
```bash
curl -X POST http://localhost:8888/voitures/1 \
  -H "Content-Type: application/json" \
  -d '{"marque":"BMW","matricule":"C 123 456","model":"X5"}'
```

### PUT update voiture
```bash
curl -X PUT http://localhost:8888/voitures/1 \
  -H "Content-Type: application/json" \
  -d '{"marque":"Mercedes","matricule":"D 789 012","model":"C-Class"}'
```

### DELETE voiture
```bash
curl -X DELETE http://localhost:8888/voitures/1
```
