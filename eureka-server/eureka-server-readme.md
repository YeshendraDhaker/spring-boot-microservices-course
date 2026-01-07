# Eureka Server - Service Discovery

## Overview
Eureka Server acts as a service registry where all microservices register themselves. Other services can discover and communicate with each other through this registry.

## Features
- Service registration and discovery
- Health checking
- Load balancing information
- Dashboard for monitoring

## Running the Server

### Using Maven
```bash
cd eureka-server
mvn spring-boot:run
```

### Using Java
```bash
cd eureka-server
mvn clean package
java -jar target/eureka-server-1.0.0.jar
```

## Accessing the Dashboard

Once started, open your browser and navigate to:
```
http://localhost:8761
```

You should see the Eureka Dashboard showing:
- Registered instances (currently none)
- General information
- Instance status

## Health Check

Check if Eureka is running:
```
http://localhost:8761/actuator/health
```

## Next Steps
1. Create additional microservices (user-service, product-service, etc.)
2. Register those services with this Eureka Server
3. Services will automatically discover each other
```

---

## 🧪 Testing & Verification

### Step 1: Build the Project

```bash
# From the root directory (amazon2-ecommerce)
mvn clean install
```

**Expected Output:**
```
[INFO] Building Amazon2 E-Commerce Platform 1.0.0
[INFO] Building Eureka Server 1.0.0
[INFO] BUILD SUCCESS
```

### Step 2: Run Eureka Server

**Option 1: Using Maven**
```bash
cd eureka-server
mvn spring-boot:run
```

**Option 2: Using JAR**
```bash
cd eureka-server
java -jar target/eureka-server-1.0.0.jar
```

### Step 3: Verify Startup

**Console Output Should Show:**
```
🚀 EUREKA SERVER STARTED SUCCESSFULLY 🚀
Dashboard: http://localhost:8761
```

### Step 4: Access Eureka Dashboard

Open browser: **http://localhost:8761**

**You should see:**
- Eureka Dashboard with system status
- "Instances currently registered with Eureka": 0 (none yet)
- "General Info" section
- "Instance Info" section

### Step 5: Test Health Endpoint

Open browser or use curl: **http://localhost:8761/actuator/health**

**Expected Response:**
```json
{
  "status": "UP",
  "components": {
    "diskSpace": {
      "status": "UP"
    },
    "ping": {
      "status": "UP"
    }
  }
}
```

---

## 🏗️ Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     AMAZON2 E-COMMERCE                      │
│                    (After Module 2)                         │
└─────────────────────────────────────────────────────────────┘

                    ┌──────────────────┐
                    │  Eureka Server   │
                    │  (Port: 8761)    │
                    │                  │
                    │  - Dashboard     │
                    │  - Registry      │
                    │  - Health Check  │
                    └──────────────────┘
                            │
                            │ (Future Modules)
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
  ┌──────────┐        ┌──────────┐       ┌──────────┐
  │  Config  │        │   API    │       │  User    │
  │  Server  │        │ Gateway  │       │ Service  │
  └──────────┘        └──────────┘       └──────────┘
   (Module 3)          (Module 4)         (Module 5)
```

---

## 📚 Understanding Service Discovery

### What Problem Does Eureka Solve?

**Without Service Discovery:**
```
User Service needs to call Product Service
❌ Hard-coded IP: http://192.168.1.10:8080/products
❌ Problems:
   - IP changes? Application breaks!
   - Load balancing? Manually configure!
   - Multiple instances? Hard to manage!
```

**With Service Discovery (Eureka):**
```
User Service needs to call Product Service
✅ Service Name: http://product-service/products
✅ Benefits:
   - Eureka resolves to actual IP
   - Automatic load balancing
   - Dynamic service updates
   - Health monitoring
```

### How It Works:

1. **Service Registration:**
   ```
   Product Service starts → Registers with Eureka
   User Service starts → Registers with Eureka
   Cart Service starts → Registers with Eureka
   ```

2. **Service Discovery:**
   ```
   User Service: "Hey Eureka, where is Product Service?"
   Eureka: "Product Service is at 192.168.1.10:8080"
   User Service: Makes call to Product Service
   ```

3. **Health Monitoring:**
   ```
   Eureka: Sends heartbeat every 30 seconds
   Service: Responds with "I'm alive"
   No response? Eureka removes service from registry
   ```
