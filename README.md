
## Paso 1: Configurar Amazon S3 (Simple Storage Service)

### Crear un Bucket en S3

1. Inicia sesión en la consola de AWS.
2. Navega a **S3** en el menú de servicios.
3. Haz clic en **Create bucket**.
4. Introduce un nombre único para tu bucket (por ejemplo, `mi-portafolio`).
5. Selecciona la región donde deseas crear el bucket.
6. Haz clic en **Create bucket** al final de la página.

### Subir Archivos Estáticos

1. Haz clic en el bucket que acabas de crear.
2. Selecciona la pestaña **Objects**.
3. Haz clic en **Upload**.
4. Arrastra y suelta tus archivos HTML, CSS y JavaScript, o selecciona **Add files**.
5. Haz clic en **Upload** para subir los archivos.

### Configurar el Bucket como Sitio Web Estático

1. En la configuración del bucket, selecciona **Properties**.
2. Busca la sección **Static website hosting** y haz clic en **Edit**.
3. Selecciona **Enable** y configura el **Index document** (por ejemplo, `index.html`).
4. Opcionalmente, configura un documento de error (por ejemplo, `error.html`).
5. Haz clic en **Save changes**.

### Configurar Permisos Públicos

1. Ve a la pestaña **Permissions**.
2. En la sección **Bucket policy**, haz clic en **Edit**.
3. Copia y pega la siguiente política para hacer que los archivos sean públicos:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::mi-portafolio/*"
            }
        ]
    }
    ```

4. Reemplaza `mi-portafolio` con el nombre de tu bucket.
5. Haz clic en **Save changes**.

## Paso 2: Configurar Amazon CloudFront

### Crear una Distribución de CloudFront

1. En la consola de AWS, navega a **CloudFront**.
2. Haz clic en **Create Distribution**.
3. Selecciona **Web** y haz clic en **Get Started**.
4. En **Origin Domain Name**, selecciona tu bucket de S3 desde el menú desplegable.
5. Configura otras opciones según tus necesidades, pero asegúrate de habilitar **Restrict Bucket Access** si deseas un mayor control sobre quién puede acceder a tu contenido.
6. Haz clic en **Create Distribution**.

### Configurar el CNAME (Opcional)

1. En la distribución de CloudFront que acabas de crear, ve a la pestaña **General**.
2. En **Settings**, agrega un **Alternate Domain Name (CNAME)** si tienes un dominio personalizado.
3. Configura un **SSL Certificate** si deseas usar HTTPS con tu dominio personalizado.

## Paso 3: Configurar Amazon Route 53

### Crear una Zona de Hospedaje

1. En la consola de AWS, navega a **Route 53**.
2. Selecciona **Hosted zones** y haz clic en **Create hosted zone**.
3. Introduce el nombre de tu dominio (por ejemplo, `miportafolio.com`).
4. Haz clic en **Create hosted zone**.

### Configurar Registros DNS

1. En tu zona de hospedaje, haz clic en **Create record**.
2. Crea un registro A para tu dominio raíz:
    - **Record type:** A - IPv4 address
    - **Alias:** Yes
    - **Alias Target:** Selecciona tu distribución de CloudFront
    - Haz clic en **Create records**.
3. Repite el proceso para crear un registro CNAME para `www` si es necesario.

### Configurar el Dominio

1. Actualiza los servidores de nombres (NS records) en tu registrador de dominio para que apunten a los servidores de nombres proporcionados por Route 53.

## Paso 4: Configurar Amazon DynamoDB

### Crear una Tabla DynamoDB

1. En la consola de AWS, navega a **DynamoDB**.
2. Haz clic en **Create table**.
3. Introduce un nombre para tu tabla (por ejemplo, `VisitorLogs`).
4. Define la clave principal (por ejemplo, `VisitorID` como tipo String).
5. Haz clic en **Create table**.

## Paso 5: Configurar Amazon API Gateway

### Crear una API REST

1. En la consola de AWS, navega a **API Gateway**.
2. Haz clic en **Create API** y selecciona **REST API**.
3. Selecciona **New API** y haz clic en **Create API**.
4. Introduce un nombre para tu API (por ejemplo, `PortfolioAPI`).
5. Haz clic en **Create Resource** y añade recursos según tus necesidades (por ejemplo, `/visitors`).
6. Configura los métodos HTTP (GET, POST, etc.) y conecta cada método a una función Lambda.

## Paso 6: Configurar AWS Lambda

### Crear una Función Lambda

1. En la consola de AWS, navega a **Lambda**.
2. Haz clic en **Create function**.
3. Selecciona **Author from scratch**.
4. Introduce un nombre para tu función (por ejemplo, `VisitorHandler`).
5. Selecciona un tiempo de ejecución compatible (por ejemplo, Node.js, Python).
6. Haz clic en **Create function**.
7. En el editor de código, agrega la lógica para manejar las solicitudes de tu API.
8. Configura los permisos necesarios para que Lambda pueda interactuar con DynamoDB y API Gateway.

### Configurar Trigger de API Gateway

1. En tu función Lambda, ve a la pestaña **Configuration** y selecciona **Triggers**.
2. Haz clic en **Add trigger**.
3. Selecciona **API Gateway** y configura el endpoint de la API que has creado.
