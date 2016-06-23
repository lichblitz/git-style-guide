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

# Índica de contenidos

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

## Merging

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "master" in order to merge it later.

  That said, *never rewrite the history of the "master" branch* or any other
  special branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

      ```shell
      [my-branch] $ git fetch
      [my-branch] $ git rebase origin/master
      # then merge
      ```

      This results in a branch that can be applied directly to the end of the
      "master" branch and results in a very simple history.

      *(Note: This strategy is better suited for projects with short-running
      branches. Otherwise it might be better to occassionally merge the
      "master" branch instead of rebasing onto it.)*

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  for marking releases or other important points in the history. Prefer
  [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  for personal use, such as to bookmark commits for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!
