# Sistema de Gestión de Boletos para Eventos

## 📌 Descripción
Sistema web para compra/venta de boletos con:
- Roles de usuario (Normal/Administrador)
- Gestión en tiempo real de disponibilidad
- Protección contra sobreventa con manejo de concurrencia

## 🛠️ Tecnologías
| **Tecnología**       | **Versión** | **Uso**                          |
|----------------------|-------------|-----------------------------------|
| Java                 | 21+         | Lenguaje base del sistema         |
| Spring Boot          | 3.x         | Framework backend                 |
| Thymeleaf            | 3.1         | Motor de plantillas HTML          |
| Bootstrap            | 5.3         | Diseño responsive                 |
| MySQL                | 8.0+        | Base de datos relacional          |
| Spring Security      | 6.x         | Autenticación y autorización      |

## 🏗️ Arquitectura
→ Cliente (Navegador)
<br>
→ Servidor Spring Boot (MVC)
<br>
→ Base de Datos MySQL


### Estructura de paquetes
```
src/
├── main/
│ ├── java/
│ │ └── com/example/
│ │ ├── config/ # Configuración
│ │ ├── controller/ # Lógica de navegación
│ │ ├── model/ # Entidades
│ │ ├── repository/ # Acceso a datos
│ │ └── Application.java
│ └── resources/
│ ├── static/ # CSS/JS
│ └── templates/ # Vistas
```


## 🔑 Funcionalidades principales
### 1. Autenticación
- Login/registro de usuarios
- Rol `ADMIN` creado por defecto:
    ```plaintext
    Email: admin@admin.com
    Contraseña: admin123
    ```


### 2. Gestión de Eventos (CRUD)
- Solo accesible para administradores
- Endpoints:
- `GET /admin/eventos` - Listar
- `POST /admin/eventos/guardar` - Crear/editar
- `GET /admin/eventos/eliminar/{id}` - Eliminar

### 3. Compra de Boletos
```java
@Transactional
public synchronized boolean comprarBoletos(...) {
// Lógica con manejo de concurrencia
}
```

## 🚀 Cómo ejecutar

1. Requisitos:
   - JDK 21+
   - MySQL 8.0+
   - Maven
2. Configuración
    ```properties
    # src/main/resources/application.properties
    spring.datasource.url=jdbc:mysql://localhost:3306/sistema_boletos
    spring.datasource.username=usuario
    spring.datasource.password=contraseña
    ```
3. Crear la base de datos
   ```sql
   CREATE DATABASE sistema_boletos;
   ```

4. Comandos
    ```bash
    mvn spring-boot:run
    ```

## ✅ Requisitos Cumplidos

| **Categoría**            | **Requisito**                          | **Implementación**                                                                 | **Evidencia**                                                                 |
|--------------------------|----------------------------------------|------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Arquitectura**         | Cliente-Servidor                       | Spring Boot (backend) + Thymeleaf/Bootstrap (frontend)                             | Peticiones HTTP, separación clara de capas                                    |
| **Concurrencia**         | Manejo de hilos                       | `@Transactional` + `synchronized` en `EventoServiceImpl.comprarBoletos()`          | [Ver código](#)                                                               |
| **Seguridad**            | Autenticación                          | Spring Security con roles                                                          | Protección de rutas `/admin/**`                                               |
| **Persistencia**         | CRUDs completos                        | 4 CRUDs: Usuarios, Eventos, Compras, Boletos (gestión)                            | Repositorios JPA                                                              |
| **Validaciones**         | Manejo de excepciones                 | Control de errores en compras + Spring Security exceptions                         | Mensajes en vistas (`login?error`)                                            |
| **Estructuras de Datos** | Colecciones genéricas                 | Uso de `List<T>` (JPA), `Optional<T>` (consultas), `Map` implícito (Security)      | Métodos `findAll()`, `findByEmail()`                                          |
| **POO**                  | Clase abstracta                       | `EntidadBase` (herencia para todas las entidades)                                  | Centraliza campo `id`                                                         |
|                          | Enum                                  | `Rol` (USUARIO, ADMIN)                                                            | Definición de roles del sistema                                               |
|                          | Interfaz                              | `IEventoService` (contrato para servicios de eventos)                              | Implementada en `EventoServiceImpl`                                           |


