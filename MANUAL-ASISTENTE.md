# Manual del asistente

**De asistentes de IA a ingeniería de agentes** · Roberto Corella y Javier Armesto
1 de octubre de 2026 · 09:00–17:00 CET · Microsoft Ibérica, sala Santiago Dexeus, Madrid

Este manual te deja listo para el taller en unos **90 minutos**. Hazlo **antes del 1 de octubre**, con calma y con conexión a Internet. El día del taller no habrá tiempo para instalar.

Si algo falla, no lo fuerces: anota el paso y el mensaje, y sigue con el resto. Lo resolvemos al empezar.

## 1 · Lo que tienes que traer

| | Qué | Cómo comprobarlo |
|---|---|---|
| ☐ | **Portátil** con Windows y permiso para instalar programas | — |
| ☐ | **Cuenta de GitHub** con **GitHub Copilot** activo (Free, Pro o de tu empresa) | En VS Code, el chat de Copilot responde y ofrece el modo **Agent** |
| ☐ | **Sandbox de Business Central 28 o posterior** de tu empresa o partner | Puedes iniciar sesión en él |
| ☐ | Un **usuario** en ese sandbox que pueda **publicar extensiones** y **editar clientes** | Publicas una app de prueba o te lo confirma tu administrador |
| ☐ | Que el sandbox **no tenga otra extensión con objetos 71200–71349** | Lo confirma tu administrador |

No uses un entorno de **producción**. Cada asistente trae su propio sandbox; no lo compartas con otro asistente, porque dos personas publicando la misma app se pisan.

## 2 · Instala los programas (≈ 30 min)

Instala, en este orden:

1. **Git** · https://git-scm.com
2. **Visual Studio Code** · https://code.visualstudio.com
3. **Node.js LTS** · https://nodejs.org
4. **APM (Agent Package Manager)** · https://microsoft.github.io/apm/
5. En VS Code, las extensiones **AL Language** y **GitHub Copilot Chat**. Inicia sesión en Copilot con tu cuenta de GitHub.
6. La extensión **ALDC 5.0.0**. Abre una terminal y ejecuta:

```powershell
code --install-extension javierarmestogonzalez.al-development-collection@5.0.0
```

**Comprueba** en una terminal nueva que todo responde:

```powershell
git --version
node --version
apm --version
code --list-extensions --show-versions
```

La última lista debe incluir `ms-dynamics-smb.al`, `github.copilot-chat` y `javierarmestogonzalez.al-development-collection@5.0.0`.

## 3 · Crea tu copia del proyecto (≈ 10 min)

1. Abre la plantilla: **https://github.com/javiarmesto/aldc-workshop-lab**
2. Pulsa **Use this template → Create a new repository**.
3. Elige tu cuenta y un nombre, por ejemplo `mi-workshop`. Puede ser **privado**. Deja **Include all branches** sin marcar.
4. En tu repositorio nuevo, pulsa **Code** y copia la URL **HTTPS**.

Trabaja siempre en **tu copia**, nunca en la plantilla.

## 4 · Descárgalo y ábrelo (≈ 10 min)

Abre **PowerShell**, pega este bloque y cambia solo la línea `$repoUrl` por la URL que copiaste:

```powershell
$repoUrl = 'https://github.com/TU-USUARIO/mi-workshop.git'
New-Item -ItemType Directory -Path C:\Workshops -Force | Out-Null
Set-Location C:\Workshops
git clone $repoUrl mi-workshop
git clone https://github.com/microsoft/BCQuality.git bcquality
git -C bcquality checkout --detach 07e324ddbc42597c479e041e06a7833740e05d0f
code .\mi-workshop\aldc-workshop-lab.code-workspace
```

Si al final VS Code pregunta si confías en los autores de la carpeta, responde que sí.

Debes ver cuatro carpetas en el explorador: **Customer Follow-up (ALDC root)**, **App**, **Test** y **BCQuality**.

## 5 · Conecta tu sandbox (≈ 15 min)

1. En **App/.vscode**, copia `launch.json.example` como `launch.json`. Haz lo mismo en **Test/.vscode**.
2. En los dos `launch.json`, sustituye `REPLACE_WITH_TENANT_ID` por tu **tenant** y `REPLACE_WITH_SANDBOX_NAME` por el **nombre de tu sandbox**.
3. Con un archivo de **App** abierto: `Ctrl+Shift+P` → **AL: Download symbols** → inicia sesión.
4. `Ctrl+Shift+P` → **AL: Publish without debugging** para publicar **App**.
5. Repite los pasos 3 y 4 con un archivo de **Test** abierto.

`launch.json` no se sube a Git: es tuyo.

## 6 · Prepara ALDC en el proyecto (≈ 15 min)

1. `Ctrl+Shift+P` → **Developer: Reload Window**.
2. `Ctrl+Shift+P` → **AL Collection: Open Project Manager**. Selecciona la carpeta raíz **Customer Follow-up (ALDC root)**, no App ni Test. Elige el perfil de **BC28** e instala el toolkit.
3. Comprueba que ha aparecido `aldc.yaml` en la raíz. Añade al final este bloque para conectar BCQuality:

```yaml
external:
  bcquality:
    mode: external-multiroot
    enabled: auto
    url: https://github.com/microsoft/BCQuality.git
    ref: main
    pinnedCommit: 07e324ddbc42597c479e041e06a7833740e05d0f
    home: ../bcquality
    entryPoint: skills/entry.md
    pilotSkills: []
```

4. Recarga la ventana y ejecuta `Ctrl+Shift+P` → **AL Collection: Run Doctor**.
5. En el chat de Copilot, modo **Agent**, comprueba que aparecen los agentes **Architect**, **AL Spec Agent**, **Conductor** y **Developer Reviewer**.

Si `aldc.yaml` ya tenía una sección `external:`, añade solo la parte de `bcquality`.

## 7 · Prueba final (≈ 10 min)

En VS Code, abre la vista **Testing**, busca **Test → 71300 → C01–C12** y ejecútalos con **Publish & Run**.

**Resultado esperado:** fallan exactamente **C02, C03, C04, C11 y C12**. Es lo correcto: son los casos que resolverás en el taller.

Para terminar, en PowerShell:

```powershell
Set-Location C:\Workshops\mi-workshop
git status
git add -A
git commit -m "preparado para la jornada"
git push
```

Antes del commit, revisa que `git status` no incluye `launch.json`, archivos `.app` ni carpetas `.alpackages`.

## Lista final

- ☐ Git, Node.js, APM, VS Code, AL Language, Copilot Chat y ALDC 5.0.0 instalados
- ☐ Mi copia creada desde la plantilla y abierta con el workspace
- ☐ BCQuality descargado en `C:\Workshops\bcquality`
- ☐ App y Test publicados en mi sandbox
- ☐ Toolkit ALDC instalado y `aldc.yaml` con BCQuality
- ☐ Tests ejecutados: fallan C02, C03, C04, C11 y C12
- ☐ Commit «preparado para la jornada» subido a mi copia

## El día del taller

- Trae el portátil **cargado** y el cargador.
- Ten a mano tus credenciales de **GitHub** y de tu **sandbox**.
- Abre VS Code con `C:\Workshops\mi-workshop\aldc-workshop-lab.code-workspace`.
- Trabajaremos en parejas y con las guías de [los ocho laboratorios](LABS.md). No hace falta leerlas antes.

## Si algo falla

| Problema | Qué hacer |
|---|---|
| GitHub muestra 404 en la plantilla | Inicia sesión en GitHub. Si sigue, avísanos: la plantilla se abre el 30 de septiembre |
| `node`, `apm` o `code` «no se reconoce» | Cierra y abre la terminal; si sigue, reinstala ese programa |
| No aparecen los agentes de ALDC | Recarga la ventana; comprueba que el toolkit se instaló en la raíz y que existe `aldc.yaml` |
| No puedo publicar en el sandbox | Revisa tenant y sandbox en `launch.json` y que tu usuario puede publicar extensiones |
| Fallan otros tests además de C02, C03, C04, C11 y C12 | Anota cuáles y el mensaje; lo revisamos al empezar |

Guía detallada, por si quieres más contexto: [preparación completa](https://github.com/javiarmesto/aldc-workshop-lab/blob/11e05d37db2953d3fd9cf1726977daf2eaf5e688/docs/preflight.md) · [ayuda](https://github.com/javiarmesto/aldc-workshop-lab/blob/11e05d37db2953d3fd9cf1726977daf2eaf5e688/docs/help.md).

Cuando nos escribas, **nunca** incluyas contraseñas, tokens ni datos de clientes.
