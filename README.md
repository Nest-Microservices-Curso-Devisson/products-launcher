## Dev
1. Clonar el repositorio
2. Crear un .env basado en el .env-template
3. Ejecutar el comando `git submodule update --init --recursive`
4. Ejecutar el comando `docker compose up --build` 

NOTA: si para el ambiente de desarrollo el contenedor no queda escuchando cambios con nest por el --watch se puede hacer uso del nodemon
para hacerlo de manera sencilla podemos hacerlo asi:

1. En cada dockerfile del contenedor realizar la instalacion global ``RUN npm install -g nodemon``
2. Modificar los package para que el npm start:dev quede asi segun corresponda: 

Para los que no tienen prisma:

`` "start:dev": "nodemon --watch src --exec ts-node -r tsconfig-paths/register src/main.ts", ``

Para los que tienen prisma:

`` "start:dev": "npm run docker-start && nodemon --watch src --exec ts-node -r tsconfig-paths/register src/main.ts", ``

3. Agregar en el docker compose la variable de entorno a cada contenedor: 

`` - CHOKIDAR_USEPOLLING=true `` 

Chokidar es una biblioteca de Node.js que se utiliza para observar cambios en el sistema de archivos. Es una de las dependencias utilizadas por nodemon para reiniciar aplicaciones automáticamente cuando se detectan cambios en los archivos del proyecto.



### Pasos para crear los Git Submodules


1. Crear un nuevo repositorio en GitHub
2. Clonar el repositorio en la máquina local
3. Añadir el submodule, donde `repository_url` es la url del repositorio y `directory_name` es el nombre de la carpeta donde quieres que se guarde el sub-módulo (no debe de existir en el proyecto)
```
git submodule add <repository_url> <directory_name>
```
4. Añadir los cambios al repositorio (git add, git commit, git push)
Ej:
```
git add .
git commit -m "Add submodule"
git push
```
5. Inicializar y actualizar Sub-módulos, cuando alguien clona el repositorio por primera vez, debe de ejecutar el siguiente comando para inicializar y actualizar los sub-módulos
```
git submodule update --init --recursive
```
6. Para actualizar las referencias de los sub-módulos
```
git submodule update --remote
```


## Importante
Si se trabaja en el repositorio que tiene los sub-módulos, **primero actualizar y hacer push** en el sub-módulo y **después** en el repositorio principal. 

Si se hace al revés, se perderán las referencias de los sub-módulos en el repositorio principal y tendremos que resolver conflictos.

