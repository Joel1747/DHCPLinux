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
         hardware ethernet 00:10:5a:f1:35:87;
         fixed-address 10.0.9.223;
     }
  }
}
~~~ 

