# Integración de Gunicorn y Nginx en Ubuntu (WSL)

1. Instalación de Ubuntu en Windows (WSL):
Instala Ubuntu en Windows utilizando Windows Subsystem for Linux (WSL) siguiendo las instrucciones proporcionadas por Microsoft.

2. Instalación de Nginx:
Ejecuta el siguiente comando en tu terminal de Ubuntu para instalar Nginx:
- sudo apt update
- sudo apt install nginx
  
3. Verificación de Python:
- Asegúrate de tener Python instalado en tu sistema ejecutando python3 --version. Si no está instalado, puedes instalarlo con sudo apt install python3.

4. Instalación de Gunicorn:
Instala Gunicorn utilizando pip3:
- sudo apt install python3-pip
- pip3 install gunicorn
  
5. Acceso al proyecto Flask:
- Navega hasta el directorio de tu proyecto Flask en la terminal de Ubuntu.

6. Instalación de Redis y Flask:
Instala Redis y Flask dentro de tu entorno virtual (si estás utilizando uno) ejecutando:
- pip3 install redis flask
  
7. Configuración de Nginx:
Edita el archivo de configuración de Nginx con el editor de texto de tu elección (por ejemplo, nano) utilizando el comando:
- sudo nano /etc/nginx/sites-available/default
- Configura Nginx para que actúe como un proxy inverso agregando la configuración adecuada. Aquí tienes un ejemplo:
  
server {
    listen 80;
    server_name tu_dominio_o_IP_publica;

    location / {
        proxy_pass http://127.0.0.1:1111;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
- Guarda los cambios y cierra el editor de texto.

8. Inicio del servidor de Gunicorn:
Ejecuta el siguiente comando en el directorio de tu proyecto Flask para iniciar el servidor Gunicorn:
- gunicorn -w 4 -b 127.0.0.1:1111 main:app
- Donde main es el nombre del archivo Python principal y app es el nombre de la instancia de tu aplicación Flask dentro de ese archivo.

9. Inicio de Nginx:
Reinicia Nginx para aplicar los cambios en la configuración:
- sudo systemctl restart nginx

10. Verificación:
Accede a tu aplicación Flask a través del navegador utilizando la dirección IP pública de tu servidor o el dominio configurado. Deberías ver tu aplicación funcionando correctamente.
