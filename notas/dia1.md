
# Git

SCM distribuido:
Un proyecto es la suma de muchos repositorios que tienen los distintos participantes de un proyecto.

## Commit

Un commit es un backup completo del proyecto en un momento del tiempo. NO GUARDA CAMBIOS !
Cuando git necesita conocer los cambios entre 2 commits, los calcula bajo demanda.

GIT en los commits almacena ARCHIVOS, no directorios... solo archivos (con su ruta).

## Los commits se almacenan en RAMAS

Linea temporal de evolución de un proyecto...paralela a otras ramas.

## Las ramas se almacenan en repositorios

Cada persona tiene su propio repositorio, diferente al de los demás participantes en el proyecto.

Un repo de git es una carpeta: .git

## En git manejamos 3 zonas de trabajo diferentes:

- Working dir: Directorio de trabajo
- Area de staging (preparación):
  Donde vamos preparando todo lo que vamos a incluir en el próximo commit.
- Repositorio: Donde guardamos las ramas y sus commits (carpeta .git)

proyecto/
 |- .git
 |- ... < todo el resto de cosas que no sean el directorio .git es lo que se considera WORKINGDIR

Realmente, dentro de la carpeta .git se guarda el repo y el área de staging


Directorio          Area de staging     ->       Commit          Repositorio
 Archivo1    ->      Copia del Archivo1           C1        ->    En un rama R1: C1
            add                                     commit
                     rm


Una vez que se hace un commit, el área de staging queda como está... No se cambia ni se vacía... ni nada parecido

# git status

Compara lo que hay en el working dir con lo que hay en el área de staging con lo que hay en el último commit del repo.

## git add

$ git add FICHERO

$ git add .
    Copia al área de staging todo lo que tengo en la carpet de trabajo. Incluyendo archivos ocultos y todo lo que hay dentro de las subcarpetas de la carpeta en la que me encuentro posicionado

$ git add *
    Copia al área de staging todo lo que tengo en la carpet de trabajo. NO SE INCLUYEN archivos ocultos, pero si todo lo que hay dentro de las subcarpetas de la carpeta en la que me encuentro posicionado (NO SUS OCULTOS)

* git add :/
    Copia al área de staging todo lo que tengo en la carpeta de trabajo desde la raiz del proyecto (desde la carpeta donde está la subcarpeta .git) . Incluyendo archivos ocultos y todo lo que hay dentro de las subcarpetas de la carpeta raiz

        carprta1
            .git
            subcarpeta1**
                subsubcarpeta1
            subcarpeta2


---

WORKING DIR                 AREA DE STAGING         REPO
carpeta1                       carpeta1             RAMA -> COMMIT (zip de como estaba la carpeta STAGING EN EL MOMENTO DEL COMMIT)
 libreria.md(1)                    libreria.md(1)           C1 [libreria.md(1)]                         Alta del proyecto     ^
 Libreria.java(4)                  Libreria.java(3)         C2 [libreria.md(1)+Libreria.java(1)]        Operacion Suma       git diff HEAD^^ HEAD
                                                            C3 [libreria.md(1)+Libreria.java(2)]        Operación resta       v
            <    git diff    >           <  git diff --cached >
            <           git diff HEAD                         >
---

## Para que uso git?

- Control de cambios        \   Es crítico tener un historial CLARO y LIMPIO del proyecto. cherrypick revert
- Identificar diferencias   /
- Colaborar con otras personas
- Copia de seguridad de mi trabajo + hacer snapshots para volver atrás

Cada commit, debería de incluir solamente cambios relativos a una o varias funcionalidades IDENTIFICADAS
La regla habitual (buena práctica) Es que cada commit, solo incluya cambios relativos a 1 funcionalidad:
- Arreglo de un bug
- Incorporación de una nueva función / funcionalidad
- Eliminación de una funcionalidad


Cuándo hacemos un commit?
- cuando me voy a comer... o a mi casa por la noche... ESTO ESTA BIEN 
- o cuando he acabado algo


---

MASTER***

        PRE***

    DESA***                             C1D (cambios asociados a la tarea X)
                                        ^
            IVAN C1 > C2 > C3 > C4 > C5 ---> La tiro a la basura... que mañana creo otra rama Ivan (en mi repo local)
                 C1 > C2 (arreglos)
                Me voy a comer

                      Voy a empezar algo.. y no se va a ir ... por si acaso jodo y quiero volver atrás, me hago una copia
                            Me voy a dormir
                                Voy a probar
                                    Mierda, al desplegar y probar he visto alguna ruina, que he arreglado

---

# COMMIT

Guarda una copia completa del area de staging en el repo, dentro de una rama
Cada commit, parte siempre de un commit anterior
Adicionalmente se almacenen metadatos: Autor, motivo

---

# HEAD

Es una variable que identifica el COMMIT que estoy visualizando en el directorio de trabajo...
Y desde el que se crearía un nuevo commit.

        C1 -> C2 -> C3*

Con el comando git checkout podemos cambiar el HEAD de un commit a otro.
Si voy a un commit que tenga otro commit ya detrás, el proyecto se queda en estado desasociado (DETACHED)
Eso significa que desde ese estado, no se me permite generar un nuevo commit.

---

# Descartar cambio no commiteados:

$ git checkout -- FICHERO

$ git restore --source=HEAD -- FICHERO

$ git reset --hard HEAD  !!! OJO: ESTA OPERACION ES DESTRUCTIVA ... hay que usarla con mucho cuidado !

# Descartar cambio YA commiteados

HE distribuido ya esos commits? He hecho push

- Si ya lo he distribuido... 
- Si nos he distribuido

En paralelo me preguntaré: Me interesa mantener esa copia (commit) con los cambios?

Opción 1: MUY DESTRUCTIVA: Quitar un commit (ultimo commit) o unos cuantos hacia atrás
$ git reset --hard <COMMIT> al que deseo restablecer mi proyecto. 
    - Todo lo que haya detrás será eliminado del repo.
    - En paralelo me restaura 
      - al working dir 
      - y al area de staging 
      lo que tengo en el commit al que restauro


Opción 2: UN POCO DESTRUCTIVA: Quitar un commit (ultimo commit) o unos cuantos hacia atrás
$ git reset --mixed <COMMIT> al que deseo restablecer mi proyecto.  ESTE ES EL COMPORTAMIENTO POR DEFECTO DEL COMANDO RESET... de hecho es lo que hemos hecho antes (git reset) para borrar lo que tenía apuntando en el área de staging: RESETEAR EL AREA DE STAGING
    - Todo lo que haya detrás (commits) será eliminado del repo.
    - En paralelo me restaura 
      - y al area de staging 
      lo que tengo en el commit al que restauro      

git reset --mixed HEAD         =             git reset

Esto sirve para:
- Juntar un monton de commits en 1:
        RAMA Ivan: C1 -> C2 -> C3 -> C4
        Al hacer un reset mixed:
         C1 (borra C2, C3, C4) pero no toques ni una linea del codigo... que ahora la voy a commitear



Opción 3: MUY POCO DESTRUCTIVA: Quitar un commit (ultimo commit) o unos cuantos hacia atrás
$ git reset --soft <COMMIT> al que deseo restablecer mi proyecto.  ESTE ES EL COMPORTAMIENTO POR DEFECTO DEL COMANDO RESET... de hecho es lo que hemos hecho antes (git reset) para borrar lo que tenía apuntando en el área de staging: RESETEAR EL AREA DE STAGING
    - Todo lo que haya detrás (commits) será eliminado del repo.

GIT REVERT

Genera un commit de anulación de cambios. Básicamente un commit que deshace los cambios que se incorporaron en otro commit.

----

RAMAS: 

Permiten controlar evoluciones paralelas en el tiempo de un proyecto
Esto tiene un impacto enorme a la hora de distribuir y sincronizar código

                                    origin/RamaIvan  C1 -> C2 -> C3 -> C4
                                        |
                                    Repositorio Remoto
                                        |
   --------------------------------------------------------------------------------- Red de mi empresa
    |                                                                   |
    IvanPC                                                       TomasPC
     |-Proyecto1                                                      |- Proyecto1
        |- Carpetas y archivos (WORKING DIR)                          |- Carpetas y archivos (WORKING DIR)
        |- repo de git asociado al proyecto                           |- repo de git
            |- RamaIvan         C1 -> C2 -> C3 -> C5 -> C6                |- RamaIvan         C1 -> C2 -> C3 -> C4
            |                         v              /                    |- origin/RamaIvan  C1 -> C2 -> C3 -> C4
            |- origin/RamaIvan  C1 -> C2 -> C3 -> C4

Repo remoto, permite sincronizar cambios / distribuir código entre participantes
GIT permite tener tantos remotos como se quiera

git push: Copia una rama de un repo local a un repo remoto

Fusión de cambios: Hacer que los cambios que se han hecho en un rama estén disponibles en otra rama:
- merge
- rebase
origin: El nombre por defecto que se da en git al primer repo remoto de un proyecto.

git pull: git fetch + (git merge o git rebase, según esté configurado)