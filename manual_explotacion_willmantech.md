# AEE: Explotación Tecnológica en ERP/CRM

## Generación del Informe Dinámico (QWeb XML)

En este apartado se debe diseñar y programar una estructura XML para la plantilla de un informa de facturas personalizadas para la empresa WillmanTech.S.L. utilizando QWeb.

Para ello se accede a odoo en modo administrador para poder acceder al apartado "tecnico", una vez dentro de este apartado se buscará el apartado vistas y dentro de vistas se buscará mediante clave "invoice" y se entrará en la segunda vista que aparece.

![alt text](src/image.png)

Este archivo que es el que se usará para la personalización. En el traduciremos cada texto que aparece en ingles además de ir personalizando conforme la empresa desee.

Además se comentará cada parte del código para que si a la hora de modificarlo o refactorizarlo sea mucho más facil para el técnico que lo realice.

## Interoperabilidad de Datos (Extracción JSON/XML)

Para este apartado se deberá adjuntar una factura electronica simplificada que cumpla con las especificaciones del estandar UBL (Universal Bussines Language) junto a los nombres de los espacios para los componentes que han sido agregados (cac, bcb...).

![alt text](src/image2.png)

Para ello se debe entrar nuevamente en Odoo, pero a diferencia de en el apartado anterior entraremos a las facturas existentes que han sido creadas anteriormente. Se elegirá una de estas y se exportará como xml.
Esto nos dará el archivo xml con el estandar UBL y los nombres de espacios que buscamos. Una vez obtenido se modificarán algunos datos faltantes o que no están correctos del todo.

## Consultas IA

#1 
### Agente:
Claude Haiku 4.5
### Propmt:
Añade los comentarios faltantes a este archivo arriba de los if/else desglosando la funcionalidad de cada uno.
### Respuesta textual:
Ha resuelto el prompt escribiendo comentarios en el xml explicando algunos if/else
