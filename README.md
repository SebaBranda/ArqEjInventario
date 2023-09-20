# INVENTARIO, MANTENIMIENTO Y CONTROL DE BIENES

## SITUACION ACTUAL

**COMPRAS**: La adquisición de bienes se puede dar mediante del sector de compras o por compra directa utilizando *“caja chica”* o distintos centros de costos de los institutos de la universidad. EL objetivo del sistema de compras es registrar el proceso de adquisición. Fecha de adquisición, valor de adquisición, sector de adquisición (el sector es organizacional no de ubicación física) esto es afines de saber quién es el *“dueño”* del bien.

**AMORTIZACION Y BIENES DE USO:** Cada bien que la universidad adquiere es considerado un bien de uso (escritorios, computadoras, automóviles). Todos los meses se realiza un proceso amortización que genera asientos contra el sistema contable representando el desgaste de estos bienes a la fecha. Con ese fin cada bien de uso debe tener una fecha de adquisición, vida útil en años (contable) y valor de adquisición en pesos. **A FINES PRACTICOS EL DETALLE CONTABLE DE LA AMORTIZACION ESTA SIMPLIFICADO**

**MANTENIMIENTO EDILICIO**: lleva un control de la ubicación física de los bienes, fecha de adquisición, marca, modelo, documentación (factura, manuales, etc.), proveedor, soporte, historial de mantenimiento, notas .



**IOT**: Hay dispositivos inteligentes conectados a la red de la universidad (Aires acondicionados inteligentes, sensores de temperatura, sensores de CO2, switchs de luz inteligentes, UPS inteligentes). Dichos dispositivos estan integrados por un BROKER de mensajes MQTT capaz de enviar y recibir información.
Los datos enviados por estos dispositivos deben ser almacenados en una base de datos de serie de tiempo que contiene los siguientes datos: ID DE DISPOSITIVO, SENSORES, TIMESTAMP, VALOR.


Es importante poder restringir la visualización de los datos de los sensores/DISPOSITIVOS por su ubicación física.



## PROBLEMATICA

La registración de bienes esta descentralizada y no es posible llevar un rastreo de los bienes.
La amortización de bienes de uso se lleva a mano por Excel y se concilia manualmente contra la contabilidad. Hay inconsistencias entre la contabilidad y los bienes de uso, no se sabe dónde estan ubicados o si estan en uso (por que se rompieron).


El mantenimiento se lleva con planillas físicas, los manuales no se resguardan. No se realiza el mantenimiento en tiempo y forma de distintos equipos. Por ejemplo, las UPS de la biofábrica no tienen un cambio de batería hace 4 años.
El sistema de IOT no existe, pero debe integrarse con el sistema de mantenimiento edilicio con el fin de utilizar los datos adquiridos para mantenimiento preventivo / predictivo (por ejemplo si un aire acondicionado consume más energía que el resto puede ser que le falte gas o tenga sucio los filtros).

## TAREAS A REALIZAR

Realice un diagrama de arquitectura del sistema con características indicadas. Las características pueden ser sistemas o servicios en sí mismos con componentes compartidos. Puede diseñarlo con la estructura que desee, pero debe minimizar el acoplamiento entre los distintos elementos sin perder sinergia/cohesión.


CLAVE: Separar los subdominios,  `CORE` (que es propio de cada característica), `GENERIC`(es independiente de la característica y puedo usar un producto existente) Y `SUPPORTING` (no es un elemento core pero es necesario).

Si emplea algún supuesto debe aclararlo.


# RESOLUCION

![Diagrama estructural](./diagrama.drawio.svg)

## SUBDOMINIOS CORE
- COMPRAS
- AM-BU
- MANTENIMIENTO
- CATALOGO-IOT
## SUBDOMINIOS SUPPORTING
- INVENTARIO DE BIENES
## SUBDOMINIOS GENERIC
- BASE DE DATOS SQL
- BASE DE DATOS NOSQL
- MQTT BROKER
- GRAFANA
- TSDB FORWARDER
- RABBITMQ
- TODO: KEYCLOAK / OIDC IDENTITY PROVIDER
## PATRONES
**SPA**: Se utiliza SPA para debido a que los UI no necesitna SEO.
**MEDIATOR PATTERN**: Se utiliza el `MEDIATOR PATTERN` para la integracion del inventario. COn los siguientes mensajes
1. COMANDO DE ALTA DE BIEN
1. COMANDO DE BAJA (LOGICA) DE BIEN
1. NOTIFICACION DE NUEVO BIEN
1. NOTIFICACION DE BAJA DE BIEN

Es clave mantener el "contrato" o la "interface" del servicio de inventario inmutable o poco volatil. Si bien este elemento suma complegidad al sistema es necesario para poder desacoplar los otros sistemas y que evolucionen en forma independiente.

## SUPUESTOS
- El acceso a la informacion de dispositivos en Grafana se maneja con autorizacion a nivel de dashboard.
- El comun denominador de un bien en los distinttos sistemas es el IDENTIFICADOR UNICO DE BIEN. Asignado al momento del alta del BIEN




## TODOs
- INCORPORAR OIDC
- ALMACENAR LOS MANUALES DE MANTENIMIENTO EN ALGO COMO MINIO EN VEZ DE LA BASE DE DATOS.
