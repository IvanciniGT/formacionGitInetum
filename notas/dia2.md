# Los 3 árboles (entornos) de git

- Working dir, mi carpeta 
- área de preparación: staging
- Repositorio local
  ---
- Repositorios remotos

# Operaciones básicas:

git init 
git add
git rm 
        --cached
git mv 
git commit -m ''

## Auditoría

git show <COMMIT>
git log --oneline --all --graph

## Ver la carpeta como estaba en el pasado... en un determinado commit

git checkout 

## Deshacer cambios no commiteados

git checkout 
git restore --source 

## Deshacer cambios commiteados, sin perder historial y dejando código en 

git revert

## Reescritura del repo (cambiar el historial del proyecto)

git commit --amend -m '' --no-edit
git reset 
            --soft   <COMMIT>   Borra commits posteriores a uno solicitado
            --mixed  <COMMIT>   + restaura el área de preparación
            --hard   <COMMIT>   + restaura el working dir... perdiendo los cambios que haya realizado en los archivos

## Repos remotos

Nos permiten compartir/publicar el código.
Podemos tener muchos. El primero por defecto se llama origin... que puedo cambiarle el nombre.
Cuando damos de alta un repo y comenzamos a sincronizar una rama con él, en nuestro repo se genera una copia de la rama X en la que vaya a trabajar que exista en el remoto. Además de la copia de la rama del remoto, tendré mi propia rama equivalente local.

                  master
                  pre
            rama: desarrollo C1 -> C2 -> C3 (3) [paso 2+3 = push] -> C4 -> C5 (7) [6+7] = push 
                | 
                REPO_A
                |
            Servidor de alojamiento de remotos (origin)
                |
    ---------------------------------------------------------------------------------
    |                                                                               |
    IvanPC                                                                      MenchuPC
    | proyecto                                                                      | proyecto
        |- repo loca:                                                                       |- repo local:
             |- desarrollo         C1 -> C2 -> C4 -> C5 (5)(merge)   En esta trabajo        |- desarrollo         C1 -> C2 -> C3 (1)
             |                                 /    \  (6)                                  |                             v  (2) 
             |- origin/desarrollo  C1 -> C2 -> C3 (4)(fetch) ->C4 -> C5 (7)                 |- origin/desarrollo  C1 -> C2 -> C3      
                ( La que uso para sincronizarme con el remoto )
                pre
                origin/pre
                master
                origin/master

    Qué es un pull? = fetch + (merge o rebase)

Remoto: master
          ^v
Local:  origin/master
          ^v
Local:  master

---

# Para qué queremos el historial del proyecto?

- Auditar... y entender realmente lo que ha ocurrido a lo largo del tiempo * merge
- Controlar lo que se ha ido incorporando a lo largo del tiempo            * rebase

---
# Operaciones de fusión de cambios en git:
- merge: 
  - fast-forward:       Copia un Commit a otra rama... con todo su historial. ESTO ES GENIAL !
  - merge completo:
- rebase:

                                      1.0.0
                              1.0.0.RC2
TAGS                  1.0.0.RC1       1.0.0.RC3
                      

Hotfix                                    C5->RP1 
                                          /     \
master                                  C5-------\----RP1--------------------------------------------> CDelivery -> Registry de Artefactos
                                       /          \  /
pre                    C3[x]--C4[x]--C5[√]---------RP1-------------------------------------------------------------> CI2 (jenkins) - Tester
                       /      /[ff]  /[ff]                    
desarrollo -C1---C2---C3-----C4-----C5---------------------C8------------C7'-----RP1---------------> CI1 (jenkins) - Desarrollo  CODIGO LISTO
                                     \                    / \            /f
feature1                             C5--->C6-->C7       /   C8---C6'---C7' rebase
 cosmin felipe                        \     \    \      /
feature2                               C5----\----\----C8
 tomas                                        \    \f   \m    
instalacion_desarrollo                         C6---C7---C9                                                                INSTALAR EN ENTORNO DESARROLLO
                                                      ^
                                                    A nivel de historial del proyecto NO ME APORTA NADA 
    De esta rama , yo paso olimpicamente de cara a entender el código... solo sirve para despliegue automatizado

Entre desarrollo y pre y entre pre y master, las fusiones siempre deberían de ser FastForward

C9, es un commit que me cuenta lo que ha pasado realmente en el proyecto, su historia real: He fusionado lo que ha hecho tomas, con lo que tenía cosmin


QUITAROS DE LA CABEZA QUE UN CONFLICTO ES ALGO MALO

Los conflictos ocurren porque 2 trabajemos el mismo código... y esto es lo que puede pasar de manera natural
Y el conflicto habrá que resolverlo.

```java
// Evolución de tomas
public void funcionAuxiliarTomas(int dato1, int dato2){
    //Tarea 1
}
public void funcionAuxiliar(int dato1){
    funcionAuxiliarTomas(dato1, null);
}
```java 

```java
// Evolución de cosmin
public void funcionAuxiliarCosminTomas(int dato1, int dato2, int dato3){
    //Tarea 1
}
public void funcionAuxiliar(int dato1, int dato2){
    funcionAuxiliarTomas(dato1, dato2, null);
}
public void funcionAuxiliar(int dato1){
    funcionAuxiliarCosmin(dato1, null, null);
}
```java 

---

desarrollo      ----C1 ---- C2 ---- C3 -- C4 ---------------C5
                                     \m /f           \    /
Ivan                C1 ---- CI ------ C4 (conflictos) \m /
Adri                C1 ---- CA ------------------------ C5
Pedro               C1 ---- CP
alex                C1 ---- CX
cosmin              C1 ---- CC
feature/galvarez    C1 ---- CG
jmontesl            C1 ---- CJ
julio               C1 ---- CJL
tomas               C1 ---- CT
