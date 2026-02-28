# 📦 Microservicios con Consul + Docker

Sistema de gestión basado en arquitectura de microservicios usando:

- Python + Flask
- SQLAlchemy
- Docker
- Docker Compose
- Consul (Service Discovery)

---

# 🏗 Arquitectura

El sistema está compuesto por los siguientes servicios:

| Servicio        | Descripción              | Puerto |
|----------------|--------------------------|--------|
| microUsers     | Gestión de usuarios      | 5002   |
| microProductos | Gestión de productos     | 5003   |
| microOrders    | Gestión de órdenes       | 5004   |
| frontend       | Interfaz web             | 5001   |
| consul         | Service Discovery        | 8500   |

Cada microservicio tiene su propia base de datos independiente:

- db_users
- db_products
- db_orders

---

# 🔎 Service Discovery (Consul)

Se utiliza **Consul** para:

- Registro automático de servicios
- Descubrimiento dinámico entre microservicios
- Health checks

Cada servicio se registra con:

- `name`
- `address`
- `port`
- `healthcheck` (`/healthcheck`)

Panel de Consul disponible en: http://localhost:8500


---

# 🐳 Docker

Cada microservicio tiene su propio:

- Dockerfile
- requirements.txt

Todos los servicios se comunican dentro de la red interna de Docker usando el nombre del servicio como hostname.

Ejemplo de comunicación interna: http://microProductos:5003/productos/1


---

# 🚀 Cómo Ejecutar el Proyecto

## 1️⃣ Clonar el repositorio

## 2️⃣ Levantar todos los servicios 

Si usas Docker Compose moderno:

docker compose -f services.yml up --build -d


---

# 🔁 Flujo de Comunicación

1. El frontend envía solicitud para crear orden.
2. microOrders:
   - Consulta microUsers vía Consul.
   - Consulta microProductos vía Consul.
3. Valida existencia y stock.
4. Calcula el total.
5. Guarda la orden.
6. Devuelve la respuesta al frontend.

Todo el descubrimiento se hace dinámicamente usando Consul.

---

# 📂 Estructura del Proyecto

├── db_orders
├── db_products
├── db_users
├── frontend
├── microOrders
├── microProductos
├── microUsers
├── services.yml
└── README.md


---

# ❤️ Endpoints Principales

## Users
- GET `/api/users`
- GET `/api/users/<id>`
- POST `/api/users`

## Products
- GET `/products`
- GET `/products/<id>`
- POST `/products`

## Orders
- GET `/api/orders`
- POST `/api/orders`

---

# 🛠 Requisitos

- Docker
- Docker Compose

No es necesario instalar Python manualmente.

---

# 🏆 Características Técnicas

- Arquitectura desacoplada
- Base de datos independiente por servicio
- Comunicación REST
- Service Discovery dinámico
- Health checks automáticos
- Contenedorización completa

---

# 📌 Notas

- Los servicios dependen de Consul para descubrimiento.
- Si un servicio no pasa el health check, no será visible para otros.
- Todo corre dentro de la red interna de Docker.

