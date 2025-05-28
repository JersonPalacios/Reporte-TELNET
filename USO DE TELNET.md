# Vulneración mediante servicio Telnet

##  1. Descubrimiento de servicios con Nmap

Antes de realizar cualquier ataque, es fundamental identificar los servicios que se encuentran activos en la máquina objetivo. Para ello, utilizamos la herramienta nmap con el siguiente comando:
```bash
nmap -sV 192.168.171.133
```
Esto nos permite identificar qué puertos están abiertos y qué servicios están corriendo. En nuestro caso, se detectó que el puerto 23 correspondiente a Telnet está abierto.

![Captura de pantalla 2025-05-28 015905](https://github.com/user-attachments/assets/3f1342e0-b7a2-42ec-be1e-b792ccc4ac33)

## 2. Análisis del servicio Telnet con Metasploit

Una vez identificado que Telnet está activo, utilizamos Metasploit para inspeccionar más a fondo el servicio. Iniciamos Metasploit con:
```bash
msfconsole
```
Este comando carga la consola interactiva de Metasploit desde donde se pueden buscar, configurar y ejecutar módulos de escaneo, explotación y post-explotación.

![image](https://github.com/user-attachments/assets/6236ce28-fb43-4d84-969d-e006597e36f2)

### 2.1. Para buscar módulos específicos que trabajen con Telnet, utilizamos el comando:
```bash
search telnet_login
```
### 2.2. Buscar módulos relacionados con Telnet
Este comando busca en la base de datos de Metasploit todos los módulos relacionados con "telnet_login". Es útil para encontrar rápidamente módulos que nos permitan interactuar con ese servicio.

![image](https://github.com/user-attachments/assets/c2b907cf-2a64-41cc-9bda-3d2f6b4cf0d9)

### 2.3. Selección del módelo de autenticación Telnet

Elegimos el módulo auxiliar para hacer un intento de autenticación por fuerza bruta:
```bash
use auxiliary/scanner/telnet/telnet_login
```
Este módulo intenta autenticarse en el servicio Telnet usando combinaciones de usuario y contraseña. Es útil para detectar configuraciones débiles o credenciales por defecto.

![image](https://github.com/user-attachments/assets/df9e7609-620f-45cb-92c5-c8523f7650d3)

### 2.4. Ver las opciones del módulo

Antes de ejecutar el módulo, revisamos las opciones configurables:
```bash
show options
```
Muestra los parámetros requeridos por el módulo, como RHOSTS (IP del objetivo), USER_FILE, PASS_FILE, THREADS, etc. Esto nos permite saber qué necesitamos configurar antes de ejecutar.

![image](https://github.com/user-attachments/assets/b0ed1a79-18e0-48c3-9a9c-190898fec1d6)

### 2.5. Asignamos la dirección IP del metasploit

```bash
set RHOSTS 192.168.171.133
```
![image](https://github.com/user-attachments/assets/c103ec42-92a1-4dba-97a2-af1d91348a34)

### 2.6. Ejecutamos el módulo

Iniciamos el escaneo y los intentos de autenticación con:
```bash
run
```
![image](https://github.com/user-attachments/assets/b75de128-acba-4721-a01e-472c2ffe26c4)

## 3. Autenticación al servicio Telnet con credenciales por defecto

Dado que en el banner se nos indica un posible usuario y contraseña (msfadmin/msfadmin), probamos ingresar manualmente a la máquina utilizando Telnet:
```bash
telnet 192.168.222.131 23
```
Cuando el sistema lo solicita, se ingresan las siguientes credenciales:

Usuario: msfadmin

Contraseña: msfadmin

Una vez autenticado correctamente, obtenemos acceso al sistema remoto mediante Telnet.

![image](https://github.com/user-attachments/assets/4bbdbf6b-fb87-4733-953d-3759a400cce3)



