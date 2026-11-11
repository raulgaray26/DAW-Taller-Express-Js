# Album Express

API sencilla de fotos con Express y almacenamiento en memoria.

## Ejecutar

```sh
npm install
npm run dev
O iniciarlo con Ctrl + Alt + N en VSCode
```

El servidor escucha en http://localhost:3000. Las peticiones de ejemplo estan en
`src/prueba.http` y se pueden ejecutar con la extension REST Client de VS Code.
Ejecutalas en orden para crear, actualizar y eliminar la foto con ID 3.

## Rutas

| Metodo | Ruta | Resultado |
| --- | --- | --- |
| GET | /api/fotos | Lista las fotos |
| GET | /api/fotos/:id | Consulta una foto |
| POST | /api/fotos | Crea una foto y responde 201 |
| PATCH | /api/fotos/:id | Actualiza solo los campos enviados |
| DELETE | /api/fotos/:id | Elimina una foto y responde 204 sin cuerpo |

Los campos editables son `titulo`, `descripcion` y `calificacion`. El ID no se
modifica. Consultar, actualizar o eliminar una foto inexistente responde 404.
El titulo es obligatorio al crear una foto y no puede vaciarse al actualizarla.

## Que ocurre al reiniciar

1. Crea una foto con POST y comprueba que aparece con GET /api/fotos.
2. Deten el servidor con Ctrl+C y ejecuta otra vez `npm run dev`.
3. Vuelve a consultar GET /api/fotos.

La foto creada desaparece y vuelven las dos fotos iniciales. Los cambios y las
eliminaciones tambien se pierden: el arreglo `fotos` vive en la memoria del
proceso y se inicializa cada vez que arranca el servidor. No se guarda en un
archivo ni en una base de datos. PostgreSQL permitira conservar los datos entre
reinicios; no se incorpora todavia en este ejercicio.

## Propuesta si la API tuviera 30 rutas

Separaria los archivos por responsabilidad, agrupando las rutas por recurso:

```text
src/
  server.js                 # Configuracion, middleware e inicio del servidor
  routes/
    fotos.routes.js         # Rutas de fotos con express.Router()
  controllers/
    fotos.controller.js     # Funciones para listar, crear, editar y eliminar
  data/
    fotos.js                # Arreglo en memoria y contador de IDs
```

En `server.js` registraria el router con `app.use('/api/fotos', fotosRouter)`.
Para otros recursos agregaria sus archivos de rutas y controladores. Por ahora
el codigo se mantiene en un solo archivo porque hay pocas rutas.

El `.gitignore` excluye dependencias, variables de entorno, registros y archivos
del sistema. `package-lock.json` se conserva para instalar las mismas versiones.
