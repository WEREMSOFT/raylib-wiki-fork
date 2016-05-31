*NOTE: This tutorial is written in Spanish, it should be translated to English.*

Software utilizado:
- OSX El Capitan (10.11.3) 
- Xcode 7.2.1 (7C1002) 

Pasos:

1) Tener un Mac preferiblemente con la última versión de OSX (10.11.3)

2) Instalar la versión correspondiente (según versión de OSX) de las *Apple Developer Tools*. Estas tools incluyen Xcode, en este caso la versión 7.2.1. 

3) Instalar la librería: GLFW3 
- Para facilitar la instalación de packages instalar Homebrew, ejecuta en la app Terminal el siguiente comando: 
    
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/ Homebrew/install/master/install)"

- Una vez instalado Homebrew copia brew install glfw3 en Terminal para su instalación.

4)  Instalar la librería: raylib
- Descargar raylib desde GitHub (https://github.com/raysan5/raylib). El ZIP incluye todo raylib: código fuente, ejemplos, algunas tools, juegos… 
- Descomprimir el ZIP en algún directorio. En caso de usar el navegador Safari se descomprimirá automáticamente. 
- Desde la app Terminal, acceder al directorio raylib-master/src, para ello escribe cd, pon un espacio, arrastra la carpeta del Finder src hacia Terminal y dale a Enter.
- Escribir make y darle a Enter. Si ha funcionado se habrá creado un archivo en la misma carpeta que se llama libraylib.a.

5) Añadir las librerías en el proyecto de Xcode para el correcto funcionamiento de Raylib. 

- Crear un proyecto nuevo con Xcode y usar el template de Command Line Tool en OS X, darle a Next y en opciones asegurar-se que el lenguaje es C.
- Una vez abierto, en a la ventana desplegable de la izquierda dentro del navegador del proyecto, donde se encuentran las carpetas i archivos, darle click a la carpeta principal o proyecto. En la ventana central aparecera Build Phases entre otras opciones, entrar allí y desplegar Link Binary With Libraries. Aquí es donde se añaden las librerías al proyecto.
- Para añadir OpenGL y OpenAL no hay complicación porque vienen por defecto, darle al + y allí buscar OpenGL.framework y OpenAL.framework. 
- Para añadir Raylib darle al + y Add Other…, aquí se tiene que buscar el archivo libraylib.a creado anteriormente en la carpeta correspondiente. 
- Para añadir GLFW3 hacer los mismos pasos que con Raylib pero en el buscador de archivos ejecutar ⇧⌘D y pegar este directorio /usr/local/lib, ya que se encuentra en un directorio oculto por el sistema. Dentro la carpeta buscar libglfw3(versión).dylib en mi caso es la 3.3.1. 
- Ahora para que Xcode encuentre raylib.h ir a Build Settings > Search Paths y en Header y Library Search Paths doble click a su derecha para incluir el directorio arrastrando la carpeta src donde se encuentra raylib.h hacia allí.
- Add raylib/src directory not only to the Build Settings -> Search Paths -> Library Search Paths, but also to the Build Settings -> Search Paths -> Header Search Paths

6) Raylib ya debería funcionar correctamente. Para hacer la prueba, en la pagina oﬁcial de Raylib(www.raylib.com) hay varios ejemplos, por ahora coge el código de ejemplo de core > basic window, o el de a continuación. Lo pegas en el ﬁchero main.c y le das a Run o ⌘R.

Apuntes: 

- Parece haber un problema con las pantallas retina en que la ventana de la aplicación se ve reducida, la solución es mover un poco la ventana y se reescalara correctamente. 

- Los recursos de los ejemplos o de tu programa se tendrán que situar donde Xcode genere los productos.

_Tutorial desarrollado por Aleix Rafegas_