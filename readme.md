# Creación de _DHCP Linux_ 

Para ello deberemos crear un fichero _docker-compose.yml_ y una carpeta _data_ que será donde guardará la configuración de nuestro servicio de _dhcp_

## _docker-compose.yml_
Nuestro fichero contendrá lo siguiente:
~~~
version: '3.9'
services:
    dhcpd:
        container_name: asir_servidor_dhcp
        hostname: servidor_dhcp
        network_mode: host
        image: networkboot/dhcpd
        volumes:
        - ./data:/data
~~~

## Carpeta _data_
crearemos esta carpeta en la cual tendremos dentro un fichero que crearemos llamado _dhcpd.conf_ en el cual tendremos la configuración de la _Red_ de la cual vamos a distribuir _IPs_

### _dhcpd.conf_ 
Este fichero tendra que contener algo así, variando en función de lo que necesitemos, donde otorgaremos un rango de la red para repartir IPs,tambien le otorgaremos un dns para que pueda resolver nombres...
~~~
subnet 10.0.9.0 netmask 255.255.255.0 {

  range 10.0.9.172 10.0.9.192;

  option domain-name-servers 8.8.8.8;
  option domain-name "prueba_asir.com";
  option routers 10.0.9.19;
  option subnet-mask 255.255.255.0;
  option broadcast-address 10.0.9.255;

  default-lease-time 86400;
  max-lease-time 172800;

  group {

     default-lease-time 604800;
     max-lease-time 691200;

     host apache {
         hardware ethernet 08:00:27:9B:E3:C5;
         fixed-address 10.0.9.223;
     }

  }
}
~~~ 

## Comprobación al otorgale la _IP fija_ a un cliente

Este el cliente al que le vamso a otorga la sigunete dirección IP
~~~
group {

     default-lease-time 604800;
     max-lease-time 691200;

     host apache {
         hardware ethernet 08:00:27:9B:E3:C5;
         fixed-address 10.0.9.223;
     }

  }
~~~

### Cliente
En este caso para comprobar que se le ha otorgado la _dirección IP_ correspondiente abriremos el terminal del cliente y en este caso como es Windows usaremos los sigunetes comandos:
~~~
ipconfig /all #para ver las caracteristicas de nuestras interfaces

ipconfig /renew #para solicitar una dirección ip nueva

ipconfig /release #para borrar la ip que tiene la interfaz
~~~
Como podremos apreciar en la sigunete captura le ha otorgado la IP sin problemas 

![Foto DHCP](https://github.com/Joel1747/DHCPLinux/blob/master/capturas/CapCliente.png)