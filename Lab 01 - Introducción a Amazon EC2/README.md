# 🧪 Laboratorio: Introducción a Amazon EC2

## Descripción general

Este laboratorio proporciona una descripción básica sobre el lanzamiento, cambio de tamaño, administración y monitoreo de una instancia de Amazon EC2. Amazon Elastic Compute Cloud (Amazon EC2) es un servicio web que ofrece capacidad informática redimensionable en la nube, diseñado para facilitar la computación en la nube a escala web a los desarrolladores.

## Diagrama arquitectónico

[Incluir Diagrama Arquitectónico aquí]

## Objetivos

Al finalizar este laboratorio, podrás:
- Iniciar un servidor web con protección de terminación habilitada.
- Supervisar su instancia EC2.
- Modificar el grupo de seguridad que utiliza su servidor web para permitir el acceso HTTP.
- Cambiar el tamaño de su instancia de Amazon EC2 para escalar.
- Probar la protección de terminación.
- Terminar su instancia EC2.


## Tareas

### Tarea 1: Lanzamiento de su instancia EC2

En esta tarea, lanzará una instancia de Amazon EC2 con _protección de terminación_ habilitada. 

**La protección de terminación** evita que finalice accidentalmente una instancia de EC2. Implementará su instancia con un script de datos de usuario que le permitirá implementar un servidor web simple.

1. En la Consola de administración de AWS, en el menú **Servicios**, seleccione **EC2**.
2. En el panel de navegación izquierdo, seleccione **Panel de EC2** para asegurarse de estar en la página del panel.
3. Seleccione **Iniciar instancia** y luego seleccione **Iniciar instancia**.
   ![](https://pandao.github.io/editor.md/examples/images/4.jpg)

##### Paso 1: Nombrar su instancia EC2
Cuando le asignas un nombre a tu instancia, AWS crea un par clave-valor. La clave de este par es **Name** y el valor es el nombre que ingresas para tu instancia EC2.

- En el panel **Nombre y etiquetas**, en el cuadro de texto **Nombre**, ingrese `Web Server`.

##### Paso 2: Elección de una imagen de máquina de Amazon (AMI)
Una AMI proporciona la información necesaria para iniciar una instancia, que es un servidor virtual en la nube. Mantenga la imagen predeterminada **Amazon Linux 2 AMI**.

##### Paso 3: Elegir un tipo de instancia
Seleccione una instancia **t3.micro**. Este tipo de instancia tiene 2 CPU virtuales y 1 GiB de memoria.

##### Paso 4: Configurar un par de claves
En este laboratorio, no necesita un par de claves. Seleccione **Continuar sin un par de claves (no recomendado)**.

##### Paso 5: Configurar los ajustes de red
- En el panel de configuración de red, seleccione **Editar**.
- Para **VPC obligatorio**, seleccione **Lab VPC**.

En el panel de configuración de red, configure el grupo de seguridad de la siguiente manera:

- **Nombre del grupo de seguridad (obligatorio)**: `Web Server security group`.
- **Descripción**: `Security group for my web server`.

Elimine el acceso SSH en las reglas de grupos de seguridad entrantes.

##### Paso 6: Agregar almacenamiento
Inicie la instancia EC2 utilizando un volumen de disco de 8 GiB predeterminado (volumen raíz).

##### Paso 7: Configuración de detalles avanzados
Expandir el panel **Detalles avanzados**.
- Seleccione el menú desplegable para **Protección de terminación** y elija **Habilitar**.

Copie y pegue el siguiente script en el cuadro de texto **Datos de usuario**:

```bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>¡Hola desde su servidor web!</h1></html>' > /var/www/html/index.html
