## Diagrama de Clean Architecture

```mermaid
flowchart LR

%% =====================================================
%% USUARIO
%% =====================================================

Usuario["👤 Usuario<br/>Cliente"]

%% =====================================================
%% MARKETPLACE WEB
%% =====================================================

subgraph Marketplace["Marketplace Web - Angular 18 + TypeScript"]

    direction LR

    %% =================================================
    %% PRESENTACIÓN
    %% =================================================

    subgraph Presentacion["PRESENTACIÓN<br/>src/app/presentacion/"]

        direction TB

        CatalogoComp["CatalogoComponent<br/>lista y filtra productos"]

        EstadoCarrito["EstadoCarrito<br/>signals · sin reglas"]

        CarritoComp["CarritoComponent<br/>resumen y confirmar compra"]

        AppComp["AppComponent<br/>shell de la aplicación"]
    end


    %% =================================================
    %% APLICACIÓN
    %% =================================================

    subgraph Aplicacion["APLICACIÓN<br/>Casos de uso · src/app/aplicacion/"]

        direction TB

        ConsultarCatalogo["ConsultarCatalogoCasoUso<br/>ejecutar()"]

        AgregarCarrito["AgregarAlCarritoCasoUso<br/>ejecutar()"]

        RegistrarCompra["RegistrarCompraCasoUso<br/>ejecutar()"]
    end


    %% =================================================
    %% DOMINIO
    %% =================================================

    subgraph Dominio["DOMINIO<br/>Núcleo · src/app/dominio/"]

        direction TB

        subgraph Modelos["Modelos - entidades y reglas"]

            direction TB

            Producto["Producto<br/>stock · categoría · precio"]

            Carrito["Carrito<br/>inmutable · subtotal · total"]

            Pedido["Pedido<br/>estados · cancelación"]

            Precios["Reglas de precios<br/>comisión 10% · IGV 18%"]
        end


        subgraph Contratos["Contratos - puertos"]

            direction TB

            RepoProductos["RepositorioProductos"]

            RepoPedidos["RepositorioPedidos"]

            ProcesadorPagos["ProcesadorPagos"]

            NotificadorCliente["NotificadorCliente"]
        end
    end


    %% =================================================
    %% INFRAESTRUCTURA
    %% =================================================

    subgraph Infraestructura["INFRAESTRUCTURA<br/>src/app/infraestructura/"]

        direction TB

        RepoProductosMem["RepositorioProductosMemoria<br/>RepositorioProductosHttp"]

        RepoPedidosMem["RepositorioPedidosMemoria"]

        ProcesadorPagoImpl["ProcesadorPagosSimulado<br/>ProcesadorPagosNiubiz"]

        NotificadorImpl["NotificadorConsola<br/>NotificadorWhatsApp"]

        Tokens["tokens.ts<br/>InjectionToken por contrato"]
    end


    %% =================================================
    %% COMPOSICIÓN
    %% =================================================

    AppConfig["app.config.ts<br/>Raíz de composición<br/>Selecciona adaptadores e inyecta dependencias"]
end


%% =====================================================
%% BACKEND EXTERNO
%% =====================================================

Backend["🌐 Marketplace API REST<br/>Backend Node.js · monolito modular<br/><br/>/api/productos<br/>/api/pedidos<br/>/api/autorizacion<br/>/api/mensajes"]


%% =====================================================
%% FLUJO DE PRESENTACIÓN
%% =====================================================

Usuario -->|"navegador"| CatalogoComp

CatalogoComp --> ConsultarCatalogo
CarritoComp --> AgregarCarrito
CarritoComp --> RegistrarCompra

EstadoCarrito -.-> CarritoComp
AppComp --> CatalogoComp
AppComp --> CarritoComp


%% =====================================================
%% APLICACIÓN HACIA DOMINIO
%% =====================================================

ConsultarCatalogo --> RepoProductos
AgregarCarrito --> Carrito
RegistrarCompra --> Pedido

RegistrarCompra --> RepoPedidos
RegistrarCompra --> ProcesadorPagos
RegistrarCompra --> NotificadorCliente


%% =====================================================
%% ENTIDADES DEL DOMINIO
%% =====================================================

Producto --> Precios
Carrito --> Producto
Pedido --> Carrito


%% =====================================================
%% INFRAESTRUCTURA IMPLEMENTA CONTRATOS
%% =====================================================

RepoProductosMem -.->|"implementa"| RepoProductos
RepoPedidosMem -.->|"implementa"| RepoPedidos
ProcesadorPagoImpl -.->|"implementa"| ProcesadorPagos
NotificadorImpl -.->|"implementa"| NotificadorCliente


%% =====================================================
%% INFRAESTRUCTURA CON BACKEND
%% =====================================================

RepoProductosMem -->|"HTTP / JSON"| Backend
RepoPedidosMem -->|"HTTP / JSON"| Backend
ProcesadorPagoImpl -->|"HTTP / JSON"| Backend
NotificadorImpl -->|"HTTP / JSON"| Backend


%% =====================================================
%% INYECCIÓN DE DEPENDENCIAS
%% =====================================================

AppConfig -.-> Tokens
Tokens -.-> RepoProductosMem
Tokens -.-> RepoPedidosMem
Tokens -.-> ProcesadorPagoImpl
Tokens -.-> NotificadorImpl
```

### Regla de dependencia

1. El **dominio no importa nada de las capas externas**.
2. Los casos de uso solo conocen **entidades y contratos**.
3. Los adaptadores implementan contratos definidos en el dominio.
4. Cambiar de tecnología implica modificar la infraestructura o `app.config.ts`, no el dominio.

![Diagrama de Clean Architecture](../images/enfoque-arquitectonico.png)