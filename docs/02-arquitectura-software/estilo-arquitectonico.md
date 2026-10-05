## Diagrama del Estilo Arquitectónico

```mermaid
flowchart TB

%% =====================================================
%% ACTORES
%% =====================================================

Cliente[👤 Cliente]
Seller[👤 Seller]
Admin[👤 Administrador]

Cliente --> Web
Seller --> Web
Admin --> Web

Web["🌐 Cliente Web<br/>Navegador - HTML / CSS / JavaScript"]

Web -->|"HTTPS / JSON<br/>/api/v1/"| MW


%% =====================================================
%% MARKETPLACE BACKEND
%% =====================================================

subgraph Backend["Marketplace Backend - Node.js 20 LTS + Express<br/>Monolito modular"]

    direction TB

    MW["Middlewares Express<br/>CORS · express.json() · Auth JWT · Validación · Manejo de errores · Logger"]

    %% =================================================
    %% CAPA DE PRESENTACIÓN
    %% =================================================

    subgraph Presentacion["1. CAPA DE PRESENTACIÓN"]
        direction LR

        subgraph MUsuarios["Módulo Usuarios"]
            UR["usuarios.routes.js"]
            UC["usuarios.controller.js"]
            UR --> UC
        end

        subgraph MSellers["Módulo Sellers"]
            SR["sellers.routes.js"]
            SC["sellers.controller.js"]
            SR --> SC
        end

        subgraph MCatalogo["Módulo Catálogo"]
            CR["catalogo.routes.js"]
            CC["catalogo.controller.js"]
            CR --> CC
        end

        subgraph MCarrito["Módulo Carrito"]
            CAR["carrito.routes.js"]
            CAC["carrito.controller.js"]
            CAR --> CAC
        end

        subgraph MPedidos["Módulo Pedidos"]
            PR["pedidos.routes.js"]
            PC["pedidos.controller.js"]
            PR --> PC
        end
    end


    MW --> UR
    MW --> SR
    MW --> CR
    MW --> CAR
    MW --> PR


    %% =================================================
    %% CAPA DE LÓGICA DE NEGOCIO
    %% =================================================

    subgraph Negocio["2. CAPA DE LÓGICA DE NEGOCIO<br/>Reglas de negocio y coordinación entre módulos"]
        direction LR

        US["usuarios.service.js<br/>registro · login · roles"]
        SS["sellers.service.js<br/>alta de tiendas · validación"]
        CS["catalogo.service.js<br/>productos · categorías · stock"]
        CAS["carrito.service.js<br/>items · totales"]
        PS["pedidos.service.js<br/>checkout · estados · pago/envío"]
    end

    UC --> US
    SC --> SS
    CC --> CS
    CAC --> CAS
    PC --> PS


    %% Comunicación entre servicios
    SS -.-> US
    CS -.-> SS
    CAS -.-> CS
    PS -.-> CAS
    PS -.-> CS
    PS -.-> US


    %% =================================================
    %% CAPA DE DATOS
    %% =================================================

    subgraph Datos["3. CAPA DE DATOS<br/>Persistencia y consultas a la base de datos"]
        direction LR

        URepo["usuarios.repository.js"]
        SRepo["sellers.repository.js"]
        CRepo["catalogo.repository.js"]
        CARepo["carrito.repository.js"]
        PRepo["pedidos.repository.js"]
    end

    US --> URepo
    SS --> SRepo
    CS --> CRepo
    CAS --> CARepo
    PS --> PRepo


    DBAccess["Acceso a datos compartido<br/>Sequelize ORM · models · pool de conexiones<br/>src/shared/db"]

    URepo --> DBAccess
    SRepo --> DBAccess
    CRepo --> DBAccess
    CARepo --> DBAccess
    PRepo --> DBAccess

end


%% =====================================================
%% BASE DE DATOS
%% =====================================================

DBAccess -->|"SQL · TCP 5432"| PostgreSQL

PostgreSQL[("🐘 PostgreSQL<br/>marketplace_db")]


%% =====================================================
%% SISTEMAS EXTERNOS
%% =====================================================

PS -->|"HTTPS / REST"| Pago
PS -->|"HTTPS / REST"| Envio

Pago["💳 Sistema externo<br/>Pasarela de pagos<br/>Ej. Culqi / Niubiz"]

Envio["🚚 Sistema externo<br/>Servicio de envíos<br/>API del courier"]
```

### Reglas de la arquitectura

1. Cada capa solo invoca a la capa inmediatamente inferior.
2. Un módulo no accede al repositorio ni a las tablas de otro módulo.
3. La comunicación entre módulos se realiza llamando a sus servicios.
4. Todo el sistema funciona como un único proceso Node.js con una única base de datos.

![Arquitectura del Marketplace](/img/arquitectura-marketplace.png)