# Arquitectura Inicial del Sistema

## Descripción
La arquitectura inicial se organiza en una arquitectura clásica de **tres capas** más la integración con sistemas externos[cite: 1]:

* **Capa de Presentación:** Permite la interacción del usuario a través de la Aplicación Web y se comunica vía API REST[cite: 1].
* **Capa de Lógica de Negocio:** Contiene los componentes responsables de la lógica operativa (Usuarios, Sellers, Catálogo, Carrito y Pedidos)[cite: 1].
* **Capa de Datos:** Se encarga de almacenar y gestionar la información persistente en la Base de Datos[cite: 1].
* **Sistemas Externos:** Módulos fuera del sistema como Pasarela de pago, ERP y Servicio de envío que interactúan mediante integraciones[cite: 1].

## Diagrama de Arquitectura

```mermaid
flowchart TD
    %% ACTORES
    subgraph ACTORES ["ACTORES"]
        Cliente ["Cliente"]
        Seller ["Seller"]
        Admin ["Administrador"]
    end

    %% PRESENTACIÓN
    subgraph PRESENTACION ["PRESENTACIÓN"]
        Web ["Aplicación Web API REST"]
    end

    %% LÓGICA DE NEGOCIO
    subgraph NEGOCIO ["LÓGICA DE NEGOCIO"]
        Usuarios ["Usuarios"]
        Sellers ["Sellers"]
        Catalogo ["Catálogo"]
        Carrito ["Carrito"]
        Pedidos ["Pedidos"]
    end

    %% DATOS
    subgraph DATOS ["DATOS"]
        BD ["Base de datos"]
    end

    %% SISTEMAS EXTERNOS
    subgraph EXTERNOS ["SISTEMAS EXTERNOS"]
        Pago ["Pasarela de pago"]
        ERP ["ERP"]
        Envio ["Servicio de envío"]
    end

    %% FLUJO PRINCIPAL
    ACTORES --> PRESENTACION
    PRESENTACION --> NEGOCIO
    NEGOCIO --> DATOS

    %% INTEGRACIONES
    DATOS -->|"integraciones"| EXTERNOS

    %% DISTRIBUCIÓN HORIZONTAL
    Cliente ~~~ Seller
    Seller ~~~ Admin
    Usuarios ~~~ Sellers
    Sellers ~~~ Catalogo
    Catalogo ~~~ Carrito
    Carrito ~~~ Pedidos
    Pago ~~~ ERP
    ERP ~~~ Envio

    %% ESTILOS
    style ACTORES fill:#222, stroke:#fff, stroke-width: 2px, color:#fff
    style PRESENTACION fill:#222, stroke:#fff, stroke-width: 2px, color:#fff
    style NEGOCIO fill:#222, stroke:#fff, stroke-width: 2px, color:#fff
    style DATOS fill:#222, stroke:#fff, stroke-width: 2px, color:#fff
    style EXTERNOS fill:#222, stroke:#fff, stroke-width: 2px, color:#fff
    style Cliente fill:#222, stroke:#fff, color:#fff
    style Seller fill:#222, stroke:#fff, color:#fff
    style Admin fill:#222, stroke:#fff, color:#fff
    style Web fill:#222, stroke:#fff, color:#fff
    style Usuarios fill:#222, stroke:#fff, color:#fff
    style Sellers fill:#222, stroke:#fff, color:#fff
    style Catalogo fill:#222, stroke:#fff, color:#fff
    style Carrito fill:#222, stroke:#fff, color:#fff
    style Pedidos fill:#222, stroke:#fff, color:#fff
    style BD fill:#222, stroke:#fff, color:#fff
    style Pago fill:#222, stroke:#fff, color:#fff
    style ERP fill:#222, stroke:#fff, color:#fff
    style Envio fill:#222, stroke:#fff, color:#fff