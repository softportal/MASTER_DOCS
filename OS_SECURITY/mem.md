# QubeOs: Un Sistema Operativo confiable

SO centrado en la seguridad personal del usuario sobre todo a la hora de ejecutar untrusted code.
\vskip 0.2in
Memoria creada para la asignatura de __Seguridad__, Master IOT (UCM)

\vskip 0.5in

![Portada](qubeosLogo.png)

\vskip 0.5in

## Alumnos
    SERGIO SEMEDI BARRANCO   <ssemedi@ucm.es>
    LUCAS SEGARRA FERNANDEZ  <lsegar01@ucm.es>

\newpage
## INDICE

\vskip 0.4in

* RESUMEN ..............................

* ¿Que es? .............................

    * Importancia ......................

    * Problema SOs actuales ............

    * Por que? .........................

* Hypervisores .........................

    * Detalles Técnicos XEN - KVM ......

    * Aspectos relevantes ..............

* Arquitectura QUBEOS ..................

* Dominios .............................

    * APPVM ............................

    * STORAGE ..........................

    * NETWORKING .......................

    * GUI ..............................

* Caso de Uso ..........................

* Reflexión Final ......................

* Fuentes ..............................


\newpage
## RESUMEN
### [Presentación](http://slides.com/master_iot/qube)


Enlace a los slides utilizados para la presentación de clase.

\newpage
## ¿Qué es QubeOs?

* Es un sistema operativo orientado a la seguridad.
* Es un _free and open-source_ (FOSS).
* QUBEOS es un SO Desktop (personal use) que aprovecha el aislamiento de un hypervisor baremetal como puede ser el Proyecto libre X en + IntelVd (hardware)

### Importancia

\newpage
### Problemas de los sistemas operativos actuales

Los sistemas operativos más populares para informática de escritorio, como Windows Mac OS X o los basados en GNU/Linux resultan insatisfactorios en cuanto a la seguridad. El principal problema es su inhabilidad para proporcionar aislamiento efectivo entre distintos programas ejecutandose en la misma máquina.

![OSs](OSs.png)

Por ejemplo si el _web browser_ del usuario se ve comprometido el sistema operativo suele tener dificultades para proteger otras aplicaciones y datos de ser verse igualmente comprometidos. Análogamente, si algún componente del nucleo del sistema se ve comprometido (driver wifi) ninguno de los OS que mencionamos en el parrafo anterior puede defenderse de un compromiso total de las aplicaciones y datos del usuario.

Tal situación es el resultado directo de algunas decisiones de diseño arquitectónico, que incluyen sobrecomplejitud de la API del OS, diseño inseguro de interfaces gráficas y la arquitectura monolítica del kernel.

Parece más sencillo conseguir cierto aislamiento entre algunos de los procesos y datos del usuario que pretender solucionar todos los posíbles bugs y vulnerabilidades de cualquier sistema.


\newpage
#### Por que Qube OS respecto a lo clasico (kernel monoliticOS)

Estamos acostumbrados a los trending OSs que vienen hasta preinstalados en hardware y ya compilados (algunos casos) en el que solo tienes el binario, además la arquitectura monolítica del Os no facilitan las labores de seguridad

![Arquitectura monolítica](monolithic.jpg)


Ejemplos punto de entrada:

           Correo
           Navegacion no deseada
           Nic -> Wifi
           Vulnerabilidades Software
           ...

Clave en cierta medida para la seguridad => Software libre (Community based).



\newpage
## Introducción basica hypervisores

Hypervisor es software, firmware or hardware que permite la creación de maquinas virtuales.
Podemos distinguirlos en dos:

* bare metal: Actuan sobre el propio HW
* hosted: Estan por encima del SO

![hypervisores: tipos](hyperviseur.png)


Desde el punto de vista de la seguridad interesa **bare metal**

####  detalles tecnicos comparativos XEN - KVM

Basicamente Comparamos una arquitectura Linux based (kvm) que funciona con el scheduler y los user processes de linux sumados al modulo del kernel (hardware) con XEN:

    * small footprint, interface -> microkernel design
    * Linux based (Linux system running as dom0)
    * Driver isolation: main device driver for a system to run inside of a virtual machine
    * Paravirtualization: Fully paravirtualized guests have been optimized to run as a virtual machine

#### Gestion de memoria / espacio direcciones | IO EMULATOR

\newpage
## Arquitectura QUBE OS orientada seguridad

Implementa una estrategia de seguridad por aislamiento, para ello usa las tecnologías vistas anteriormente (Xen, Linux, Vt...) sumado a una arquitectura basada en dominios.

![QubeOS overview](qubeos_main.png)

- Networking subsystem (IOMMU/VT-d).
- Storage subsystem
    - USB y Drivers parecdos aislados en una maquina virtual sin privilegios.
- Graphics
- User Apps/VMs

Esto hace que comprometer cualquiera de los sistemas aislados no suponga un peligro real global.

\newpage
## Dominios

#### AppVM

Aplicaciones directas para el usuario que podrá usar como quiera, cada una tiene su espacio de dominio aislado.
Ahorro de Disco based, las aplicaicones pueden compartir un FS RO.

Para separar cualquier tipo de problema relacionado con el FS tenemos el Storage domain.

#### storage

QubeOs siempre tratará de hacer un uso del disco (hardware) óptimo, empleara técnicas como pueden ser un Copy-on-write Filesystem.

Esta clase de arquitectura es insegura de tener en el dominio administrativo.

Tiene acceso al controlador de disco/USB o CD/DVD.

#### GUI X

Este dominio accede directamente a los graficos del dispositivos y además IO como puede ser el raton o el teclado.
Sera por lo tanto el encargado de lanzar un Servidor X.

Qube OS Usa un app viewer que te permite interactuar con el X server de forma independiente para cada AppVM.

Este dominio es muy importante a nivel de ser comprometido por lo tanto **no debería ejecutar código externo**.

#### networking,

Aqui encontramos lo referente a la parte de redes (drivers NICS, stacks de protocolos como 802.11, tcp/ip...).
Interactua con las NICs físicas mediante un dominio (network domain) sin privilegios, todo gracias a Vt-d.

\newpage
## Dominio APPVM

\newpage
## Dominio STORAGE

\newpage
## Dominio NETWORKING

\newpage
## Dominio GUI

\newpage
## Caso de Uso

Enfocado desde el punto de vista del usuario presentamos una visión práctica del modelo de seguridad de Qubes OS.

Las máquinas virtuales (en adelante VMs), también llamadas dominios, son los _bloques de construcción_ primarios en el modelo de seguridad de Qubes OS, la tarea principal del sistema operativo consiste en aislar las VMs entre ellas, de tal forma que aunque una se viera comprometida, las demás seguirían siendo seguras de utilizar. Consideramosm para un ejemplo práctico, que evidencia como podría esto contribuir a mejorar la seguridad de cualquier usuario, las siguientes VMs (dominios) definidas por el usuario.

- La __Random VM__ utilizada para la navegación típica web, buscar en Google, leer noticias, ver vídeos etc. El usuario tambiém usaría esta máquina para instalar aplicaciones en las quizá no pueda confiar completamente pero quiera probar como algunos juegos. La principal característica de esta VM es que __no contiene niguna información sensible__, con lo que su potencial compromiso supone ningún problema de seguridad para el usuario. En cualquier momento el usuario puede simplemente reiniciarla y Qubes haría seguro reverirla a un estado bueno conocido.
- __Social VM__ utilizada por ejemplo para albergar un cliente de email para su uso personal (no corporativo), para actualizar su blog privado,twitter, facebook o cualquier otro _servicio web social_. El denominador común de todo lo que el usuario haga en esta máquina, es que __están todas conectadas__ con la _identidad online_ del usuario. Es probable que si un atacante se hiciera control sobre la cuenta de email del cliente por ejemplo, _ganase_ fácilmente el control de la cuenta de twitter etc. Consecuentemente con la dependencia de todas estas aplicaciones de los mismos datos sensibles, la _identidad social del usuario_, no hay razón para separarlas entre ellas, siendo razonable albergarlas en la misma VM.
- __Shopping VM__ utilizadad para todas la compras por internet, amazon, ebay, etc. El __recurso sensible común es el número de tarjeta del usuaurio__ en este caso. Y posiblemente la dirección, telefono y nombre del usuario. Si el número de tarjeta fuese _robado_ por alguna de las páginas de venta ya estaría comprometida, por lo que el hecho de que otro página pudiera robarlo luego es completamente irrelevante. No hay razón para separar las actividades de compra en Amazon de las realizadas en una pequeña tienda local si el usuario utiliza la misma tarjeta en los dos sitios. A lo mejor un usuario más paranoico preferiría utilizar dos tarjetas de credito diferentes para distintos tipos de compras, una con un límite bajo para las compras comunes, pedir comida, comprar libros etc, y la otra con un límite más alto para otro tipo de gastos más caros. En tal caso si que podría tener dos máquinas __Shopping VM low__ y __Shopping VM hig__, en los países más desarrollados, el seguro generalizado de transacciones con tarjetas de crédito haría que tal esfuerzo no mereciese la pena.
- __Bank VM__ utilizada exclusivamente para acceder a su banco. __Permtiendo exclusivamente acceso HTTPS__ a la web del banco, y nada salvo eso.
- __Corporate VM__ utilizada para albergar el cliente de correo de la empresa, acceder a la intranet corporativa. Así que esta máquina tendría un cliente VPN, y __pernite únicamente conexiones al VPN gateway corporativo__. Aquí es también donde se almacenan y utilizan los ficheros relacionados con el trabajo del usario, como informes, hojas de cálculo, bases de datos, etc. Esta máquina también podría verse comprometida si por ejemplo un compañero de trabajo enviáse al usuario un PDF infectado que explote una vulnerabilidad del visor de PDF que utilice el usuario, Qubes no detendría que esa _explotación_ se hiciese con toda la VM pero si le impediría hacerse con otras. En cualquier caso parece probable que en tal situación los secretos de la VM de la organización se viesen comprometidas en cualquier caso, porque el colega tiene una máquina comprometida, así que si trabajan en el mismo equipo, en el mismo proyecto, todos los documentos relacionados con el proyecto se verían comprometidos en cualquier caso. Un usuario que interactuara con más de un equipo, o más de un departamento necesitaría probablemente definir más de una VM corporativa.

Esto es solo un ejemplo de TODO TERMINAR

\newpage
## Reflexión final.

QubeOs es un sistema operativo razonablemente seguro, que juntando varias tecnologias consigue ser una optativa bastante deseable en cuanto a cualquier uso informático orientado a Desktop se refiere.

Se detallarán mas detalles técnicos en la presentación al igual de un caso de uso.


\newpage
## Fuentes:

- Wikipedia.com

- qubes-os.org

- Manual pdf arquitectura QUBE OS

- Hacker News

- doc.opensuse.org/documentation/leap/virtualization/html/book.virt/cha.kvm.intro.html

- researchgate.net/figure/Xen-architecture-29_fig4_261411692

- wiki.xen.org/wiki/Xen_Project_Software_Overview
