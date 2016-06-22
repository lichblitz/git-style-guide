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

### Messages

* Use the editor, not the terminal, when writing a commit message:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  Committing from the terminal encourages a mindset of having to fit everything
  in a single line which usually results in non-informative, ambiguous commit
  messages.

* The summary line (ie. the first line of the message) should be
  *descriptive* yet *succinct*. Ideally, it should be no longer than
  *50 characters*. It should be capitalized and written in imperative present
  tense. It should not end with a period since it is effectively the commit
  *title*:

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* After that should come a blank line followed by a more thorough
  description. It should be wrapped to *72 characters* and explain *why*
  the change is needed, *how* it addresses the issue and what *side-effects*
  it might have.

  It should also provide any pointers to related resources (eg. link to the
  corresponding issue in a bug tracker):

  ```text
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Ultimately, when writing a commit message, think about what you would need
  to know if you run across the commit in a year from now.

* If a *commit A* depends on *commit B*, the dependency should be
  stated in the message of *commit A*. Use the SHA1 when referring to
  commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  also be stated in the message of *commit A*.

* If a commit is going to be squashed to another commit use the `--squash` and
  `--fixup` flags respectively, in order to make the intention clear:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*

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
