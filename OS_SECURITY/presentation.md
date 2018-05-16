# Tema Presentación Resumen

    SERGIO A SEMEDI BARRANCO <ssemedi@ucm.es>
    LUCAS SEGARRA FERNANDEZ <lsegar01@ucm.es>

SO centrado en la seguridad personal del usuario sobre todo a la hora de ejecutar untrusted code.

## Porque el SO? y por que es dificil confiar en lo actual?
#### Por que Qube OS respecto a lo clasico (kernel monoliticOS)

Estamos acostumbrados a los trending OSs que vienen hasta preinstalados en hardware y ya compilados (algunos casos) en el que solo tienes el binario, además la arquitectura monolítica del Os no facilitan las labores de seguridad

Ejemplos punto de entrada:

           Correo
           Navegacion no deseada
           Nic -> Wifi
           Vulnerabilidades Software
           ...

Clave en cierta medida para la seguridad => Software libre (Community based).


QUBEOS es un SO Desktop (personal use) orientado a seguridad aprovechando la isolation de un hypervisor baremetal como puede ser el Proyecto libre X en + IntelVd (hardware)


## Introducción basica hypervisores

Hypervisor es software, firmware or hardware que permite la creación de maquinas virtuales.
Podemos distinguirlos en dos:

* bare metal: Actuan sobre el propio HW
* hosted: Estan por encima del SO

Desde el punto de vista de la seguridad interesa **bare metal**

####  detalles tecnicos comparativos XEN - KVM

Basicamente Comparamos una arquitectura Linux based (kvm) que funciona con el scheduler y los user processes de linux sumados al modulo del kernel (hardware) con XEN:

    * small footprint, interface -> microkernel design
    * Linux based (Linux system running as dom0)
    * Driver isolation: main device driver for a system to run inside of a virtual machine
    * Paravirtualization: Fully paravirtualized guests have been optimized to run as a virtual machin 

#### Gestion de memoria / espacio direcciones
#### IO emulator

## Arquitectura QUBE OS orientada seguridad
#### servidores X
#### networking,
    - File System
    - storage
    ...

## Fuentes:

* Wikipedia.com
* qubes-os.org
* Manual pdf arquitectura QUBE OS
* Hacker News
* doc.opensuse.org/documentation/leap/virtualization/html/book.virt/cha.kvm.intro.html
* researchgate.net/figure/Xen-architecture-29_fig4_261411692
* wiki.xen.org/wiki/Xen_Project_Software_Overview
