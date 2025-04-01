# CloudFormation Project

Este repositorio contiene plantillas de CloudFormation para desplegar diversos recursos en AWS, organizados en diferentes "stacks" (pilas). Cada stack se enfoca en un conjunto específico de recursos, permitiendo la modularidad y la fácil gestión de tu infraestructura.

## Estructura del Repositorio

cloudformation-project/
├── README.md
├── parameters/
│   ├── compute-params.json
│   ├── networking-params.json
│   ├── storage-params.json
│   └── vpn-params.json
├── scripts/
│   ├── deploy.sh (Placeholder)
│   └── destroy.sh (Placeholder)
└── templates/
├── compute.yaml
├── networking.yaml
├── storage.yaml
└── vpn.yaml

**`README.md`**: Este archivo, que proporciona información general sobre el proyecto.
* **`parameters/`**: Contiene archivos JSON con los parámetros necesarios para cada stack.
* **`scripts/`**: (Actualmente en blanco) Pensado para scripts de automatización de despliegue y destrucción.
* **`templates/`**: Contiene las plantillas de CloudFormation en formato YAML.

## Descripción de las Plantillas

### 1. Networking Stack (`networking.yaml`)

* Crea una VPC con subredes públicas y un NAT Gateway.
* Configura tablas de enrutamiento y un Internet Gateway para la conectividad a Internet.
* Parámetros configurables:
    * `VpcCidr`: Rango de direcciones CIDR para la VPC.
    * `PublicSubnet1Cidr`, `PublicSubnet2Cidr`: Rangos de direcciones CIDR para las subredes públicas.
    * `NatGatewayEip`: Dirección IP Elástica para el NAT Gateway.
* Archivo de parámetros: `parameters/networking-params.json`

### 2. Compute Stack (`compute.yaml`)

* Despliega instancias EC2 en las subredes públicas.
* Crea un grupo de seguridad para permitir acceso SSH y HTTP.
* Parámetros configurables:
    * `VpcId`, `PublicSubnet1Id`, `PublicSubnet2Id`: IDs de los recursos de la pila de networking.
    * `KeyName`: Nombre de un KeyPair EC2 existente.
    * `Instance1ImageId`, `Instance2ImageId`, `Instance3ImageId`: AMIs para las instancias EC2.
    * `Instance1PrivateIp`, `Instance2PrivateIp`, `Instance3PrivateIp`: Direcciones IP privadas para las instancias.
* Archivo de parámetros: `parameters/compute-params.json`

### 3. Storage Stack (`storage.yaml`)

* Crea un bucket S3 y una tabla DynamoDB.
* Parámetros configurables:
    * `S3BucketName`: Nombre del bucket S3.
    * `DynamoDBTableName`: Nombre de la tabla DynamoDB.
* Archivo de parámetros: `parameters/storage-params.json`

### 4. VPN Stack (`vpn.yaml`)

* Configura una conexión VPN Site-to-Site con un Customer Gateway.
* Crea un Virtual Private Gateway.
* Crea rutas estáticas para la comunicación entre la red local y la VPC.
* Parámetros configurables:
    * `VpcId`: ID de la VPC.
    * `CustomerGatewayIp`: IP pública del Customer Gateway.
    * `LocalNetworkCidr`: Rango de direcciones IP de la red local.
    * `BgpAsn`: Número ASN para BGP.
* Archivo de parámetros: `parameters/vpn-params.json`

## Cómo Utilizar

1.  **Requisitos:**
    * AWS CLI instalado y configurado.
    * Cuenta de AWS con permisos para crear los recursos especificados.
2.  **Configurar los parámetros:**
    * Edita los archivos JSON en el directorio `parameters/` con los valores adecuados para tu entorno.
3.  **Desplegar las pilas:**
    * Utiliza la AWS CLI para crear las pilas de CloudFormation. Por ejemplo, para la pila de networking:

        ```bash
        aws cloudformation create-stack --stack-name NetworkingStack --template-body file://templates/networking.yaml --parameters file://parameters/networking-params.json
        ```

    * Repite el proceso para las demás pilas, ajustando los nombres de la pila y los archivos de parámetros.
4.  **Verificar el despliegue:**
    * Utiliza la consola de AWS CloudFormation o la AWS CLI para verificar el estado de las pilas.

## Próximos Pasos

* Implementar los scripts `deploy.sh` y `destroy.sh` para automatizar el despliegue y la destrucción de las pilas.
* Agregar mas validaciones a las plantillas de cloud formation.
* Agregar mas recursos a las plantillas.

## Contribuciones

Las contribuciones son bienvenidas. Por favor, crea un pull request con tus cambios.