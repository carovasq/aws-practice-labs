# 🧪 Laboratorio: Introducción a Amazon EC2

## Descripción general

Este laboratorio proporciona una descripción básica sobre el lanzamiento, cambio de tamaño, administración y monitoreo de una instancia de Amazon EC2. Amazon Elastic Compute Cloud (Amazon EC2) es un servicio web que ofrece capacidad informática redimensionable en la nube, diseñado para facilitar la computación en la nube a escala web a los desarrolladores.

![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-000.jpeg)

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
3. Seleccione **Lanzar la instancia** y luego seleccione **Iniciar instancia**.
![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-0.png)

##### Paso 1: Nombrar su instancia EC2
Cuando le asignas un nombre a tu instancia, AWS crea un par clave-valor. La clave de este par es **Name** y el valor es el nombre que ingresas para tu instancia EC2.
4. En el panel **Nombre y etiquetas**, en el cuadro de texto **Nombre**, ingrese `Web Server`.

##### Paso 2: Elección de una imagen de máquina de Amazon (AMI)
Una AMI proporciona la información necesaria para iniciar una instancia, que es un servidor virtual en la nube. Una AMI incluye lo siguiente:
- Una plantilla para el volumen raíz de la instancia (por ejemplo, un sistema operativo o un servidor de aplicaciones con aplicaciones)
- Permisos de lanzamiento que controlan qué cuentas de AWS pueden usar la AMI para lanzar instancias
- Un mapeo de dispositivo de bloque que especifica los volúmenes que se adjuntarán a la instancia cuando se inicie.

    La lista de inicio rápido contiene las AMI más utilizadas. También puede crear su propia AMI o seleccionar una AMI de AWS Marketplace, una tienda en línea donde puede vender o comprar software que se ejecuta en AWS.
5. Localice el panel **Imágenes de aplicaciones y sistemas operativos (Amazon Machine Image)**.
6. En **Imagen de máquina AMI (AMI)**, observe que la imagen **Amazon Linux 2 AMI** está seleccionada de manera predeterminada. Mantenga esta configuración.
![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-1.png)

##### Paso 3: Elegir un tipo de instancia
Amazon EC2 ofrece una amplia selección de tipos de instancias optimizadas para adaptarse a diferentes casos de uso. Los tipos de instancias comprenden distintas combinaciones de CPU, memoria, almacenamiento y capacidad de red y le brindan la flexibilidad de elegir la combinación adecuada de recursos para sus aplicaciones. Cada tipo de instancia incluye uno o más tamaños de instancia para que pueda escalar sus recursos según los requisitos de su carga de trabajo de destino.

Seleccione una instancia **t3.micro**. **Este tipo de instancia tiene 2 CPU virtuales y 1 GiB de memoria**.
7. En el menú desplegable, seleccione **t3.micro**.

   **NOTA:** Es posible que no pueda utilizar otros tipos de instancias en este laboratorio.

##### Paso 4: Configurar un par de claves
Amazon EC2 utiliza criptografía de clave pública para cifrar y descifrar la información de inicio de sesión. Para iniciar sesión en su instancia, debe crear un par de claves, especificar el nombre del par de claves cuando inicie la instancia y proporcionar la clave privada cuando se conecte a la instancia.

En este laboratorio, no inicia sesión en su instancia, por lo que no necesita un par de claves.

8. En el panel **Par de claves (inicio de sesión), seleccione Continuar sin un par de claves (no recomendado)**.
![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-2.png)

##### Paso 5: Configurar los ajustes de red
En este laboratorio, no inicia sesión en su instancia, por lo que no necesita un par de claves.

La **VPC** indica en qué nube privada virtual (VPC) desea iniciar la instancia. Puede tener varias VPC, incluidas distintas para desarrollo, pruebas y producción.
9. En el panel de **configuración de red**, seleccione **Editar**.
10. Para **VPC obligatorio**, seleccione **Lab VPC**.
11. En el panel de configuración de red, configure el grupo de seguridad de la siguiente manera:
    - **Nombre del grupo de seguridad (obligatorio)**: `Web Server security group`.
    - **Descripción**: `Security group for my web server`.

    Un _grupo de seguridad_ actúa como un firewall virtual que controla el tráfico de una o más instancias. Cuando se inicia una instancia, se asocian uno o más grupos de seguridad con la instancia. Se agregan _reglas_ a cada grupo de seguridad que permiten el tráfico hacia o desde sus instancias asociadas. Puede modificar las reglas de un grupo de seguridad en cualquier momento; las nuevas reglas se aplican automáticamente a todas las instancias que están asociadas con el grupo de seguridad.
12. En **las reglas de grupos de seguridad entrantes**, seleccione **Eliminar**

    En este laboratorio, no iniciará sesión en su instancia mediante SSH. Eliminar el acceso SSH mejorará la seguridad de la instancia.
![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-3.png)

##### Paso 6: Agregar almacenamiento
En este laboratorio, no iniciará sesión en su instancia mediante SSH. Eliminar el acceso SSH mejorará la seguridad de la instancia.

Inicia la instancia EC2 utilizando un volumen de disco de 8 GiB predeterminado. Este es el volumen raíz (también conocido como volumen de arranque).

13. En el panel **Configurar almacenamiento**, mantenga la configuración de almacenamiento predeterminada.

##### Paso 7: Configuración de detalles avanzados
14. Expandir el panel **Detalles avanzados**.
15. Seleccione el menú desplegable para **Protección de terminación** y elija **Habilitar**.

    Cuando inicia una instancia en Amazon EC2, tiene la opción de pasar datos de usuario a la instancia. Estos comandos se pueden utilizar para realizar tareas de configuración automatizadas comunes e incluso ejecutar scripts después de que se inicie la instancia.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-4.png)
16. Copie y pegue el siguiente script en el cuadro de texto **Datos de usuario**:

    ```bash
    #!/bin/bash
    yum -y install httpd
    systemctl enable httpd
    systemctl start httpd
    echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
    ```
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-5.png)
    El script hace lo siguiente:
    - Instalar un servidor web Apache (httpd)
    - Configurar el servidor web para que se inicie automáticamente al arrancar
    - Activar el servidor web
    - Crear una página web sencilla

##### Paso 8: Lanzamiento de una instancia EC2
Ahora que ha configurado los ajustes de su instancia EC2, es momento de iniciar su instancia.
17. En el panel derecho, seleccione ``Iniciar instancia``
18. Seleccione ``Ver todas las instancias ``
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-6.png)
    La instancia aparece en estado **Pendiente**, lo que significa que se está iniciando. Luego cambia a **En ejecución**, lo que indica que la instancia ha comenzado a iniciarse. Pasará un breve tiempo antes de que puedas acceder a la instancia.

    La instancia recibe un nombre DNS público que puede utilizar para comunicarse con la instancia desde Internet.
19. Seleccione la casilla del **servidor web**. La pestaña **Detalles** muestra información detallada sobre su instancia. 

    Para ver más información en la pestaña **Detalles**, arrastre el divisor de la ventana hacia arriba.

    Revise la información que se muestra en las pestañas **Detalles**, **Seguridad** y **Redes**.
20. Espere a que su instancia muestre lo siguiente:

    **Nota:** Actualice si es necesario.
    - **Estado de la instancia:** Correr
    - **Comprobaciones de estado:** 2/2 cheques aprobados

### Tarea 2: Supervisar su instancia
El monitoreo es una parte importante para mantener la confiabilidad, disponibilidad y rendimiento de sus instancias de Amazon Elastic Compute Cloud (Amazon EC2) y sus soluciones de AWS.
21. Seleccione la instancia marcando la casilla junto a la instancia y navegue hasta la parte inferior de la pantalla hasta la pestaña **Verificaciones de estado**.

    Con la supervisión del estado de las instancias, puede determinar rápidamente si Amazon EC2 ha detectado algún problema que pueda impedir que sus instancias ejecuten aplicaciones. Amazon EC2 realiza comprobaciones automáticas en cada instancia EC2 en ejecución para identificar problemas de hardware y software.

    Tenga en cuenta que se han aprobado las comprobaciones de **accesibilidad del sistema** y de **accesibilidad de la instancia**.
22. Seleccione la pestaña **Monitoreo**.

    Esta pestaña muestra las métricas de Amazon CloudWatch para su instancia. Actualmente, no hay muchas métricas para mostrar porque la instancia se lanzó recientemente.

    Puede elegir un gráfico para ver una vista ampliada.

    Amazon EC2 envía métricas a Amazon CloudWatch para sus instancias EC2. La monitorización básica (cinco minutos) está habilitada de forma predeterminada. Puede habilitar la monitorización detallada (un minuto).
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-7.png)
23. En el menú de ``Acciones``, seleccione **Monitorear y solucionar problemas** y después, **Obtener captura de pantalla de la instancia**.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-8.png)
    Esto le muestra cómo se vería su consola de instancia de Amazon EC2 si tuviera una pantalla adjunta.

    Si no puede acceder a su instancia a través de SSH o RDP, puede capturar una captura de pantalla de su instancia y verla como una imagen. Esto proporciona visibilidad sobre el estado de la instancia y permite una resolución de problemas más rápida.
24. Seleccione ``Cancelar`` ubicado en la parte inferior de la captura de pantalla de la instancia.

    **¡Felicitaciones!** Ha explorado varias formas de monitorear su instancia.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-9.png)

### Tarea 3: Actualice su grupo de seguridad y acceda al servidor web
Cuando inició la instancia EC2, proporcionó un script que instaló un servidor web y creó una página web simple. En esta tarea, accederá al contenido del servidor web.
25. Seleccione la instancia marcando la casilla y seleccione la pestaña **Detalles**.
26. Copie la **Public IPv4 address** de su instancia a su portapapeles.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-10.png)
27. Abra una nueva pestaña en su navegador web, pegue la dirección IP que acaba de copiar y luego presione **Enter**.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-11.png)
    **Pregunta:** ¿Puedes acceder a tu servidor web? ¿Por qué no?

    **Actualmente no** puede acceder a su servidor web porque el _grupo de seguridad_ no permite el tráfico entrante en el puerto 80, que se utiliza para solicitudes web HTTP. Esta es una demostración del uso de un grupo de seguridad como firewall para restringir el tráfico de red que se permite dentro y fuera de una instancia.

    Para corregir esto, ahora actualizará el grupo de seguridad para permitir el tráfico web en el puerto 80.
28. Mantenga abierta la pestaña del navegador, pero regrese a la pestaña **Consola de administración de EC2**.
29. En el panel de navegación izquierdo, seleccione **Grupos de seguridad** ubicado en **Red y seguridad**.
30. Seleccionar **Grupo de seguridad del servidor web**.
31. Seleccione la pestaña **Reglas de entrada**.

    El grupo de seguridad actualmente no tiene reglas.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-12.png)
32. Seleccione ``Editar reglas de entrada``, luego seleccione ``Agregar regla`` y configure la regla con las siguientes configuraciones:
    - **Tipo:** _HTTP_ 
    - **Fuente:** _Anywhere-IPv4_
    - Seleccione ``Guardar reglas``

    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-13.png)
33. Regrese a la pestaña del servidor web que abrió anteriormente y actualice la página.

    Deberías ver el mensaje _¡Hola desde tu servidor web!_

    **¡Felicitaciones!** Ha modificado correctamente su grupo de seguridad para permitir el tráfico HTTP en su instancia de Amazon EC2.

### Tarea 4: Cambiar el tamaño de la instancia: tipo de instancia y volumen EBS
A medida que sus necesidades cambien, es posible que descubra que su instancia está sobreutilizada (demasiado pequeña) o subutilizada (demasiado grande). Si es así, puede cambiar el _tipo de instancia_. Por ejemplo, si una instancia _t3.micro_ es demasiado pequeña para su carga de trabajo, puede cambiarla a una instancia _m5.medium_. De manera similar, puede cambiar el tamaño de un disco.

#### Detener su instancia
Antes de poder cambiar el tamaño de una instancia, debes detenerla .

Cuando se detiene una instancia, esta se apaga. No se cobra ningún cargo por una instancia EC2 detenida, pero se mantiene el cargo por almacenamiento de los volúmenes de Amazon EBS conectados.

34. En la **Consola de administración de EC2**, en el panel de navegación izquierdo, seleccione **Instancias**.

    **El servidor web** ya debería estar seleccionado.
35. Seleccione **Estado de la instancia > Detener instancia**.
36. Seleccionar ``Detener``
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-14.png)

    Su instancia realizará un apagado normal y luego dejará de funcionar.
37. Espere a que el **estado de la instancia** muestre: detenido

#### Cambiar el tipo de instancia
38. En el menú de ``Acciones``, **seleccione Configuración de instancia > Cambie el tipo de instancia** y luego configure:
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-15.png)
    - **Tipo de instancia:** _t3.small_
    - Seleccione ``Aplicar``
      ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-16.png)
    Cuando se vuelva a iniciar la instancia, será una _t3.small_, que tiene el doble de memoria que una instancia _t3.micro_. **NOTA:** Es posible que no pueda usar otros tipos de instancias en este laboratorio.

#### Cambiar el tamaño del volumen EBS
39. En el menú de navegación de la izquierda, seleccione **Volúmenes** ubicado debajo de **Elastic Block Store**.
40. Seleccione el volumen marcando la casilla y navegue hasta ``Acciones``, seleccione **Modificar volumen**.
![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-17.png)

    El volumen del disco actualmente tiene un tamaño de 8 GiB. Ahora aumentará el tamaño de este disco.
41. Cambie el tamaño a: 10 

    **NOTA:** Es posible que tenga restricciones para crear grandes volúmenes de Amazon EBS en este laboratorio.
42. Seleccione ``Modificar``
43. Seleccione ``Modificar`` para confirmar y aumentar el tamaño del volumen.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-18.png)

#### Iniciar la instancia redimensionada
Ahora iniciará nuevamente la instancia, que ahora tendrá más memoria y más espacio en disco.

44. En el panel de navegación izquierdo, seleccione **Instancias**.

45. Seleccione la instancia del **servidor web** marcando la casilla y luego navegue a **Estado de la instancia > Iniciar instancia**.

    **¡Felicitaciones!** Ha redimensionado correctamente su instancia de Amazon EC2. En esta tarea, cambió el tipo de instancia de _t3.micro_ a _t3.small_. También modificó el volumen del disco raíz de 8 GiB a 10 GiB.

### Tarea 5: Prueba de protección contra terminación
Puede eliminar su instancia cuando ya no la necesite. Esto se conoce como finalizar su instancia. No puede conectarse a una instancia ni reiniciarla después de que se haya finalizado.

En esta tarea, aprenderá cómo utilizar la protección de terminación.

46. En el panel de navegación izquierdo, seleccione **Instancias**.

47. Seleccione la instancia del **servidor web** marcando la casilla y navegue hasta la parte superior y seleccione ``Estado de la instancia``, seleccionar **Terminar instancia**.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-19.png)

    Nota: Hay un mensaje que dice: _En una instancia respaldada por EBS, la acción predeterminada es que se elimine el volumen raíz de EBS cuando se finaliza la instancia. Se perderá el almacenamiento en cualquier unidad local_. Le preguntará si está seguro de que desea finalizar la instancia. Podrá seleccionar el botón ``Finalizar``.

    Nota: Notará que la instancia no finalizó y aparecerá un mensaje de error rojo en la parte superior que dice: _No se pudo finalizar una instancia: es posible que la instancia no se haya finalizado_. Esto se debe a que tiene habilitada la protección contra finalización.
48. En ``Acciones``, seleccione **Configuración de instancia > Cambiar la protección de terminación**.
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-20.png)
49. Desmarcar la casilla **Habilitar** seguido de ``Guardar``
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-21.png)
    Ahora puedes finalizar la instancia.
50. En ``Acciones``, seleccione **Estado de la instancia > Terminar instancia**.
51. Seleccionar ``Terminar``
    ![](https://raw.githubusercontent.com/carovasq/aws-practice-labs/refs/heads/main/Lab%2001%20-%20Introducci%C3%B3n%20a%20Amazon%20EC2/img/lab01-22.png)
    **¡Felicitaciones!** Ha probado con éxito la protección contra terminación y ha finalizado su instancia.