# Una guía estupendástica para compilar y exportar apps con Xcode

**Index**:
1. [Necesidades previas](#necesidades-previas)
2. [Niveles de permisos para cuenta](#niveles-de-permisos-para-cuenta)
3. [Identificadores de app](#dentificadores-de-app)
4. [Generación de iconos](#generación-de-iconos)
5. [Exportando y firmando nuestra app](#exportando-y-firmando-nuestra-app)
6. [Métodos de Distribución](#métodos-de-distribución)
7. [iTunes Connect y homologación](#itunes-connect-y-homologacion)

(*Antes de comenzar, tengamos en cuenta que la app en la que nos hemos basado para ilustrar este proceso se basa en Cordova/Ionic, y que por tanto alguno de estos pasos puede diferir para apps construidas sobre otros entornos*).

(*Y también que esto sólo servirá para exportar en XCode. Y por tanto, en Mac. Por lo que algún paso os supondrá ‘pasar por caja’. Y dad gracias a que aún no nos piden sentarnos también en una silla Apple iSit de 2500 euros*).


## Necesidades previas

Antes de empezar, necesitaréis disponer de una **cuenta de desarrollador de Apple**, ya sea una propia o una existente creada para el proyecto. Si no la tenéis, entrad en apple.developer.com para solicitarla. Recordad que, siendo esto Apple, posiblemente haya que ‘pasar por caja’ si no existe una cuenta ya disponible para el proyecto. Lo cual me lleva a los tipos de cuenta disponibles: 

- **Cuenta de Desarrollador a nivel Individual**: 99 dólares al año. Permite llevar a cabo la mayoría de acciones como firma de aplicaciones, creación de Permisos de Provisionamiento (lo veremos más adelante) o subida de aplicaciones a la App Store.
- **Cuenta de Desarrollador de tipo Empresa**: 299 dólares al año. Considerablemente más cara, pero da la opción de crear un ‘team’ y de incluir desde ahí a más personas en el equipo, pudiendo asignar diferentes tipos de permisos a cada uno de los integrantes.


## Niveles de permisos para cuenta
Ya que estamos en este punto, y sabiendo que posiblemente utilicéis la cuenta de Empresa, vamos a ver los niveles de permisos que podemos tener en un Equipo de Desarrollo:

- **Member**: el básico, que no permite llevar a cabo muchas acciones. Sí podremos consultar el listado de permisos de provisionamiento y descargarlos.
- **Admin**: el primer level-up jugoso. Este nivel permite crear permisos de provisionamiento nuevos, incluir en ellos **UDID** adicionales (veremos esto en seguida), solicitar permisos de Distribución… Casi todo lo que necesitemos como desarrollador.
- **Agent**: el dueño de la cuenta. No he podido operar con este nivel de cuenta así que no puedo dar mucha información, pero éste es el nivel de permisos que permite incluir a desarrolladores nuevos en el equipo y modificar sus niveles de permiso. Por tanto, si os diesen de alta en un equipo y fuerais a necesitar modificar perfiles de provisionamiento, solicitar nuevos permisos… etc, tendréis que poneros en contacto con el Agent de la cuenta para que extienda vuestro nivel de permisos.

 ### Nota adicional e IMPORTANTÍSIMA DE LA MUERTE
Cuando llegue el momento de distribuir vuestra aplicación necesitaréis estar dados de alta en dos sites de Apple: **App Store Connect**, e **iTunes Connect**. Se prevé que a partir de Marzo de 2019 la conectividad entre estos dos portales esté sincronizada, pero de momento necesitaréis solicitar a vuestro **Agent** que os dé de alta en ambos sitios (cuando la sincronización entre ambos suceda, con daros de alta en App Store Connect también tendréis acceso a iTunes Connect). 

Quedan dos puntos previos que debíamos comentar:

## Identificadores de app
#### UDIDs y Perfiles de Provisionamiento

- **UDID**: Unique Device Identidier. Es un código generado automáticamente en cada dispositivo Apple que sirve para autenticarlo. Algunos métodos de exportado de apps, como el **Ad Hoc** (me estoy repitiendo, pero lo veremos más adelante) generarán un archivo que sólo podrán instalarse aquellos dispositivos a los que se les haya incluido en un Perfil de Provisionamiento, utilizando sus UDID para incluirles.
- **Perfiles de Provisionamiento**: archivos generados desde Apple Connect o desde XCode que incluyen varios puntos de información importantes para Apple, como el tipo de permisos de distribución de los que se disponen (Basic, Developer o Enterprise), la cuenta asignada a ese permiso y los diferentes UDID que se han autenticado. Estos permisos se pueden modificar *mientras* tengamos un nivel de permiso de desarrollador **Admin o superior**.


No está mal como serie de pasos previos antes de que estemos preparados para distribuir la primera copia de nuestra app, ¿verdad? Pues queda aún un punto que, aunque no es exclusivo de iOs, debemos tener en cuenta: el **Bundle ID de nuestra app**. El bundle identifier es un código en formato string que podemos localizar en nuestro archivo `config.xml` como una etiqueta `id` o `widget id`, dependiendo del framework que utilicemos para desarrollar nuestra app, y sirve para identificar a nuestra app en plataformas de distribución oficiales (Google Play / iOs App Store y demás) y otras. Personalmente recomiendo usar **el mismo bundle id** si vamos a distribuir en ambas plataformas principales, porque tener que cambiar el ID manualmente cada vez que vayamos a cambiar de plataforma puede ser un engorro. El formato habitual de un bundle id suele ser similar a una URL, por ejemplo: `myapp.mycompany.com`, pero no hay cánones o estándares estrictos sobre qué condiciones necesitan estos bundle id’s. 

## Generación de iconos

Un punto del desarrollo de nuestra app que puede convertirse en un bloqueo más adelante es la **generación de iconos para nuestra app**. Tanto Google Play como iOs App Store tienen sus propias peculiaridades en cuanto a dimensiones y número de iconos que necesitan. La mayoría de frameworks (como Ionic) cuentan con herramientas para generar estos iconos, pero recomiendo consultar antes lo que nos vayan a requerir ambas plataformas de distribución

Con todo esto, hemos terminado de prepararnos para el proceso. Lo sé, es una lista de pasos y condiciones larga, pero creo que puede ser mucho mejor tener todo esto preparado desde el principio. De lo contrario, podríamos tener nuestra aplicación ya lista para distribuir, y ver ese proceso bloqueado al faltarnos alguna credencial o permiso necesario.


## Exportando y firmando nuestra app

Con todo esto listo, podemos exportar nuestra primera **ipa**- iPhone Application, el formato de archivo que reconocen los dispositivos Apple a la hora de instalar alguna aplicación. Tras terminar el proceso de building de nuestra aplicación para un entorno iOs (habiendo lanzado, por ejemplo, el comando `npm build ipa` en un framework repositado en NPM), en los directorios de nuestro proyecto probablemente tendremos una carpeta llamada ‘platforms’ donde veremos un subdirectorio ‘ios’. Dentro de esta carpeta se habrán generado, entre otros archivos, un archivo **xcodeworkspace** y un archivo **xcodeproj**. Hacer click en cualquiera de ellos nos abrirá XCode apuntando a nuestro proyecto.

![XCode Workspace](/images/xcodeworkspace.jpg)

Bajo estas líneas tenemos un ejemplo de lo que veremos al abrirse la interfaz. Hay una serie de valores y parámetros a los que prestar atención:

- **Tipo de dispositivo para el que exportamos**. Aquí podemos elegir si vamos a exportar para un modelo concreto, para una generación de dispositivos determinada, o si queremos exportar para todo el entorno Apple. Nuestro ejemplo deja seleccionada la opción ‘Generic iOs Device’ para conseguir esto (aunque si nuestro archivo ‘config.xml’ ha especificado un valor mínimo de iOs, será ésta la opción que prevalezca).

- **Bundle Identifier** (como hemos visto en la sección *necesidades previas*).

- **Cuenta de desarrollador escogida**. Debemos estar seguros de qué tipo de permisos de distribución tiene la cuenta que hemos escogido.

- **Perfil de Provisionamiento** (como hemos visto en la sección *Identificadores de app*). Haciendo click en el icono de info que vemos, podemos previsualizar los permisos de este perfil. Si hacemos click en el icono teniendo un iPhone conectado al Mac, podemos comprobar también cuántos dispositivos están autenticados en este perfil.

Otros detalles relevantes de la interfaz de XCode son:

- Algunos frameworks/arquitecturas (nos ocurrió con **Cordova**) pueden tardar en actualizarse para no tener conflictos cuando sale una versión nueva de XCode, y su modo por defecto de compilado puede fallar. Por suerte, XCode permite usar el sistema **Legacy** (el sistema de versiones anteriores). Podemos localizar esta opción desde `File > Preferences > Build System`.

- En la captura de pantalla anterior, bajo las opciones de **Signing** podemos ver otra opción llamada **Deployment Info**, que permite preconfigurar el tipo de dispositivos para los cuales vamos a exportar (tablets, iPhones, sólo una de estas opciones…). Si nuestra app está pensada para ser utilizada sólo desde iPhone, desplegaremos la pestaña *Devices* y dejaremos seleccionada sólo **iPhone**. No es necesario seleccionar ninguna opción en el nuevo desplegable que veremos después (Mainframe). La razón de esta selección es que, si dejamos nuestra selección de deployment a Universal, al cargar nuestra app para su revisión en la App Store la interfaz nos pedirá incluir capturas de pantalla en formato Tablet de forma obligatoria. 

Con todo preparado y configurado como lo necesitamos, hacemos esta selección:

![XCode Archiving](/images/xcodearchive.jpg)

Esta selección preparará los archivos y los comprimirá finalmente en un archivo .ipa, el formato de archivo que usan las aplicaciones en un entorno iOs. Si el proceso termina correctamente y no se dan errores, veremos la siguiente pantalla, en la que debemos seleccionar **Distribute App**. Desde aquí deberemos seleccionar el tipo de distribución adecuada, y podremos limitar nuevamente la app a una serie de dispositivos. Al acabar el proceso también podremos seleccionar el directorio donde se exportará la ipa, y renombrarla. 


## Métodos de Distribución
Hay cuatro métodos de distribución, y lo que podamos escoger dependerá de los permisos asociados a la cuenta que hayamos escogido (Developer / Enterprise, como vimos previamente):

![XCode Distribution Methods](/images/distributionmethods.jpg)

- **iOS App Store**: servirá para subir nuestra app a la App Store, para su evaluación y posterior aprobación (¡con suerte!). Hay que tener en cuenta que no podremos utilizar esta opción si nuestra cuenta no dispone de un permiso de distribución, y si no hemos registrado nuestra aplicación previamente en **iTunes Connect** (veremos esto más adelante).

- **Ad Hoc**: Tal como comentamos al hablar de los Perfiles de Provisionamiento, usaremos esta opción para distribuir nuestra app a un equipo cerrado de usuarios de testing.

- **Enterprise**: Podremos exportar una ipa sin limitación de UDID’s, pero no podremos distribuirla a ningún market. Es una buena opción para hacer testing nosotros mismos.

- **Development**: Prácticamente igual al modo Ad Hoc, con la diferencia de que el método Ad Hoc es propio de cuentas de nivel Enterprise, y con una de Development podremos usar este otro método. 


Ahora podremos volver a decidir si queremos aplicar alguna restricción de dispositivos a nuestra app. Si previamente seleccionamos ‘Generic iOS Device’, escoger aquí **None**.

![XCode Distribution Methods](/images/appthinning.jpg)

Dependiendo del tipo de distribución que hayamos seleccionado puede que necesitemos volver a confirmar opciones de permisos.

Y si este último paso nos sale bien…. ¡Ya tenemos nuestra ipa exportada!

Ahora podemos instalarla en iPhones para testearla, enviarla a cliente o a un equipo de testeo externo. Pero si creíais que la odisea de exportar con un entorno iOs acababa aquí… *Ay, ilusos*. Nos queda el plato fuerte. El acto final. La *piece de résistance*…

Nos queda revisar el proceso para poder distribuir la aplicación a través de la App Store. Por suerte, si ya hemos dado todos los pasos que indiqué en el apartado de ‘necesidades previas’, esta parte se nos hará mucho más sencilla.


## Últimos pasos 
#### iTunes Connect y homologación

En apartados previos vimos que, con una cuenta de tipo **Enterprise**, un mismo dueño de cuenta (Agent) puede asignar diferentes niveles de permisos a los desarrolladores que formen parte del equipo. Para poder enviar la aplicación a la App Store para su revisión y homologación, un usuario del equipo con rango de Agent o Admin debe registrar la aplicación en **iTunes Connect** (*[itunesconnect.apple.com](itunesconnect.apple.com)*), utilizando como primer paso el **bundle id** de la app.

Para poder llevar a cabo este paso debemos haber solicitado un **Permiso de Distribución** a través de la **plataforma de desarrolladores de Apple** (*[developer.apple.com](developer.apple.com)*), y generar un perfil de provisionamiento para dicho permiso. Opcionalmente, también podemos incluir una serie de UDID’s en este permiso. Nuestro permiso de distribución deberá llevar un bundle ID asignado, único y que no hayamos utilizado antes. Este mismo bundle ID debe ser el que utilicemos para registrar nuestra app en iTunes Connect- si ponemos el bundle id de otro perfil de provisionamiento, el proceso seguramente fallará.

Desde iTunes Connect, antes de enviar nuestra ipa, tendremos que rellenar una serie de datos referentes a nuestra aplicación como su nombre, subtítulo (de necesitarlo) y URL pública, entre otros datos. 

Con este primer registro completado, podemos tratar de enviar una compilación de la ipa a través del propio XCode. Tras haber compilado la ipa, en las opciones de Distribución seleccionaremos **iOs App Store**, y en el caso de que hayamos usado más de un perfil de provisionamiento durante el desarrollo (por ejemplo, usando uno de Development para enviar ipas en modo Enterprise/Ad Hoc), tenemos que asegurarnos en este paso de seleccionar aquí nuestro **perfil de provisionamiento de Distribución**. 

En este punto, XCode tratará de subir directamente nuestra aplicación a la App Store (también podemos elegir exportar el archivo para testearlo antes o tratar de subirlo con App Uploader. Si el proceso finaliza con éxito (y no aborta por algún problema con los permisos o el formato de las imágenes), podremos ver nuestro archivo cargado en iTunes Connect, desde donde podremos seguir incluyendo la información y material necesario (capturas de pantalla, información de copyright, información de contacto, etc).

Con todo finalizado (¡por fin!), podemos **enviar la aplicación para su revisión**, proceso que puede durar varios días (aunque personalmente he podido ver resultados en menos de 24 horas). Nuestra app puede ser rechazada en una primera instancia, pero eso no implica que no vaya a poder estar disponible para iOs- nos indicarán puntos de mejora, solicitudes de modificación de información o credenciales adicionales. Cuando hayamos corregido estos problemas podemos subir una versión nueva de nuestra app que contemple estos cambios.


Y tras una avalanca de párrafos, esta guía llega a su fin. Relajaos, abrid una cerveza y suerte en el proceso de desarrollo y homologación de vuestra app.

