# TixManager - Event Ticket Management System

## 📌 Description
A web-based system for ticket sales and management with:
- User roles (Normal/Administrator)
- Real-time availability management
- Protection against overselling with concurrency handling

## 🛠️ Technologies
| **Technology**      | **Version** | **Usage**                        |
|---------------------|-------------|----------------------------------|
| Java                | 21+         | Base language of the system      |
| Spring Boot         | 3.x         | Backend framework                 |
| Thymeleaf           | 3.1         | HTML template engine             |
| Bootstrap           | 5.3         | Responsive design                |
| MySQL               | 8.0+        | Relational database              |
| Spring Security     | 6.x         | Authentication and authorization |

## 🏗️ Architecture
→ Client (Browser)
<br>
→ Spring Boot Server (MVC)
<br>
→ MySQL Database


### Package Structure
```
src/
├── main/
│ ├── java/
│ │ └── com/example/
│ │ ├── config/ # Configuration
│ │ ├── controller/ # Navigation logic
│ │ ├── model/ # Entities
│ │ ├── repository/ # Data access
│ │ └── Application.java
│ └── resources/
│ ├── static/ # CSS/JS
│ └── templates/ # Views
```


## 🔑 Main Features
### 1. Authentication
- User login/registration
- Default `ADMIN` role created:
    ```plaintext
    Email: admin@admin.com
    Password: admin123
    ```


### 2. Event Management (CRUD)
- Accessible only to administrators
- Endpoints:
- `GET /admin/eventos` - List events
- `POST /admin/eventos/guardar` - Create/edit
- `GET /admin/eventos/eliminar/{id}` - Delete

### 3. Ticket Purchase
```java
@Transactional
public synchronized boolean comprarBoletos(...) {
// Logic with concurrency handling
}
```

## 🚀 How to Run

1. Requirements:
   - JDK 21+
   - MySQL 8.0+
   - Maven
2. Configuration
    ```properties
    # src/main/resources/application.properties
    spring.datasource.url=jdbc:mysql://localhost:3306/sistema_boletos
    spring.datasource.username=username
    spring.datasource.password=password
    ```
3. Create the database
   ```sql
   CREATE DATABASE sistema_boletos;
   ```

4. Commands
    ```bash
    mvn spring-boot:run
    ```

## ✅ Requirements Met

| **Category**         | **Requirement**                     | **Implementation**                                                                 | **Evidence**                                                                 |
|----------------------|-------------------------------------|------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Architecture**     | Client-Server                       | Spring Boot (backend) + Thymeleaf/Bootstrap (frontend)                             | HTTP requests, clear separation of layers                                     |
| **Concurrency**      | Thread handling                     | `@Transactional` + `synchronized` in `EventoServiceImpl.comprarBoletos()`          | [See code](src/main/java/com/example/sistemaboletos/model/servicio/EventoServiceImpl.java)                                                               |
| **Security**         | Authentication                      | Spring Security with roles                                                          | Protection of `/admin/**` routes                                              |
| **Persistence**      | Complete CRUDs                      | 4 CRUDs: Users, Events, Purchases, Tickets (management)                            | JPA repositories                                                              |
| **Validations**      | Exception handling                  | Error control in purchases + Spring Security exceptions                             | Messages in views (`login?error`)                                             |
| **Data Structures**  | Generic collections                 | Use of `List<T>` (JPA), `Optional<T>` (queries), implicit `Map` (Security)         | Methods `findAll()`, `findByEmail()`                                          |
| **OOP**              | Abstract class                      | `EntidadBase` (inheritance for all entities)                                        | Centralizes `id` field                                                        |
|                      | Enum                                | `Rol` (USER, ADMIN)                                                                | Definition of system roles                                                    |
|                      | Interface                           | `IEventoService` (contract for event services)                                      | Implemented in `EventoServiceImpl`                                            |


