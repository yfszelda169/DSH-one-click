# Instalador todo en uno de DeepSeek Harness para Windows

[中文](README.zh.md) | [English](README.en.md) | [Français](README.fr.md) | Español | [Русский](README.ru.md) | [العربية](README.ar.md)

Haz doble clic en **install-deepseek-harness.cmd** para iniciar. Es un **instalador CMD autónomo de un solo archivo** y no requiere un archivo .ps1 separado. El CMD extrae su lógica integrada al directorio temporal de Windows, la ejecuta con PowerShell integrado y elimina el archivo temporal al terminar. El instalador detecta automáticamente el idioma del sistema Windows: chino, inglés, francés, español, ruso o árabe. Puedes forzar el idioma con la variable **DSH_INSTALLER_LANG**, por ejemplo:

    set DSH_INSTALLER_LANG=es
    install-deepseek-harness.cmd

## Inicio rápido

1. Haz doble clic en **install-deepseek-harness.cmd**.
2. Espera a que terminen Node.js, Git y DeepSeek Harness.
3. El instalador inicia brevemente DeepSeek Harness para inicializar el perfil Desktop y luego lo cierra automáticamente.
4. Selecciona los complementos por número y después las habilidades. Ejemplos: 1 3 5 o 1,3,5; all selecciona todo; 0 no instala nada.
5. Cada complemento o habilidad se intenta primero desde GitHub. Si hay tiempo de espera agotado o error, se usa un espejo accesible desde China.
6. DeepSeek Harness se inicia automáticamente al terminar.
7. Introduce tu clave API de DeepSeek en DeepSeek Harness en **Settings → Models**.

Comprobación sin instalar nada:

    install-deepseek-harness.cmd -CheckOnly -NoLaunch

## Opciones

| Opción | Descripción |
|---|---|
| -CheckOnly | Solo comprueba el entorno y la red; no instala nada |
| -SkipPlugins | Omite la selección e instalación de complementos/habilidades |
| -NoLaunch | No inicia DeepSeek Harness al terminar |
| -Force | Fuerza la reinstalación de la aplicación de escritorio |

## Qué se instala

| Componente | Origen / comportamiento |
|---|---|
| Aplicación oficial DeepSeek Harness para Windows | https://download.deepseek.com/desktop/dsh-latest-windows-x64.exe |
| Node.js 24+ | Requiere >=24.9.0; primero WinGet LTS y después el ZIP portátil oficial. WinGet puede activar UAC |
| Git for Windows | Primero WinGet; un fallo no bloquea el flujo principal. WinGet puede activar UAC |
| WinGet | Se usa si está disponible; si no, Node.js usa el ZIP oficial |

Node.js es necesario para find-skills y el ecosistema del CLI DSH. Python no es necesario para la instalación base y los complementos opcionales tampoco requieren Python del sistema.

## Rutas predeterminadas

| Contenido | Ruta |
|---|---|
| DeepSeek Harness | %LOCALAPPDATA%\Programs\DeepSeek Harness |
| DSH_HOME | %USERPROFILE%\.dsh |
| Habilidades | %USERPROFILE%\.dsh\skills |
| Registro del instalador | %LOCALAPPDATA%\DeepSeekHarnessInstaller |

Si DSH_HOME está definido, el instalador lo respeta.

## Complementos opcionales (10 complementos populares, selección múltiple)

Los complementos se instalan en el perfil Desktop mediante el comando integrado **dsh plugin --profile desktop add**. El instalador muestra los 10 complementos siguientes y no los instala todos por defecto. La combinación se validó con DSH **0.2.2-rc.1** en un DSH_HOME temporal y con --dump-config.

| # | Complemento | Versión | Estrellas | Función | GitHub |
|---:|---|---:|---:|---|---|
| 1 | dshmarket | 1.73.15 | 10,3 k | Mercado visual de complementos | https://github.com/dsh-market/dsh-market |
| 2 | dsh-mnemon | 0.5.42 | 912 | Memoria por capas y gestión de contexto | https://github.com/omdsh-dev/dsh-mnemon |
| 3 | superpowers-dsh | 0.4.0 | 135 | Habilidades de TDD, depuración, planificación y colaboración | https://github.com/LayneChai/superpowers-dsh |
| 4 | @liustack/modlens | 3.27.3 | 4,7 k | Visión, OCR y reconstrucción de interfaces | https://github.com/liustack/modlens |
| 5 | @next-ai-drawio/dsh | 0.1.0 | 36,7 k | Edición de diagramas draw.io en vivo | https://github.com/DayuanJiang/next-ai-draw-io |
| 6 | @linxin666/dsh-client-ui-task-board | 0.4.16 | 10,2 k | Tablero de tareas, subtareas y ejecuciones programadas | https://github.com/zhu1090093659/dsh-web |
| 7 | dsh-whale-widget | 0.4.2 | 7,0 k | Widget de saldo y uso de DeepSeek | https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget |
| 8 | dsh-better-sidebar | 0.30.1 | 5,0 k | Barra lateral mejorada: terminal, archivos, Git y subagentes | https://github.com/omdsh-dev/DSH-better-sidebar |
| 9 | dsh-context | 0.82.2 | 4,4 k | Análisis de contexto, tokens y llamadas a herramientas | https://github.com/bowenliang123/dsh-context |
| 10 | @michengai/dsh-skills-manager | 1.2.35 | 444 | Gestión de habilidades Agent locales | https://github.com/MichengAI/dsh-skills-manager |

> Los recuentos de estrellas provienen del catálogo Awesome DSH Plugin del 2026-11-07.

Notas: ModLens necesita un modelo de visión o una configuración local existente; Next AI Draw.io descarga su servidor MCP mediante npx en el primer uso; Task Board usa el modelo configurado actualmente en DSH; el widget de la ballena usa las credenciales de DeepSeek gestionadas por DSH.

## Habilidades opcionales (selección múltiple)

| Habilidad | Origen | Compatibilidad |
|---|---|---|
| ponytail | https://github.com/DietrichGebert/ponytail | Formato SKILL.md estándar, detectado por DSH |
| find-skills | https://github.com/vercel-labs/skills | Usa npx skills; el instalador instala Node.js/npm |
| find-plugins | https://github.com/Nagi-ovo/dsh-find-plugins | Busca complementos DSH en GitHub |

Las habilidades están fijadas a commits Git concretos para evitar cambios inesperados de la rama main.

## GitHub primero, espejos chinos después

Complementos: el instalador intenta primero el commit fijado en GitHub. Si se agota el tiempo de espera de 60 segundos, falla o el perfil no cambia, instala la misma versión desde **registry.npmmirror.com**.

Habilidades: primero se intenta GitHub Raw. Si falla o se agota el tiempo, se usa el espejo CDN jsDelivr. Todas las fuentes Git están fijadas a commits y nunca usan una rama main flotante.

## Configuración de la clave API

El instalador no solicita, valida ni almacena la clave API de DeepSeek. Después de instalar, inicia DeepSeek Harness e introduce la clave en **Settings → Models**. La clave es gestionada por el mecanismo de configuración y credenciales de DSH.

## Permisos de administrador y UAC

- La aplicación DSH, los complementos, las habilidades, el PATH de usuario y los archivos bajo %USERPROFILE%\.dsh son operaciones de usuario y no requieren permisos de administrador.
- Node.js y Git se instalan primero con WinGet usando --scope user. Si el paquete WinGet no admite el ámbito de usuario, el instalador puede volver a la instalación del sistema y Windows puede mostrar UAC.
- -CheckOnly solo comprueba el entorno y la red. No instala nada ni activa UAC.
- Para una instalación estrictamente sin UAC, preinstala Node.js >=24.9.0 y Git, o adapta el instalador para usar solo versiones portátiles de Node.js y Git.
- Ejecuta el instalador como usuario normal. El script avisa si detecta una sesión de administrador, porque normalmente no se necesitan permisos elevados.

## Validación y seguridad

- El instalador oficial DSH valida su firma Authenticode y se detiene si falla.
- Se muestra el SHA256 del instalador.
- El ZIP portátil de Node.js verifica el archivo oficial SHASUMS256.txt.
- Los complementos usan primero un commit fijado de GitHub y después la misma versión desde npmmirror; las habilidades usan commits fijados con respaldo jsDelivr.
- Los complementos de terceros se ejecutan dentro del proceso DSH con tus permisos de usuario. Revisa cada repositorio antes de instalarlo.

## Notas

- Requiere Windows 10/11 de 64 bits, al menos 2 GB de espacio libre y conexión a Internet.
- El instalador no gestiona la clave API. Configúrala en **Settings → Models** tras el primer inicio.
- La aplicación oficial DSH se instala por usuario y normalmente no requiere permisos de administrador.
- La aplicación puede aparecer brevemente mientras se inicializa el perfil Desktop; el instalador la cierra automáticamente.
- Si falla un complemento, vuelve a ejecutar el instalador y selecciónalo de nuevo.
- DeepSeek Harness está en Developer Preview y las interfaces de complementos pueden cambiar. Las versiones están fijadas en el instalador.
- Antes de desinstalar DeepSeek Harness, elimina el comando dsh registrado por la aplicación. Este instalador solo añade el directorio CLI integrado al PATH de usuario.

## Referencias

- DeepSeek Harness: https://github.com/deepseek-ai/deepseek-harness
- Guía oficial de Web UI: https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md
- Reglas oficiales de habilidades: https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md
