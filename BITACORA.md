# Bitácora del laboratorio de rescate

**Aprendiz:** Carlos Stiven Gutiérrez Ramírez  
**Ficha:** 3407799 · ADSO  
**Repositorio:** `laboratorio-rescate-calos` (`https://github.com/TheCarlosS5/laboratorio-rescate-calos`)

---

## Reto 1 — Ver el historial
- **`git log`:** Muestra el historial completo de commits en orden del más reciente al más antiguo, incluyendo el hash completo (SHA-1), autor, fecha, hora y mensaje de cada commit.
- **`git log --oneline`:** Resume cada commit en una sola línea mostrando el código hash abreviado (7 caracteres) y el título del mensaje.
- **`git log --oneline --graph --all`:** Muestra el mismo resumen en una línea, añadiendo un gráfico ASCII lateral de las ramas y punteros (`HEAD -> main`, `origin/main`, otras ramas) para ver cómo evoluciona el historial.
- **¿Qué es el código al inicio de cada commit?** Es el identificador único del commit (hash SHA-1), que representa de forma exacta ese punto guardado del proyecto y permite referenciarlo al revisar cambios o deshacer errores.

## Reto 2 — Ver cambios antes de hacer commit
- **¿Por qué el segundo `git diff` no mostró nada?** Porque `git diff` sin opciones compara el directorio de trabajo contra el área de preparación (*staging area*). Al ejecutar `git add notas.txt`, los cambios pasaron al staging area y ya no había diferencias pendientes en el working directory.
- **¿Qué muestra `git diff --staged`?** Muestra exactamente las líneas añadidas, modificadas o eliminadas que ya fueron preparadas con `git add` y están listas para entrar en el próximo commit.

## Reto 3 — Recuperar un archivo borrado por error
- **¿Qué hace `git restore`?** Restaura un archivo en el directorio de trabajo dejándolo igual a como estaba en el último commit (o en el índice), revirtiendo modificaciones no confirmadas o recuperando un archivo borrado por error.
- **¿Funcionaría si nunca hubieras hecho commit de ese archivo?** No, porque Git solo puede restaurar archivos de los que ya tiene una versión guardada en el historial (o en el staging area); un archivo nuevo sin seguimiento (*untracked*) no tiene respaldo previo en Git.

## Reto 4 — Subir un archivo que no debía (`secreto.env`) y `.gitignore`
- **¿Qué diferencia hay entre `git restore` y `git restore --staged`?**
  - `git restore <archivo>` descarta los cambios en el directorio de trabajo (vuelve el archivo al estado guardado).
  - `git restore --staged <archivo>` saca el archivo del área de preparación (*staging area*) sin borrar los cambios locales del archivo en el disco, evitando incluirlo en el commit.
- Con `.gitignore` (`*.env`), Git ignora automáticamente `secreto.env` para que nunca se suba al repositorio remoto.

## Reto 5 — Corregir el mensaje de un commit con `git commit --amend`
- Cuando se escribió un mensaje poco descriptivo (`"cambios"`) antes de hacer `git push`, `git commit --amend -m "docs: agrega línea 4 a notas"` permitió reemplazar el mensaje del último commit local sin crear un commit adicional, manteniendo el historial limpio y en formato Conventional Commits. Solo debe usarse antes de subir el commit a GitHub.

## Reto 6 — Deshacer un commit que ya se subió (`git revert`)
- **¿`git revert` borró el commit equivocado o creó uno nuevo?** Creó un **nuevo commit** (`Revert "fix: limpia notas"`) que aplica exactamente los cambios inversos al commit `c5e77f5`, restaurando las 4 líneas de `notas.txt` sin eliminar el historial previo.
- **¿Por qué es la opción segura cuando ya se hizo push?** Porque no reescribe ni borra commits que ya fueron publicados en GitHub y que otros compañeros pueden tener sincronizados; simplemente añade una corrección limpia hacia adelante.

## Reto 7 — Deshacer un commit local antes de subirlo (`git reset --soft HEAD~1`)
- **¿Qué diferencia hay entre `git reset --soft` y `git revert`?**
  - `git reset --soft HEAD~1` mueve la rama un commit atrás eliminando el último commit del historial, pero **conserva todos los archivos y cambios listos en el staging area** para corregirlos y volver a confirmar.
  - `git revert` conserva el commit anterior en el historial y genera un commit nuevo con el cambio inverso.
- **¿Cuándo se usa cada uno?** `git reset --soft` se usa **solo en commits locales que todavía no han tenido `push`**; `git revert` se usa **cuando el commit ya fue subido a un repositorio compartido (`git push`)**.

## Reto 8 — Guardar trabajo temporalmente (`git stash`)
- **¿Para qué sirve `git stash`?** Guarda temporalmente los cambios locales sin confirmar (dejando el árbol de trabajo limpio) para poder cambiar de rama y atender otra tarea urgente sin hacer un commit incompleto.
- **¿Qué hace `git stash pop`?** Recupera los cambios guardados en la pila del stash aplicándolos de nuevo sobre el directorio de trabajo actual y los elimina de la lista de stashes.

## Reto 9 — Resultado final de `git log --oneline --graph`
```text
* e77e179 (HEAD -> main, origin/main, origin/HEAD) docs: agrega línea 5 a notas
* 45345e9 (fix/urgente) feat: agrega borrador completo
* 936b32f Revert "fix: limpia notas"
* c5e77f5 fix: limpia notas
* 6d5c556 docs: agrega línea 4 a notas
* 865e589 chore: agrega .gitignore para archivos .env
* 94f32c9 docs: modifica línea 2 de notas
* 11162bf feat: agrega notas y bitácora
* 026b0ee Initial commit
```
