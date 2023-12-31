# GitFlow y GitHub

- Fuente: [A successful Git branching model: Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)
- Fuente: [GitFlow en Github: G. Mizael Mtz Hdzn (youtube)](https://www.youtube.com/watch?v=LkYWop93S70&t=2176s)

## ¿Qué es GitFlow?

GitFlow se define como un sistema de branching o `ramificación o modelo de manejo de ramas en Git`, en el que se usan las `ramas principales y auxiliares`. 

### Ramas principales

Son ramas fijas, con tiempo de vida infinito.

- `main`, esta rama contiene el código estable del producto. Representa la versión en producción.
- `develop`, contiene los últimos cambios desarrollados para la próxima versión del software.

Si trabajamos con plataformas en la nube como `GitHub` u otro que nos ofrecen el servicio de alojamiento y gestión de repositorios de código, veremos que cuando clonamos un proyecto tendremos como rama remota `origin/main` y su equivalente en nuestro local `main`. Lo mismo sucede cuando creamos una rama en local llamada `develop`, su equivalente en el repositorio remoto sería `origin/develop`.

Consideramos que `origin/main` **es la rama principal donde el código fuente de HEAD siempre refleja un estado listo para producción.**

Consideramos que `origen/develop` **es la rama principal donde el código fuente de HEAD siempre refleja un estado con los últimos cambios de desarrollo entregados para la próxima versión.**


### Ramas auxiliares

Además de las ramas principales de `main y develop`, nuestro modelo de desarrollo utiliza una **variedad de ramas de soporte para ayudar al desarrollo paralelo entre los miembros del equipo, facilitar el seguimiento de las funciones, prepararse para los lanzamientos de producción y ayudar a solucionar rápidamente los problemas de producción en vivo.** A diferencia de las ramas principales, estas ramas siempre tienen un tiempo de vida limitado, ya que eventualmente serán eliminadas.

Cada una de estas ramas tiene un propósito específico y **está sujeta a reglas estrictas sobre qué ramas pueden ser su ramas de origen y qué ramas deben ser sus objetivos de fusión.**

- `feature`, contendrá un desarrollo o evolutivo. Cada nueva mejora o característica que vayamos introduciendo en nuestro software tendrá una rama que contendrá su desarrollo.
- `release`, cuando **se está preparando una nueva versión para ser lanzada**, se crea una rama de "release" a partir de "develop". Aquí se realizan los últimos ajustes, pruebas y preparativos para el lanzamiento.
- `hotfix`, las ramas hotfix también están destinadas a prepararse para una nueva versión de producción, aunque no esté planificada. **Surgen de la necesidad de actuar inmediatamente ante un estado no deseado de una versión de producción en vivo.** Cuando un **error crítico en una versión de producción debe resolverse inmediatamente.**

Es importante aclarar que inicialmente `Git` tenía como nombre de rama principal `master` pero posteriormente se cambió al nombre de `main`, en ese sentido, se muestra a continuación las interacciones entre las distintas ramas de GitFlow:

![git branching model](./assets//01-git-branching-model.jpg)

## Origen y destino de las ramificaciones

| Rama          | Origen   | Destino          | Convención  | Ejemplo                       |
|---------------|----------|------------------|-------------|-------------------------------|
| **main**      |          |                  |             |                               |
| **develop**   | main     |                  |             |                               |
| **feature**   | develop  | develop          | feature/    | feature/nombre-de-feature     |
| **release**   | develop  | main y develop   | release-*   | release-1.2                   |
| **hotfix**    | main     | main y develop   | hotfix-*    | hotfix-1.2.1                  |

## Práctica: Tabla de cambios

|ID         |  DESCRIPCIÓN CORTA                            |  RAMA DE TRABAJO                | RAMA ORIGEN | RAMA DESTINO   |
|-----------|-----------------------------------------------|---------------------------------|-------------|----------------|
|Cambio #1  |  Implementar inicio de sesión con facebook    |  feature/login-con-facebook     | develop     | develop        |
|Cambio #2  |  Exportar reporte de usuarios a Google Drive  |  feature/exportar-reporte-drive | develop     | develop        |
|Cambio #3  |  Error al iniciar sesión con Linkedin (1.1.0) |  hotfix-1.1.0                   | main        | main y develop |
|Cambio #4  |  Liberar versión (1.2.0)                      |  release-1.2.0                  | develop     | main y develop |

## Inicio: Creación de repositorio

- En `GitHub` creamos un repositorio llamado [gitflow-practice](https://github.com/magadiflo/gitflow-practice.git). Por defecto, cuando creamos un repositorio se crea la rama `main`.
- Clonamos el `repositorio remoto` en nuestra pc para tenerlo como `repositorio local`.
- Como el repositorio remoto lo creamos sin el archivo `README.md`, en nuestro repositorio local creamos dicho archivo.
- Realizamos nuestro primer commit, tal solo con el archivo `README.md` siguiendo estos pasos:
  ````
  git add .
  git commit -m "Initial commit"
  ````
- Subimos nuestra rama main local a la rama main remota:
  ````
  git push -u origin main
  ````
- Ahora, en nuestro `repositorio local, creamos la rama develop` y nos posicionamos en él.
    ````bash
    git checkout -b develop
    ````
- Subimos la rama `develop` a `GitHub`.
    ````bash
    git push -u origin develop
    ````

- Hasta este punto, tenemos nuestro repositorio local y remoto de esta manera:
    ````bash
    M:\PROGRAMACION\DESARROLLO_GIT\06.gitflow_en_github\gitflow-practice (develop -> origin)
    git lg

    * 157609f (HEAD -> develop, origin/main, origin/develop, main) Initial commit
    ````
- A partir de aquí, ya **estamos listos para empezar a generar nuevas features.**

## Cambio #1: feature/login-con-facebook

### Funcionalidad
- Nos posicionamos en la rama `develop` y lo actualizamos con cambios que hayan en la `rama remota develop`.
    ````bash
    git checkout develop | git pull origin develop | revisar git graph
    ````
- Creamos la rama del feature y **nos posicionamos en él:**
    ````bash
    git checkout -b feature/login-con-facebook
    ````
- **Implementamos el feature:** creación de archivos, vistas, codificación, etc.. relacionado al feature.
  En nuestro caso solo creamos archivos representativos, donde suponemos está nuestra implementación del login.
    ````bash
    touch login-facebook.html login-facebook.css login-facebook.js
    ````
- Agregamos los archivos implementados al `staging area`:
    ````bash
    git add .
    ````
- Confirmamos los cambios con un commit, agregándolos al `git repository`:
    ````bash
    git commit -m "Se implementó el inicio de sesión con Facebook"
    ````
- Posicionados en nuestra rama `feature/login-con-facebok` lo pusheamos para que esté en `GitHub`:
    ````bash
    git push -u origin feature/login-con-facebook
    ````
### Resultado

- **Repositorio Local**

    ![02-local-feature-login-con-facebook.png](./assets/02-local-feature-login-con-facebook.png)

- **Repositorio Remoto**

    ![feature-login-con-facebook](./assets/02-feature-login-con-facebook.png)

### Pull Request

**Realizamos un pull request** para que **la rama trabajada**, que fue subida al repositorio remoto, **se integre a la rama develop del repositorio remoto.**

![03-pull-request-login-con-facebook.png](./assets/03-pull-request-login-con-facebook.png)

Hasta este momento no hemos integrado las ramas, sino que solo hicimos la solicitud `(pull request)` **para que nuestra rama feature/login-con-facebook sea integrada a la rama develop.**
Veremos además, que luego de haber realizado el pull request se creó un ícono que dice "Open" en color verde, y está a la espera de que se haga la confirmación.

![04-espera-aprobacion-login-con-facebook](./assets/04-espera-aprobacion-login-con-facebook.png)

### Aprobando Pull Request

Debemos avisar a quien tenga permiso de aprobar los Pull Request que revise nuestro pull request subido y nos lo apruebe en caso de que esté todo correcto.
Lo que haría el encargado de aprobar los pull request sería `ir a la pestaña Pull requests del repositorio remoto en GitHub`. Abrir nuestro pull request, `verificar los cambios que se han hecho (pestaña commits)`, ver los archivos modificados, en caso sea necesario, llevar esos cambios a su local, hace pruebas, etc.. 

**Una vez verificado que nuestro pull request está correcto**, iría a la pestaña Conversation y haría click en `Merge pull request`

![05-pull-request-login-con-facebook](./assets/05-pull-request-login-con-facebook.png)

Ahora, si damos click nuevamente en la pestaña `Pull requests` (como en el paso 1) veremos que **ya no tendremos ningún pull request pendiente.**

## Cambio #2: feature/exportar-reporte-drive

Como ya vimos la secuencia del Cambio #1, este nuevo cambio será algo similar:

### Funcionalidad
````bash
git checkout develop | git pull origin develop | revisar git graph
````
````bash
git checkout -b feature/exportar-reporte-drive
````
````bash
touch exportar-reporte-drive.xls
````
````bash
git add .
````
````bash
git commit -m "Soporte para exportar reportes de usuarios a Google Drive"
````
````bash
git push -u origin feature/exportar-reporte-drive
````

### Resultado
- **Repositorio Local**

    ![06-feature-2-local.png](./assets/06-feature-2-local.png)

- **Repositorio Remoto**

    ![06-feature-2-remoto.png](./assets/06-feature-2-remoto.png)

### Pull Request

Solicitaremos que nuestra rama `feature/exportar-reporte-drive sea integrado a la rama develop`.
Para eso realizamos la misma secuencia que seguimos en el `Cambio #1` en nuestro `repositorio remoto de GitHub`:

> **Repositorio > Pull requests > new pull request**
> - **base:** develop
> - **compare:** feature/exportar-reporte-drive


### Aprobando Pull Request

Aquí se haría la aprobación del pull request como en el `Cambio #1`.

---

**¡IMPORTANTE!**

> Antes de que cree una nueva rama, a partir de, por ejemplo: la rama Develop, **es importante ACTUALIZAR nuestro repositorio local con lo que se tenga en el REPOSITORIO REMOTO (git pull)**, tal como lo hicimos en el Cambio #1 y Cambio #2 al inicio de los pasos a implementar.

Hasta este punto, teniendo actualizado nuestra rama `develop` de nuestro `repositorio local` con los cambios ocurridos en el repositorio remoto, tenemos nuestras ramas de esta manera:

![07-feature-1-feature-2-local](./assets/07-feature-1-feature-2-local.png)
---


## Cambio #3: hotfix-1.1.0

A continuación veremos los pasos a implementar cuando haya algún error en producción.

### Funcionalidad
````bash
git checkout main | git pull origin main | revisar git graph
````
````bash
git checkout -b hotfix-1.1.0
````
`Aquí estaríamos aplicando la solución al error detectado en producción.` 

En este ejemplo, "mi solución del error" es solo cambiar el contenido inicial del archivo README.md, pero **¡Ojo! aquí no se agregan nuevas características ni funcionalidades**, sino **solo es para solucionar el error detectado:**
````
### README.md (contenido inicial)
Giflow en GitHub - Práctica
````
````
### README.md (contenido luego de haber "solucionado el error")
# gitflow-practice

---

Flujo de trabajo usando GitFlow con GitHub
````
Luego procedemos agregando los archivo al staging, luego commiteando y pusheando la rama local al remoto:
````bash
git add .
````
````bash
git commit -m "Se solucionó el error al iniciar sesión con Linkedin"
````
````bash
git push -u origin hotfix-1.1.0
````

### Resultado
- **Repositorio Local**

    ![08-pull-request-hotfix-local.png](./assets/08-pull-request-hotfix-local.png)

- **Repositorio Remoto**

    ![08-pull-request-hotfix-remoto](./assets/08-pull-request-hotfix-remoto.png)

### Pull Request: de hotfix a main

Haremos que nuestra rama `hotfix-1.1.0` sea integrado a la rama `main`. Para eso, seguimos la siguiente secuencia desde nuestro repositorio remoto en GitHub
similar a lo que hicimos en el `#Cambio 1`:

> **Repositorio > Pull requests > new pull request**
>   - **base:** main
>   - **compare:** hotfix-1.1.0

### Aprobando Pull Request: de hotfix a main

Aquí se haría la aprobación del pull request como en el `Cambio #1`.

**En Local**

Nos posicionamos en la rama `main` y traemos las actualizaciones:

````bash
git checkout main | git pull origin main
````
Es buena práctica etiquetar las últimas versiones que se tienen en main:
````bash
git tag -a 1.1.0 -m "Versión 1.1.0"
````
Subimos la etiqueta creada al repositorio remoto de GitHub
````bash
git push origin 1.1.0
````

### Pull Request: de hotfix a develop

Como la rama hotfix fue integrada a la rama main, ahora esta rama main contiene la solución del error corregido, por lo tanto es necesario que nuestra rama develop también cuente con esta solución para que los desarrolladores sigan agregando características adicionales con estas actualizaciones incluidas.

Haremos que nuestra rama `hotfix-1.1.0` sea integrado a la rama `develop` en nuestro repositorio remoto de GitHub:

> **Repositorio > Pull requests > new pull request**
>   - **base:** develop
>   - **compare:** hotfix-1.1.0

### Aprobando Pull Request: de hotfix a develop 

Aquí se haría la aprobación del pull request como en el `Cambio #1`.

**En Local**

Nos posicionamos en la rama `develop` y traemos las actualizaciones:

````bash
git checkout develop | git pull origin develop
````

Con los cambios realizados en el repositorio remoto y local, tenemos de esta manera nuestras ramas, donde la rama `hotfix-1.1.0` fue mergeada primero a la rama `main` y luego a la `develop`:

![09-hotfix-main-develop](./assets/09-hotfix-main-develop.png)

## Cambio #4: release-1.2.0 

A continuación veremos los pasos a implementar cuando se va a publicar la siguiente versión del software:

### Funcionalidad

````bash
git checkout develop | git pull origin develop | revisar git graph
````
````bash
git checkout -b release-1.2.0
````

Realizamos algunos ajustes, pruebas o preparativos para el lanzamiento. En mi caso solo crearé un archivo donde simularemos que tendremos algunas configuraciones requeridas:

````bash
touch ajustes-release-1.2.0.properties
````
````bash
git add .
````
````bash
git commit -m "Último ajuste"
````
````bash
git push -u origin release-1.2.0
````

### Resultado

- **Repositorio Local**

    ![10-release-local.png](./assets/10-release-local.png)

- **Repositorio Remoto**

    ![10-release-remoto](./assets/10-release-remoto.png)


### Pull Request: de release a main

Haremos que nuestra rama `release-1.2.0` sea integrado a la rama `main`. Para eso, seguimos la siguiente secuencia desde nuestro repositorio remoto en GitHub similar a lo que hicimos en el `#Cambio 1`:

> **Repositorio > Pull requests > new pull request**
>   - **base:** main
>   - **compare:** release-1.2.0

### Aprobando Pull Request: de release a main

Aquí se haría la aprobación del pull request como en el `Cambio #1`.

**En Local**

Nos posicionamos en la rama `main` y traemos las actualizaciones:

````bash
git checkout main | git pull origin main
````
Es buena práctica etiquetar las últimas versiones que se tienen en main:
````bash
git tag -a 1.2.0 -m "Versión 1.2.0"
````
Subimos la etiqueta creada al repositorio remoto de GitHub
````bash
git push origin 1.2.0
````

### Pull Request: de release a develop

Para mantener los cambios realizados en la rama `release`, debemos fusionarlos nuevamente con `develop`, entonces haremos que nuestra rama `release-1.2.0` sea integrado a la rama `develop` en nuestro repositorio remoto de GitHub:

> **Repositorio > Pull requests > new pull request**
>   - **base:** develop
>   - **compare:** release-1.2.0

### Aprobando Pull Request: de release a develop 

Aquí se haría la aprobación del pull request como en el `Cambio #1`.

**En Local**

Nos posicionamos en la rama `develop` y traemos las actualizaciones:

````bash
git checkout develop | git pull origin develop
````

Con los cambios realizados en el repositorio remoto y local, tenemos de esta manera nuestras ramas, donde la rama `release-1.2.0` fue mergeada tanto a la rama `main` como a la `develop`:

![11-release-merge-local-remoto](./assets/11-release-merge-local-remoto.png)

---

**NOTA**

> Los ejemplos fueron tomados de `GitFlow en Github: G. Mizael Mtz Hdzn`, en ese tutorial cuando se mergea el `hotfix` o `release` se hace a `main` y luego, en ambos casos, el `main` se mergea con `develop`, algo que en la documentación original `(A successful Git branching model: Vincent Driessen)` no ocurre así. 
>
> En la documentación original, lo que ocurre es que cuando hablamos de las ramas `hotfix` o `release`, estas ramas en una primera instancia se mergean con `main` y luego nuevamente el `hotfix` o `release` se mergean con `develop`.