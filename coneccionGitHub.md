# CONECTAR CON SSH GITHUB
1. Abrir la terminal (Puede se GIT BAS o cmd)
2. Copiar y pegar, reemplazar el correo electronico
ssh-keygen -t ed25519 -C "juan15le1996l@gmail.com"
Se puede asignar un nombre de archivo y una contraseña [Enter] para nombre por defecto y para dejar sin contraseña
3. Iniciamos  el agente
eval "$(ssh-agent -s)"
4. Añadimos la clave generada al estado global con el siguiente comando, estamos tomando el nombre por defecto "id_ed25519"
ssh-add ~/.ssh/id_ed25519
5. Abrimos el archivo y copiamos la clave
Agregamos la clave en GitHub /Setting/SSH ANS GPG Keys
6. Probamos si funciona con el siguiente comando
ssh -T git@github.com
escribimos "yes" y [Enter]