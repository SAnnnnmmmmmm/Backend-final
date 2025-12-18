<br clear="both">

<div>
  <img style="100%" src="https://capsule-render.vercel.app/api?type=waving&height=100&section=header&reversal=false&text=Proyecto%20Final&fontSize=70&fontColor=FFFFFF&fontAlign=50&fontAlignY=50&stroke=-&descSize=20&descAlign=50&descAlignY=50&textBg=false&color=random"  />
</div>

###

<p align="left">Empezamos creando un repositorio en githgub para guardar los cambios y subirlo ahi</p>

###

<p align="left">En el visual hay que hacer el el entorno virtual(venv) para decaragar las librerias ,creamos la carpeta de .gitignore para que los archivos importantes como el venv no se suban al repositorio,luego hay que descargar librerias externas a python como fastapi,firebase-admin,uvicorn con los comandos , con el siguiente comando en la terminal:pip install nombre_de_la_libreria,una vez decargado creamos el archivo requirements.text,en ese archivo ponemos los nombres de las librerias que usemos.</p>

###

<p align="left">explicacion del codigo:<br><br>from fastapi import FastAPI<br>import firebase_admin<br>from firebase_admin import credentials, firestore<br>from pydantic import BaseModel<br>cred = credentials.Certificate("firebase.json")<br>firebase_admin.initialize_app(cred)<br>db = firestore.client()<br>app =FastAPI()<br>esta parte del codigo nos sirve para:<br>Importación de Herramientas<br>FastAPI: El framework para construir las rutas (URLs) de tu aplicación.<br><br>firebase_admin: La librería oficial para interactuar con los servicios de Google Firebase.<br><br>BaseModel (Pydantic): Se usa para definir la estructura de los datos que vas a recibir o enviar (esquemas).<br><br>2. Conexión con Firebase<br>credentials.Certificate("firebase.json"): Carga tu llave privada de seguridad. Sin este archivo, el código no tiene permiso para entrar a tu base de datos.<br><br>firebase_admin.initialize_app: Inicia la conexión oficial entre tu servidor y el proyecto de Firebase.<br><br>db = firestore.client(): Crea una variable llamada db que es el "puntero" hacia la base de datos Firestore. La usarás después para guardar, leer o borrar datos.<br><br>3. Creación de la Aplicación<br>app = FastAPI(): Es el motor de tu API. A partir de aquí, usarás @app.get o @app.post para crear las rutas que tus usuarios visitarán.</p>

###

<p align="left">@app.get("/health")<br>def obtener_salud_sitio():<br>    return "Santinoo"<br>class Usuario (BaseModel):<br>    nombre:str<br>    email:str<br>    edad:int<br>    contraseña:str<br>    repetir_contraseña:str<br>class Curso (BaseModel):<br>    precio:int<br>    nombre:str<br>    descripcion:str<br>    duracion:int<br>Qué hace: Cuando alguien visite la URL tu-servidor.com/health, el servidor responderá con el texto "Santinoo".<br><br>Nota técnica: Aunque funciona, en FastAPI es mejor práctica devolver un diccionario (JSON), por ejemplo: return {"status": "ok", "user": "Santinoo"}.<br><br>2. Modelos de Pydantic (Usuario y Curso)<br>Estas clases actúan como validadores. Si alguien intenta registrar un usuario pero olvida el email o pone una letra donde va la edad, FastAPI rechazará la petición automáticamente con un error claro.<br><br>Usuario: Define los campos obligatorios para registrar a alguien.<br><br>Curso: Define las propiedades de los productos o contenido que vas a ofrecer.</p>

###

<p align="left">@app.get("/usuarios")<br>def obtener_usuarios():<br>    collection = db.collection("usuarios").stream()<br>    return [c.to_dict() for c in collection]<br>@app.post("/usuarios")<br>def crear_usuario (usuario:Usuario):<br>    if usuario.contraseña != usuario.repetir_contraseña:<br>        return "Las contraseñas no coinciden"<br>    del usuario.repetir_contraseña<br>    db.collection("usuarios").add(usuario.dict())<br>    return "usuario creado con exito"<br>@app.get("/cursos")<br>def obtener_cursos():<br>    collection = db.collection("cursos").stream()<br>    return [c.to_dict() for c in collection]<br>@app.post("/cursos")<br>def crear_curso (curso:Curso):<br>    if curso.nombre == "admin":<br>        return "el nombre no puede llevar admin"<br>    db.collection("cursos").add(curso.dict())<br>    return "curso creado con exito"<br>GET y POST<br>GET (/usuarios y /cursos): Usas .stream(), que es la forma más eficiente de Firestore para traer todos los documentos de una colección. Luego usas una list comprehension para transformar cada documento en un diccionario de Python legible.<br><br>POST (/usuarios y /cursos): Recibes un objeto de Pydantic, lo validas con tus if y, si todo está bien, lo guardas en la base de datos.</p>

###

<p align="left">antes de todo esto debemos crearnos una cuenta en firebase en google,una vez creada nuestra cuenta en firebase debemos ir a crear un proyecot de firebase nuevo,le ponemos el nombre que queramos ,damos los permisos necesarios y cuanod nos pida una cuenta de google analythics,ponemos la opcion de default,ya con eso tendriamos nuestro proyecto de firebase creado cuando tengamos el proyecto creado nos va a aparecer un menu co una columna del lado izquierdo,apretamos en la opcion  que dice compilacion,de ahi apretamos firebase database,una vez ahi dentro,apretamos el donde dice crear base de datos , en ubicacion elegimos la opcion mas cercana a nuestro pais,ahi ponemos continuar y ya estaria creada nuestra base de datos,pero no termina ahi,una vez dentro de la base de datros nos vamos a la tuerquita que aparece arriba a la izquierda,apretanos configuracion del proyecto  y vamos a cuentas de servicio,elejimos el lenguaje que necesitemos(en nuestro caso python) y le damos a generar nueva clave privada,una vez hecho eso se nos va a descargar un archivo .json que lo vamos a tener que poner en la carpeta raiz de nuestro proyecto osea main.py o el nombre que nosotros le hayamos puesto,este archivo .json es basicamente nuestro DNI</p>

###

<p align="left">Imagina que Google (Firebase) tiene un edificio gigante lleno de cajas fuertes (tus bases de datos). Como el edificio es privado, no dejan pasar a cualquiera. Cuando tu código intenta guardar un usuario, Google pregunta: "¿Quién eres y por qué quieres escribir aquí?". El archivo .json contiene una Clave Privada que le dice a Google: "Soy el dueño legítimo de este proyecto y tengo permiso para hacer cambios".Por eso es muy importante que este archivo este dentro de la carpeta de .gitignore</p>

###