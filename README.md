
## Paso 1: Crear un Bucket en Amazon S3

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

## Paso 2: Crear una Función AWS Lambda

### Crear una Función Lambda

1. En la consola de AWS, navega a **Lambda**.
2. Haz clic en **Create function**.
3. Selecciona **Author from scratch**.
4. Introduce un nombre para tu función (por ejemplo, `VisitorHandler`).
5. Selecciona un tiempo de ejecución compatible (por ejemplo, Node.js, Python).
6. Haz clic en **Create function**.
7. En el editor de código, agrega la lógica para manejar las solicitudes (por ejemplo, procesamiento de formularios).
8. Configura los permisos necesarios para que Lambda pueda interactuar con DynamoDB y API Gateway.

## Paso 3: Crear una API Gateway

### Crear una API REST

1. En la consola de AWS, navega a **API Gateway**.
2. Haz clic en **Create API** y selecciona **REST API**.
3. Selecciona **New API** y haz clic en **Create API**.
4. Introduce un nombre para tu API (por ejemplo, `PortfolioAPI`).
5. Haz clic en **Create Resource** y añade recursos según tus necesidades (por ejemplo, `/visitors`).
6. Configura los métodos HTTP (GET, POST, etc.) y conecta cada método a la función Lambda creada en el paso anterior.

### Conectar API Gateway con S3 y Lambda

1. En tu API Gateway, navega a **Stages** y crea un nuevo stage (por ejemplo, `prod`).
2. En el stage creado, selecciona **Deploy** para desplegar la API.
3. En la configuración de la API, asegúrate de que las solicitudes GET de `/` estén configuradas para servir el contenido de S3, y las solicitudes POST estén configuradas para llamar a la función Lambda.

## Paso 4: Configurar Amazon SNS y DynamoDB

### Crear una Tabla DynamoDB

1. En la consola de AWS, navega a **DynamoDB**.
2. Haz clic en **Create table**.
3. Introduce un nombre para tu tabla (por ejemplo, `VisitorLogs`).
4. Define la clave principal (por ejemplo, `VisitorID` como tipo String).
5. Haz clic en **Create table**.

### Crear un Tópico SNS

1. En la consola de AWS, navega a **SNS**.
2. Haz clic en **Create topic**.
3. Selecciona **Standard** y introduce un nombre para tu tópico (por ejemplo, `VisitorNotifications`).
4. Haz clic en **Create topic**.

### Conectar SNS y DynamoDB a Lambda

1. En tu función Lambda, ve a la pestaña **Configuration** y selecciona **Triggers**.
2. Haz clic en **Add trigger**.
3. Selecciona **SNS** y configura el tópico creado anteriormente.
4. Configura permisos adicionales en la política de IAM de tu función Lambda para permitir la interacción con DynamoDB.

## Paso 5: Configurar Amazon CloudFront

### Crear una Distribución de CloudFront

1. En la consola de AWS, navega a **CloudFront**.
2. Haz clic en **Create Distribution**.
3. Selecciona **Web** y haz clic en **Get Started**.
4. En **Origin Domain Name**, selecciona tu bucket de S3 desde el menú desplegable.
5. Configura otras opciones según tus necesidades, pero asegúrate de habilitar **Restrict Bucket Access** si deseas un mayor control sobre quién puede acceder a tu contenido.
6. Haz clic en **Create Distribution**.

### Conectar CloudFront a S3

1. En la distribución de CloudFront que acabas de crear, ve a la pestaña **Origins**.
2. Configura las políticas de caché y comportamiento de la distribución para asegurar que el contenido estático de S3 se distribuya de manera eficiente.
3. Asegúrate de que el nombre del bucket de S3 esté correctamente configurado como origen en CloudFront.

## Paso 6: Comprar un Dominio en Amazon Route 53

### Comprar un Dominio

1. En la consola de AWS, navega a **Route 53**.
2. Selecciona **Registered domains** y haz clic en **Register Domain**.
3. Introduce el nombre de dominio que deseas registrar (por ejemplo, `miportafolio.com`).
4. Sigue los pasos para completar la compra del dominio.

### Configurar una Zona de Hospedaje

1. En Route 53, selecciona **Hosted zones** y haz clic en **Create hosted zone**.
2. Introduce el nombre de tu dominio y haz clic en **Create hosted zone**.

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

