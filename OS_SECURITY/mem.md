# Tema Presentación Resumen

QubeOs: SO centrado en la seguridad personal del usuario sobre todo a la hora de ejecutar untrusted code.

![alt text](https://raw.githubusercontent.com/softportal/MASTER_DOCS/master/OS_SECURITY/qubeosLogo.png)

## Alumnos
    SERGIO A SEMEDI BARRANCO <ssemedi@ucm.es>
    LUCAS SEGARRA FERNANDEZ <lsegar01@ucm.es>


## INDICE

* ¿Que es?
    * SUB
* ¿Que es?
* ¿Que es?
* ¿Que es?
* ¿Que es?
* ¿Que es?


## [Presentación](http://slides.com/master_iot/qube)

Enlace a los slides utilizados para la presentación de clase.

### ¿Qué es QubeOs?

* Es un sistema operativo orientado a la seguridad.
* Es un _free and open-source_ (FOSS).
* QUBEOS es un SO Desktop (personal use) que aprovecha el aislamiento de un hypervisor baremetal como puede ser el Proyecto libre X en + IntelVd (hardware)


### Problemas de los sistemas operativos actuales

Los sistemas operativos más populares para informática de escritorio, como Windows Mac OS X o los basados en GNU/Linux resultan insatisfactorios en cuanto a la seguridad. El principal problema es su inhabilidad para proporcionar aislamiento efectivo entre distintos programas ejecutandose en la misma máquina.

Por ejemplo si el _web browser_ del usuario se ve comprometido el sistema operativo suele tener dificultades para proteger otras aplicaciones y datos de ser verse igualmente comprometidos. Análogamente, si algún componente del nucleo del sistema se ve comprometido (driver wifi) ninguno de los OS que mencionamos en el parrafo anterior puede defenderse de un compromiso total de las aplicaciones y datos del usuario.

Tal situación es el resultado directo de algunas decisiones de diseño arquitectónico, que incluyen sobrecomplejitud de la API del OS, diseño inseguro de interfaces gráficas y la arquitectura monolítica del kernel.

Parece más sencillo conseguir cierto aislamiento entre algunos de los procesos y datos del usuario que pretender solucionar todos los posíbles bugs y vulnerabilidades de cualquier sistema.

#### Por que Qube OS respecto a lo clasico (kernel monoliticOS)

Estamos acostumbrados a los trending OSs que vienen hasta preinstalados en hardware y ya compilados (algunos casos) en el que solo tienes el binario, además la arquitectura monolítica del Os no facilitan las labores de seguridad

![alt text](https://raw.github.com/softportal/MASTER_DOCS/blob/master/OS_SECURITY/monolithic.jpg)

Ejemplos punto de entrada:

           Correo
           Navegacion no deseada
           Nic -> Wifi
           Vulnerabilidades Software
           ...

Clave en cierta medida para la seguridad => Software libre (Community based).



## Introducción basica hypervisores

Hypervisor es software, firmware or hardware que permite la creación de maquinas virtuales.
Podemos distinguirlos en dos:

* bare metal: Actuan sobre el propio HW
* hosted: Estan por encima del SO

![alt text](https://raw.githubusercontent.com/softportal/MASTER_DOCS/master/OS_SECURITY/hyperviseur.png)

Desde el punto de vista de la seguridad interesa **bare metal**

####  detalles tecnicos comparativos XEN - KVM

Basicamente Comparamos una arquitectura Linux based (kvm) que funciona con el scheduler y los user processes de linux sumados al modulo del kernel (hardware) con XEN:

    * small footprint, interface -> microkernel design
    * Linux based (Linux system running as dom0)
    * Driver isolation: main device driver for a system to run inside of a virtual machine
    * Paravirtualization: Fully paravirtualized guests have been optimized to run as a virtual machin 

#### Gestion de memoria / espacio direcciones | IO EMULATOR

## Arquitectura QUBE OS orientada seguridad

Implementa una estrategia de seguridad por aislamiento, para ello usa las tecnologías vistas anteriormente (Xen, Linux, Vt...) sumado a una arquitectura basada en dominios.

- Networking subsystem (IOMMU/VT-d).
- Storage subsystem
    - USB y Drivers parecdos aislados en una maquina virtual sin privilegios.
- Graphics
- User Apps/VMs

Esto hace que comprometer cualquiera de los sistemas aislados no suponga un peligro real global.

### Algunos Sistemas

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


#### Reflexión.

QubeOs es un sistema operativo razonablemente seguro, que juntando varias tecnologias consigue ser una optativa bastante deseable en cuanto a cualquier uso informático orientado a Desktop se refiere.

Se detallarán mas detalles técnicos en la presentación al igual de un caso de uso.


## Fuentes:

* Wikipedia.com
* qubes-os.org
* Manual pdf arquitectura QUBE OS
* Hacker News
* doc.opensuse.org/documentation/leap/virtualization/html/book.virt/cha.kvm.intro.html
* researchgate.net/figure/Xen-architecture-29_fig4_261411692
* wiki.xen.org/wiki/Xen_Project_Software_Overview
