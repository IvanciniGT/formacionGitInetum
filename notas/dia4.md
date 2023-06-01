Rebase
                                                                                ENTORNO REAL

Contenedores
        CASO ANOMALOS !         rebase
      feature5------------    F1  / \                           Pipeline Jenkins    Pre - Pruebas finales
     /                    \     /    \
    master                 F5  /      \                                             Producción
     ^ fast forward           /        \
    pre           F1         /          \                                           Pre - Pruebas finales   
     ^ fast forward         /            \
    desarrollo    F1       /              \F5                                       Integración 
    v   v (^ MR)
    v   feature1
    v       Cada dia, cuando vaya a empezar a trabajar en una feature: git rebase, para ir resolviendo conflictos
    v       Cuando tengo un feature listo, lo subo a desarrollo también con un fast forward
    v
    v       En el día a día, necesitamos desplegar en pruebas. Y eso dijimos que :
    v
    v       Aquí se toca un fichero A con una función
    v   feature2
    v       Aquí se toca el mismo fichero A el la misma función
    v
    v   v
    pruebas         <- Hoy en día vuestro desarrollo                            Desarrollo (CI)
            Desde pruebas hago merge a la rama features que quiero probar

            Tengo la función modificada según feature1
                Tengo la función modificada según feature2.. Conflictos, que tengo que arreglar.
            Si el arreglo de esos conflictos, me lo llevo de vuelta a feature2

    La rama pruebas yo la iría regenerando cada cierto tiempo: La borro y la recreo desde desarrollo.


    Jenkins : Triggers:
        Control de repo -> 

        LA única forma de usar Jenkins en un entorno real es mediante scripts Jenkinsfile... 
        Los proyectos maven y estilo libre no valen ! solo para jugar y aprender jenkins.
        Los pipelines son scripts (programa) -> lo quiero bajo control de versión. Jenkins no da control de versión !
----

Librería CORE Del sistema -> desarrollo > pre > produccion
funcionA(a)
    tarea 1
    tarea 2
----

feature1
    funcionA(a,F1)
        tarea 1
        tarea 2
        tarea F1
feature2
    funcionA(a,F2)
        tarea 1
        tarea 2
        tarea F2

Queremos ir probando...
- Llegan primero los del feature 1
    En pruebas -> 
        funcionA(a,F1)
            tarea 1
            tarea 2
            tarea F1
- Llegan primero los del feature 2
    En pruebas ->  OSTION EN TO LOS MORROS !!!! conflicto
        funcionA(a,F1, F2)
            tarea 1
            tarea 2
            tarea F1
            tarea F2
- Con ese cambio (la resolución del conflicto) feature2 decide llevárselo a su rama: ERROR . MALA DECISION !
        funcionA(a,F1, F2)
            tarea 1
            tarea 2
            tarea F1
            tarea F2 -> desarrollo -> pre -> prod 

Y resulta que F1 no va a prod al final, por lo que:
1- Tengo código que no hace falta... y que además he sacado de una versión que estaba en pruebas, por loo que puede contener bugs... que ni siquiera he probado... porque yo he probado el feature2 que es lo mio...

BUENA DECISION:
Feature2, sube a desarrollo su función... que es la que ahora mismo tengo guay, probada y probada!
    funcionA(a,F2)
            tarea 1
            tarea 2
            tarea F2
Al mes... feature1 dice, yo voy también a prod.
    - Feature 1 ha seguido trabajando en su código... y ha hecho más cambios en la funcionA:
    funcionA(a,F1)
            tarea 1
            tarea 2
            tarea F1a
            tarea F1b
    Cuando hayan estado probando esto, se habrán topado con conflictos también... que habrán arreglado en pruebas.
    funcionA(a,F1, F2)
            tarea 1
            tarea 2
            tarea F1a
            tarea F1b
            tarea F2
                ESTE CONFLICTO SE HA ARREGLADO EN PRUEBAS
                pero... no me lo llevo a feature1... Lo que si puedo hacer es guardarme ese arreglo que he hecho -> Stash
                También lo puedo tener en un commit 
    Ahora, quieren subir a desarrollo, que es la UNICA FUENTE DE VERDAD, feature1.
    Y antes de hacerlo, rebase al canto... y en ese rebase, me traeré los cambios de feature2... y me da conflictos...
    Que curioso, los mismo que me dió en pruebas o no !
    Si son los mismo, ojalá y debería, con el stash, lo aplico y resuelvo conflictos. Ya lo hice... no tiro ese trabajo.!


---

# Stashes

Almacenamiento temporales de cambios
En los commits, guardamos fotos de un proyecto... en el stash cambios !

Git calcula los cambios que se han hecho en un momento dado de forma dinámica


CVS

            todo
C1 -> C2 -> C3 -> C4 -> C5 -> C6  --- MERGE (le tardaba días)
        guardo todo                 /
                                   /
                                  /
    -> Ca -> Cb -> Cc -> Cd -----/-->C3

En un Stash por contra se guardan CAMBIOS

---

Merge request

La idea básica es que tengo gente de la que no me fio y gente de la que si!

Esto es necesario en el mundo OpenSource, proyectos colaborativos es necesario

Proyecto 
    Yo y mi equipo hacemos el desarrollo y tenemos control del producto
                                              ^
                                              Control por parte de alguien que me fie!
                                              ^
    Quiero una nueva funcionalidad... y la creo


Otro caso de uso de los Merge Request es forzar un gitflow!
Eso frena ! Alguien que no es el que está subiendo código tiene que entrar y aprobar!




desarrollo
    version 1.1.1
    antes de commit :
        pom -> 1.1.0-dev
    despues del commit -> tag 1.1.0-dev


feature1
    version 1.1.1
    antes de commit :
        pom -> 1.1.0-feature1
    despues del commit -> tag 1.1.0-feature1

pre
    checkout :
        pom -> 1.1.0-RC
    despues del commit -> tag 1.1.0-RC

master
    checkout :
        pom -> 1.1.0            ----> Artifactory
    despues del commit -> tag 1.1.0




master                
     ^ fast forward         JENKINS | GITLAB CI/CD + Entorno pro
    pre Fa Fb         
     ^ fast forward         JENKINS | GITLAB CI/CD + Entorno Pre
    desarrollo   Fa Fb -Fb           
    v   v ^                 Jenkins New Feature !
    v   feature1
  J v   feature2
    v   v
    pruebas                 JENKINS -> Auto a Pruebas

    Pipeline diario a las 7am en todas las ramas feature me haga un rebase
    