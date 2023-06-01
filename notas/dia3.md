REPO REMOTO
        desarrollo ---- C1---C2----C3-----
        Adri             \--CA
        Cosmin            \--CC

REPO IVAN

 origin/adrian     ---- C1----CA------- *
 origin/cosmin     ---- C1----CM------- *

 origin/desarrollo ---- C1---C2----C3-----
        desarrollo ---- C1---C2----C3------CI---CJ------C456
                                    \      /     \      /
                    ivan             C3---CI     CJ---C456

REPO JULIO
 origin/desarrollo ---- C1---C2----C3- *
 origin/adrian     ---- C1----CA------- *
 origin/cosmin     ---- C1----CM------- *
        desarrollo ---- C1------------
                         \
        julio             C1---CJ-----

    Estando en la rama desarrollo:
        - Si hago un git fetch que ocurre?
            Me traigo una copia de todas las ramas que ahora mismo existan en el remoto 
        - Si hago un git pull?
             1. se hace un git fetch
             2. hace un merge de la rama en la que estoy con la rama remota copia de la que estoy: No hay cambios

    Estando en la rama julio:
        - Si hago un git fetch que ocurre?
            Me traigo una copia de todas las ramas que ahora mismo existan en el remoto 
        - Si hago un git pull?
             1. se hace un git fetch
             2. hace un merge de la rama en la que estoy con la rama remota copia de la que estoy: ERROR... ya que no hay rama origin/julio con la que mergear


proyecto

    master
     ^
    release (PRE)
     ^
    desarrollo (CODIGO GUAY)

    pruebas (CI) -> Jenkins .> Despliegue en el entorno DESARROLLO

proyecto GITLAB (personal) FORK 

    *master
    *release (PRE)
    desarrollo (CODIGO GUAY)
     v
    Ivan
    *pruebas (CI) -> Jenkins .> Despliegue en el entorno DESARROLLO




* a6da37c (HEAD -> desarrollo, ivan) Fichero ivan
* 2e3342f (origin/desarrollo) Otra Edicion Readme.md
* f86981b Edicion Readme.md
* 63e8e10 Alta del proyecto


----

master ---> Producción  Hasta la característica 16
 ^
pre    ---> Previo y hago pruebas finales
            Añado característica 17 + 16 + 15 ... + 1                  a mano? De donde saco la pasta?
                                 18             Si las pruebas están automatizadas! -> Confianza
                                 19
                                 20
 ^ 
desarrollo ---> Codigo guay Integrarlo con el resto de cosas... y ver que las COMUNICACIONES SON BUENAS (desarrollo)
     v rebase  ^ merge ff
    feature1/tarea1 
    feature2/tarea1
      v  Si trabajo con pruebas unitarias, me quito esto 
      v
      v  Merge ff
pruebas_en_desarrollo --> Entorno de desarrollo para pruebas


De desarrollo a feature1, como me actualizo?
    El desarrollo esta en una situación concreta con un  codigo vA...
        Salimos a desarrollar F1, F2, F3
            git switch -c feature1
        En un momento dado, la F1 se dice LISTA ! ---> desarrollo
            Estando en feature1: git rebase desarrollo (conflictos) ... 
                    eso si... poco a poco . Esto lo hago todos los dias 
            Estando en desarrollo: git merge --ff-only feature1 
        En paralelo sigo con feature2
            se hace sus rebase... y en momento le entra los cambios del feature1... guay (conflictos... guay... los resuelvo)
            Estando en desarrollo: git merge --ff-only feature1 

---

Pruebas de software                                                 Mocks
- En base al nivel de la prueba                                       ^
  - unitarias           Prueba una característica de un componente AISLADO !        A  ->   B   --->  C  <---   D
  - Integracion         Prueba la COMUNICACION
  - sistema             Prueba es el comportamiento del sistema 
  - aceptación          
- En base al procedimientop para su realización:
  - estáticas <- Sonarqube
  - dinámicas
    -  funcionales
    -  no funcionales
       -  ha
       -  rendimiento
       -  estres
       -  carga
       -  ux
       -  ...

Cualquier prueba debe probar UNA SOLA COSA

----

Microservicios es insufiente

Componentes desacoplados de software


----

Libreria Java que hace un tipo de análisis concreto


---
AngulaJS
  modulos
    componentes
          ^
Angular   ^
    app   ^
        modulos 


Proyectos angular  diferentes


---

version:   a.b.c
                        INCREMENTAN
a: MAJOR                Cuando hago un cambio que no respeta retro-compatibilidad
                            Quito una función
                            Refactorización heavy
                                Quitar funciones (las he renombrado)
b: MINOR                Incrementa con nueva funcionalidad
                        Si marco funcionalidad como obsoleta @Deprecated
                            + adicionalmente puede ser que vengan bugs corregidos.
c: PATCH                Solo incrementa con arreglo de bugs

Servicio A
Y lo publico !
                1.1.0
Microservicio A 1.2.1             <           Microservicio B (Pensando que ataco a la versión 1.2.0)
                                              Frontal JS      (Pensando que ataco a la versión 2.2.0)

Microservicio A 2.2.0

/api/v1/facturas
/api/v2/facturas



maven

    proyecto GORDO
        dependencias 
            MODULO 1 v1.1.0 -> jar -> Artifactory
            MODULO 2
            MODULO 3