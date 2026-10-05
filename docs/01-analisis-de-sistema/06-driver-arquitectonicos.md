# Drivers Arquitectónicos

| ID | Driver Arquitectónico | Origen | Justificación |
| :--- | :--- | :--- | :--- |
| **DA01** | Soporte de incremento masivo de usuarios durante campañas comerciales. | AC03 - Escalabilidad | Influye en la estrategia de escalamiento y despliegue. |
| **DA02** | Tiempos de respuesta adecuados bajo alta concurrencia. | AC01 - Rendimiento | Influye en la comunicación, procesamiento y almacenamiento. |
| **DA03** | Protección de datos de usuarios y compras transaccionales. | AC04 - Seguridad | Influye en autenticación, autorización y protección de los datos. |
| **DA04** | Integración con pasarela de pago externa vía API. | RC04 - Pasarela de pago | Condiciona la integración y comunicación con servicios externos. |
| **DA05** | Uso obligatorio de API REST para la comunicación entre frontend y backend. | RC03 - API REST | Determina el mecanismo de comunicación entre los componentes del sistema. |
| **DA06** | El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | AC05 - Mantenibilidad | Influye en la separación de responsabilidades, modularidad y dependencias internas. |