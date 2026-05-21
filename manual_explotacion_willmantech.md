# AEE: Explotación Tecnológica en ERP/CRM

## 1. Introducción y Arquitectura

Este manual proporciona las instrucciones de explotación, administración y mantenimiento del sistema ERP de **WillmanTech S.L.** El objetivo es garantizar la disponibilidad, integridad y confidencialidad del sistema.

El ERP está compuesto por tres módulos principales interconectados:
* **Módulo Comercial y Clientes (CRM):** Gestión de cuentas, presupuestos y pedidos.
* **Módulo de Facturación y Finanzas:** Emisión de facturas, control de cobros, impuestos y contabilidad automatizada.
* **Módulo de Informes y Analítica:** Renderizado de reportes financieros y operacionales en tiempo real.

El sistema se despliega mediante una arquitectura de microservicios contenedorizados utilizando Docker Compose. Esta estructura aísla los componentes, facilitando la escalabilidad y el mantenimiento de todo el sistema.

## 2. Guía de Instalación y Reinstalación

Este apartado describe el procedimiento para levantar el entorno del ERP desde cero en un servidor limpio (Bare Metal o VPS) con arquitectura Linux de 64 bits.

Antes de comenzar debemos comprobar que contamos con un motor de contenedores (Docker Compose en este caso), con un sistema de gestión de base de datos (Docker incluye PostgreSql ya que es su imagen oficial) y por última memoria y almacenamiento suficiente.

El despligue requiere un archivo .env en la raíz del proyecto con el que gestionar las credenciales y parámetros para conectarse de forma segura. 

Por último solo queda el levantamiento del entorno y la verificación en el mismo.

## 3. Seguridad y Control de Acceso
La seguridad del ERP de WillmanTech S.L. se basa en el principio de mínimo privilegio y en el control de acceso basado en roles.

| Rol de Usuario | Módulo Comercial  | Módulo Facturación | Configuración del Sistema |
| :--- | :---: | :---: | :---: |
| **Administrador** | Acceso Total | Acceso Total  | Acceso Total  |
| **Contable / Gestor** | Solo Lectura | Acceso Total  | Sin Acceso  |
| **Comercial** | Acceso Total  | Solo Creación de Presupuestos | Sin Acceso  |

Para eliminar riesgos de accesos no autorizados o ataques de fuerza bruta, la autenticación fuerza de forma nativa los siguientes requisitos de directiva de grupo:
* **Longitud Mínima:** Las contraseñas deben tener un mínimo de 12 caracteres.
* **Complejidad Requerida:** Inclusión obligatoria de al menos una letra mayúscula, una letra minúscula, un dígito numérico y un carácter especial.
* **Rotación Forzada:** Caducidad automática de credenciales cada 90 días naturales, además de esto el sistema impedirá la reutilización de las últimas 3 contraseñas.
* **Política de Bloqueo (Lockout):** Tras 5 intentos fallidos consecutivos en un tiempo de 15 minutos, la cuenta de usuario se suspenderá automáticamente. Solo un Administrador podrá desbloquear la cuenta o en el caso de no ser posible se liberará automáticamente pasadas 24 horas.

## 4. Procedimiento de Backup y Restauración
El Plan de Continuidad de Negocio de WillmanTech S.L. exige mantener copias de seguridad constantes tanto de la base de datos relacional como del almacén de archivos.

El respaldo se realiza en caliente utilizando la herramienta nativa pg_dump, garantizando la integridad referencial sin necesidad de interrumpir el servicio de la aplicación.

## 5. Flujo Operativo de Facturación e Informes

El proceso para hacer una factura legal es una línea recta muy fácil de seguir, pensada precisamente para que nadie tenga dificultad en el proceso.

**Navegación:** El usuario con rol Administrador accede a Facturación > Nueva Factura.

**Selección de Entidad:** Al escribir las primeras letras del cliente en el buscador dinámico, el sistema realiza una llamada no sincronizada mediante AJAX (*) para autocompletar la ficha fiscal del cliente.

**Carga de Conceptos:** Se añaden las líneas de detalle introduciendo el artículo/servicio, la cantidad y el precio unitario. Los impuestos aplicables se calculan y desglosan en tiempo real en el pie del formulario.

**Confirmación y Persistencia:** El usuario presiona el botón "Confirmar y Emitir Factura". El sistema valida que el documento contenga campos obligatorios coherentes, le asigna un número correlativo único e inalterable y guarda el registro con estado EMITIDA en la base de datos.

(*) AJAX (Asynchronous JavaScript And XML) es una técnica de desarrollo web que permite a las páginas actualizarse y comunicarse con un servidor en segundo plano, teniendo como ventaja la capacidad de modificar el contenido de una web sin necesidad de recargar la página completa.

## Generación del Informe Dinámico (QWeb XML)

En este apartado se debe diseñar y programar una estructura XML para la plantilla de un informa de facturas personalizadas para la empresa WillmanTech.S.L. utilizando QWeb.

Para ello se accede a odoo en modo administrador para poder acceder al apartado "tecnico", una vez dentro de este apartado se buscará el apartado vistas y dentro de vistas se buscará mediante clave "invoice" y se entrará en la segunda vista que aparece.

<img width="740" height="39" alt="image" src="https://github.com/user-attachments/assets/bca64db1-9dc6-462f-94a5-df8afdbadc59" />

Este archivo que es el que se usará para la personalización. En el traduciremos cada texto que aparece en ingles además de ir personalizando conforme la empresa desee.

Además se comentará cada parte del código para que si a la hora de modificarlo o refactorizarlo sea mucho más facil para el técnico que lo realice.

## Interoperabilidad de Datos (Extracción JSON/XML)

Para este apartado se deberá adjuntar una factura electronica simplificada que cumpla con las especificaciones del estandar UBL (Universal Bussines Language) junto a los nombres de los espacios para los componentes que han sido agregados (cac, bcb...).

<img width="888" height="388" alt="image" src="https://github.com/user-attachments/assets/f2d282a8-e677-40b5-92a9-5dfb260d7674" />


Para ello se debe entrar nuevamente en Odoo, pero a diferencia de en el apartado anterior entraremos a las facturas existentes que han sido creadas anteriormente. Se elegirá una de estas y se exportará como xml.
Esto proporcionará el archivo xml con el estandar UBL y los nombres de espacios que buscamos. Una vez obtenido se modificarán algunos datos faltantes o que no están correctos del todo.

## Citas

W. Willman Acosta, "Refactorización", Willman Acosta, . [Online]. Available: https://docs.google.com/document/d/1G209kpkvMFlEUbU4CH87hF10F0nqILof_LzvSATLxz0/edit?tab=t.0#heading=h.nvj08m9tbkbp. [Accessed: 05-21-2026].

W. Willman Acosta, "La Explotación Tecnológica en Sistemas de Gestión Empresarial (ERP/CRM)", Willman Acosta, . [Online]. Available: https://docs.google.com/document/d/1DHxZ9GXbE7yWfzH-1XwHj6JrkAj-MD-q28ghienvtxY/edit?tab=t.0#heading=h.s5qlwwef4wtv. [Accessed: 05-21-2026].

## Consultas IA
 
| Consulta 1 |  |
| :---- | :---- |
| **Agente:** | Claude Haiku 4.5 |
| **Propmt:** | Añade los comentarios faltantes a este archivo arriba de los if/else desglosando la funcionalidad de cada uno. |
| **Respuesta textual:** | Ha resuelto el prompt escribiendo comentarios en el xml explicando algunos if/else |
