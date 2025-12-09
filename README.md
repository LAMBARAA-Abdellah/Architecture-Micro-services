# 📘 TP – Architecture Micro‑services (Spring Boot + Spring Cloud + Angular)

## 🎯 Objectif  
Créer une application complète basée sur une architecture **micro‑services** permettant de gérer :  
- Les **clients**  
- Les **produits**  
- Les **factures** associées à un client et contenant une liste de produits  

L’architecture respecte les standards professionnels : **Spring Cloud Gateway**, **Eureka Discovery**, **OpenFeign**, **Spring Cloud Config**, et un **client Angular**.

---
# 🏗️ **Structure du Projet**

```
micro-services-app/
│
├── customer-service/      → Gestion des clients
├── inventory-service/     → Gestion des produits
├── billing-service/       → Gestion des factures (Feign)
│
├── eureka-discovery/      → Annuaire des micro-services
├── gateway-service/       → API Gateway + Routing dynamique
│
├── config-service/        → Spring Cloud Config Server
├── config-repo/           → Fichiers de configuration Git
│
└── angular-client/        → Interface Web Angular
```

---

# 📌 1. **Customer-Service (Spring Boot)**  
### Fonctionnalités :
- CRUD complet des clients  
- Base : H2 ou MySQL  
- Endpoints `/customers`  
- Exposition au gateway via Eureka  

---

# 📌 2. **Inventory-Service (Spring Boot)**  
### Fonctionnalités :
- CRUD complet des produits  
- Base : H2 ou MySQL  
- Endpoints `/products`  

---

# 📌 3. **Gateway – Spring Cloud Gateway**  
### Configuration statique :
```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: customer-service
          uri: http://localhost:8081
          predicates:
            - Path=/customers/**
        - id: inventory-service
          uri: http://localhost:8082
          predicates:
            - Path=/products/**
```

### Configuration dynamique via Eureka :
```yaml
spring:
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true
          lowerCaseServiceId: true
```

---

# 📌 4. **Eureka Discovery Service**  
- Centralise les micro‑services  
- Permet le load‑balancing et la configuration dynamique du routage  

---

# 📌 5. **Billing-Service**  
### Fonctionnalités :  
- Générer une facture appartenant à un client  
- Ajouter une liste d’items contenant des produits  
- Consommer customer-service et inventory-service avec **OpenFeign**

### Exemple d’un client Feign :
```java
@FeignClient(name = "CUSTOMER-SERVICE")
public interface CustomerRestClient {
    @GetMapping("/customers/{id}")
    Customer getCustomer(@PathVariable Long id);
}
```

---

# 📌 6. **Config Service (Spring Cloud Config)**  
### Contenu du config-repo (dans un repo Git) :
```
customer-service.yml
inventory-service.yml
billing-service.yml
gateway-service.yml
```

### Exemple customer-service.yml :
```yaml
server:
  port: 8081

spring:
  datasource:
    url: jdbc:h2:mem:customers-db
```

---

# 📌 7. **Client Angular**  
### Fonctionnalités :
- Afficher liste des clients  
- Afficher liste des produits  
- Afficher les factures  
- Appels via **Gateway**  
- Modules Angular professionnels  

---

# ✅ **Livrables attendus**

✔ Customer-Service  
✔ Inventory-Service  
✔ Billing-Service (Feign)  
✔ Gateway (statique + dynamique)  
✔ Eureka  
✔ Config-Service + repo Git  
✔ Client Angular  
✔ README pro (ce fichier)

---

# ✨ Auteur  
**LAMBARAA Abdellah – BDCC / ENSET Mohammedia**  
---

