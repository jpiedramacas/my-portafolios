# Creación de Portafolio Personal en AWS

Este proyecto describe la configuración de un portafolio personal utilizando varios servicios de AWS. La infraestructura incluye los siguientes componentes: S3, CloudFront, Route 53, Certificados SSL, DynamoDB, API Gateway y Lambda.

![IMAGE PORTAFOLIOS](https://github.com/jpiedramacas/my-portafolios/blob/main/mapa_mental_portafolio.png)

## Servicios de AWS Utilizados

### 1. Amazon S3 (Simple Storage Service)
Amazon S3 se utiliza para el almacenamiento de los archivos estáticos del portafolio, como HTML, CSS, JavaScript e imágenes. S3 proporciona una solución de almacenamiento escalable y segura.

**Características clave:**
- **Durabilidad:** 99.999999999% de durabilidad garantizada para los objetos almacenados.
- **Escalabilidad:** Almacenamiento prácticamente ilimitado.
- **Acceso:** Integración fácil con otros servicios de AWS y control de acceso granular.

### 2. Amazon CloudFront
CloudFront es una red de entrega de contenido (CDN) que se utiliza para distribuir el contenido del portafolio con baja latencia y altas velocidades de transferencia. Mejora la experiencia del usuario al ofrecer contenido a través de una red global de servidores.

**Características clave:**
- **Reducción de latencia:** El contenido se entrega desde la ubicación más cercana al usuario.
- **Seguridad:** Integración con AWS Shield para protección contra DDoS y con WAF para protección contra amenazas a nivel de aplicación.
- **Escalabilidad automática:** Capacidad para manejar grandes volúmenes de tráfico.

### 3. Amazon Route 53
Route 53 es un servicio de DNS escalable y de alta disponibilidad que se utiliza para dirigir el tráfico a la infraestructura del portafolio. Se encarga de resolver el nombre de dominio del portafolio a la IP correspondiente.

**Características clave:**
- **Alta disponibilidad:** Operación en múltiples ubicaciones para garantizar resiliencia.
- **Escalabilidad:** Manejo eficiente de grandes volúmenes de solicitudes de DNS.
- **Integración con otros servicios de AWS:** Fácil configuración con CloudFront, S3, entre otros.

### 4. AWS Certificate Manager (ACM)
ACM se utiliza para administrar los certificados SSL/TLS necesarios para asegurar el tráfico HTTPS hacia el portafolio. Garantiza que los datos transferidos entre el cliente y el servidor estén cifrados y seguros.

**Características clave:**
- **Automatización:** Emisión, renovación y administración de certificados SSL/TLS sin intervención manual.
- **Integración con otros servicios de AWS:** Compatible con CloudFront, Elastic Load Balancing, y API Gateway.
- **Seguridad:** Cifrado robusto para proteger la información.

### 5. Amazon DynamoDB
DynamoDB es una base de datos NoSQL totalmente gestionada, utilizada para almacenar datos dinámicos del portafolio, como registros de visitantes, feedbacks y cualquier otro dato que requiera un acceso rápido y de baja latencia.

**Características clave:**
- **Escalabilidad:** Ajuste automático de capacidad según la carga de trabajo.
- **Baja latencia:** Respuestas rápidas a cualquier escala.
- **Alta disponibilidad:** Replicación automática en varias regiones.

### 6. Amazon API Gateway
API Gateway se utiliza para crear, publicar, mantener, monitorear y asegurar APIs RESTful y WebSocket a cualquier escala. Permite la interacción entre el frontend del portafolio y las funciones Lambda que manejan la lógica del backend.

**Características clave:**
- **Desarrollo simplificado:** Facilita la creación y gestión de APIs.
- **Escalabilidad:** Manejo de miles de llamadas API concurrentes.
- **Seguridad:** Integración con AWS IAM, Lambda authorizers y Amazon Cognito para la autenticación y autorización.

### 7. AWS Lambda
Lambda se utiliza para ejecutar código en respuesta a eventos, como solicitudes HTTP a través de API Gateway, sin necesidad de administrar servidores. En el contexto del portafolio, Lambda puede manejar lógica de backend como procesamiento de formularios o interacción con DynamoDB.

**Características clave:**
- **Ejecución bajo demanda:** Solo se paga por el tiempo de cómputo consumido.
- **Escalabilidad automática:** Capacidad para manejar desde unos pocos hasta miles de solicitudes simultáneas.
- **Integración con otros servicios de AWS:** Interoperabilidad con S3, DynamoDB, CloudWatch, y más.

## Arquitectura del Portafolio

### Frontend
El frontend del portafolio está compuesto por archivos HTML, CSS y JavaScript alojados en Amazon S3. Estos archivos estáticos son distribuidos a través de Amazon CloudFront para asegurar una entrega rápida y eficiente a nivel global. El contenido del frontend puede incluir páginas de presentación, galerías de proyectos, formularios de contacto y cualquier otra información relevante.

### Backend
El backend del portafolio está construido utilizando los siguientes servicios:

1. **API Gateway:** Exponiendo endpoints RESTful que permiten la comunicación entre el frontend y el backend.
2. **AWS Lambda:** Ejecutando funciones que manejan la lógica del negocio, como el procesamiento de datos de formularios, gestión de contenido dinámico y consultas a la base de datos.
3. **DynamoDB:** Almacenando datos dinámicos como registros de visitantes, feedbacks y cualquier otro tipo de información que necesite ser persistida.
