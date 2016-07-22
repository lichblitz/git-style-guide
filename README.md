# Guía de estilo GIT

Esta guía de estilo de GIT esta inspirada en [*Como integrar tus cambios en el kernel de Linux*](https://www.kernel.org/doc/Documentation/SubmittingPatches),
en la [página man](http://git-scm.com/doc) y varias prácticas populares en la comunidad.

Las traducciones están disponibles en los siguientes idiomas:

* [Chino (Simplificado)](https://github.com/aseaday/git-style-guide)
* [Chino (Tradicional)](https://github.com/JuanitoFatas/git-style-guide)
* [Francés](https://github.com/pierreroth64/git-style-guide)
* [Griego](https://github.com/grigoria/git-style-guide)
* [Japonés](https://github.com/objectx/git-style-guide)
* [Koreano](https://github.com/ikaruce/git-style-guide)
* [Portugues](https://github.com/guylhermetabosa/git-style-guide)
* [Thailandés](https://github.com/zondezatera/git-style-guide)
* [Ukrainiano](https://github.com/denysdovhan/git-style-guide)
* [Español](https://github.com/alexsimo/git-style-guide)

Si tienes pensado contribuir, por favor hazlo! Haz un fork del proyecto y abre una pull request.

# Índice de contenidos

1. [Ramas](#branches)
2. [Commits](#commits)
  1. [Mensajes](#messages)
3. [Merges](#merging)
4. [Miscelánea](#misc)

## Ramas

* Usa nombres *cortos* y *descriptivos*:

  ```shell
  # bien
  $ git checkout -b oauth-migration

  # mal - demasiado impreciso
  $ git checkout -b login_fix
  ```

* Los identificadores de tickets de servicios externos (ej. incidencia de GitHub) también son
  buenos candidatos para usarlos en los nombres de las ramas. Por ejemplo:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* Usa *guiónes* para separar las palabras.

* Cuando varias personas trabajan sobre la *misma* funcionalidad, puede ser
  conveniente tener una rama de la funcionalidad *personal* y una *del-equipo*.
  Usa la siguiente nomenclatura:

  ```shell
  $ git checkout -b feature-a/master # rama del equipo
  $ git checkout -b feature-a/maria  # rama personal de Maria
  $ git checkout -b feature-a/nick   # rama personal de Nick
  ```

  Los merges (fusiones) de las ramas personales se harán a la rama 
  conjunta del equipo (ver ["Merges"](#merging).
  Posteriormente la rama de la funcionalidad del equipo entero, se 
  fusionará con "master"

* Al menos que haya una razón específica, borra la rama, una vez 
  mergeada en el repositorio remoto.
  
  Consejo: Usa el siguiente comando estando en "master", para listar
  las ramas mergeadas:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* Cada commit debería ser un único *cambio lógico*. No hagas varios *cambios lógicos*
  en un sólo commit. Por ejemplo, si un _patch_ arregla un bug y optimiza el
  rendimiento de una funcionalidad, separalo en dos commits separados.

  *Consejo: Usa `git add -p` para añadir al _staging_ partes específicas
  de los ficheros modificados.*

* No separes un único *cambio lógico* en varios commits. Por ejemplo,
  la implementación de una funcionalidad y sus correspondientes tests,
  deberían estar en el mismo commit.

* Haz commits *anticipados* y a *menudo*. Los commits pequeños y autosuficientes
  son fácilmente entendibles y reversibles cuando algo ha ido mal.

* Los commits deberían ordenarse de forma *lógica*. Por ejemplo, si el *commit X*
  depende de los cambios hechos en *commit Y*, entonces el *commit Y* debería ir
  antes del *commit X*

Nota: Si trabajas solo en una rama local que *todavía no ha sido subida*, no es
inconveniente que hagas commits como capturas temporales de tu trabajo. Aún así,
siguen siendo válido aplicar las prácticas anteriores *antes* de subir la rama. 

### Mensajes

* Usa el editor, no la terminal, cuando escribas el mensaje del commit

  ```shell
  # bien
  $ git commit

  # mal
  $ git commit -m "Arreglo rápido"
  ```

  Haciendo commits desde la terminal fortalece la costumbre de tener que encajar
  todo en una sola línea, que normalmente acaba siendo poco informativa, y mensajes
  de commit ambiguos.

* La línea de resumen (ej. la primera línea del mensaje) debería ser **descriptiva**
  y **breve**. Preferiblemente con no más de **50 caracteres**. La primera letra 
  debería ser mayúscula y escrita en imperativo presente. No debería tener punto
  al final de la frase, ya que es el título del commit.

  ```shell
  # bien - tiempo imperativo presente, primera letra mayúscula, menos de 50 caracters
  Marcar registros grandes como obsoletos al limpiar los errores

  # mal
  arreglado ActiveModel::Mensajes de error de desuso cuando se usaba el AR.
  ```

* Después del título debería ir una línea en blanco seguida de una descripción
  más completa. Debería contenerse en **72 caracteres** y explicar *por que*
  el cambio es necesario, *como* arregla el fallo y que posibles *efectos secundarios*
  podría tener.

  También debería proveer enlaces a recursos relacionados (ej. link a la tarea
  correspondiente en el gestor de incidencias):

  ```text
  Título breve (50 caracteres o menos) de los cambios

  Texto descriptivo mas completo, si necesario. Encajar la 
  descripción en 72 caracteres. En algunos casos, la primera
  línea es considerada el asunto de un email y el resto del
  texto, el mensaje. El salto de línea que separa el título
  del cuerpo, es crítica (al menos que el mensaje no tenga 
  descripción); herramientas como rebase puede funcionar
  mal si se juntan estas dos líneas.

  Siguientes párrafos van después de saltos de línea.

  - Se pueden usar listas ordenadas.

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between
  - Usar guiones o asteriscos para nuevos elementos de
    la lista, seguidos de un espacio, con salto de línea
    entre cada elemento

  Fuente http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```
  
  Como último detalle, cuando escribas el mensaje del commit, piensa que 
  necesitarias saber cuando necesites leerlo después de un año.

* Si el *commit A* depende del *commit B*, la dependencia debería anotarse
  en el mensaje del *commit A*. Usa el código SHA1 cuando te refieras a 
  commits.

  De la misma forma, si el *commit A* arregla un fallo introducido por el
  *commit B*, debería anotarse también en el mensaje del *commit A*.

* Si a un commit se le va a hacer *squash* con otro commit, use los parámetros
  `--squash` y `--fixup` respectivamente, para reforzar nuestra intención:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Truco: Usa el parámetro `--autosquash` cuando hagas **rebase**. Los commits marcados
  será incluidos automáticamente en el squash)*

## Merges

* **No reescribas la historia pública.** La historia del repositorio es muy valiosa
  por si misma y debería ser capaz de indicar *que ha pasado* en determinados momentos.
  Alterar la historia del git es una fuente de problemas muy común para todo aquel
  que trabaje en el proyecto.

* Aún así, hay casos donde sobreescribir la historia es legítimo. Eso casos son:
  
  * Eres la única persona trabajando en esa rama y nadie la está revisando.
  
  * Quieres limpiar u ordenar los commits (ej. squash commit) o/y hacer *rebase*
    a master para fusionarla más tarde.

  Recapitulando, *nunca reescribas la historia de la rama "master"* o ninguna 
  otra rama especial (ej. usada para producción o servidores de CI)
  
* Manten la historia *limpia* y *simple*. *Antes de fusionar tu rama* asegurate:

    1. Asegurate de que está acorde a la guía de estilo y realiza cualquier 
      acción necesaria si no lo está (squash / reordenar commits, modificar mensajes, etc.)
    
    2. Hacer _rebase_ en la rama en la que será fusionada:
      
      ```shell
      [my-branch] $ git fetch
      [my-branch] $ git rebase origin/master
      # y hacer merge
      ```
      
      Esto acaba resultando en una rama que se puede aplicar directamente al
      final de la rama _"master"_ y produce una historia muy simple.
      
      *(Nota: Esta estrategia se adapta mejor a proyectos con ramas que tengan
      un ciclo de vida corto. De lo contrario sería mejor fusionar la rama 
      "master" ocasionalmente en vez de hacer _rebase_.)*

* Si tu rama incluye más de un commit, no la fusiones con un _fast-forward_:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Miscelánea

* Hay varios flujos de trabajo y cada uno de ellos tiene sus pros y contras.
  Que un determinado flujo de trabajo encaje en tu caso, depende de tu equipo,
  del proyecto y vuestros procedimientos de desarrollo.

  Dicho lo anterior, es importante *elegir* un flujo de trabajo y ceñirnos a él.
  
* *Sé consistente.* Esto se aplica tanto al flujo de trabajo hasta los mensajes
  de los commits, nombres de ramas y tags. Tener un estilo consistente en el
  repositorio hace fácilmente interpretable que está pasando sólamente mirando
  el log, los mensajes de los commits, etc.

* *Testea / prueba siempre antes de subir (push)*. No subas trabajos a medias.

* Usa [etiquetas anotadas](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  para hacer las releases u otros puntos importantes en la historia. Mejor usar
  [etiquetas _ligeras_](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  para uso personal, como forma de marcar un commit para futuras referencias.

* Manten tus repositorios en buena forma, realizando tareas de mantenimiento
  ocasionalmente:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)


# Licencia

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

Este trabajo está escrito bajo licencia [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Créditos

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... y [contribuidores](https://github.com/agis-/git-style-guide/graphs/contributors)!
