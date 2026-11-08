# Installateur Windows tout-en-un pour DeepSeek Harness

[中文](README.zh.md) | [English](README.en.md) | Français | [Español](README.es.md) | [Русский](README.ru.md) | [العربية](README.ar.md)

Double-cliquez sur **install-deepseek-harness.cmd** pour démarrer. Il s’agit d’un **installateur CMD autonome en un seul fichier**, sans fichier .ps1 séparé. Le CMD extrait sa logique intégrée dans le dossier temporaire Windows, l’exécute avec PowerShell intégré, puis supprime le fichier temporaire. L’installateur détecte automatiquement la langue du système Windows : chinois, anglais, français, espagnol, russe ou arabe. Vous pouvez forcer la langue avec la variable **DSH_INSTALLER_LANG**, par exemple :

    set DSH_INSTALLER_LANG=fr
    install-deepseek-harness.cmd

## Démarrage rapide

1. Double-cliquez sur **install-deepseek-harness.cmd**.
2. Attendez l’installation de Node.js, Git et DeepSeek Harness.
3. L’installateur démarre brièvement DeepSeek Harness pour initialiser le profil Desktop, puis le ferme automatiquement.
4. Sélectionnez les plugins par numéro, puis les compétences. Exemples : 1 3 5 ou 1,3,5 ; all sélectionne tout ; 0 n’installe rien.
5. Chaque plugin ou compétence est récupéré depuis GitHub. En cas de délai dépassé ou d’échec, l’installation échoue ; les miroirs chinois sont désactivés pour cette langue.
6. DeepSeek Harness démarre automatiquement à la fin.
7. Saisissez votre clé API DeepSeek dans DeepSeek Harness sous **Settings → Models**.

Vérification sans installation :

    install-deepseek-harness.cmd -CheckOnly -NoLaunch

## Options

| Option | Description |
|---|---|
| -CheckOnly | Vérifie uniquement l’environnement et le réseau ; n’installe rien |
| -SkipPlugins | Ignore la sélection et l’installation des plugins/compétences |
| -NoLaunch | Ne démarre pas DeepSeek Harness à la fin |
| -Force | Force la réinstallation de l’application de bureau |

## Contenu installé

| Composant | Source / comportement |
|---|---|
| Application de bureau officielle DeepSeek Harness | https://download.deepseek.com/desktop/dsh-latest-windows-x64.exe |
| Node.js 24+ | Nécessite >=24.9.0 ; WinGet LTS en priorité, puis archive ZIP officielle. WinGet peut déclencher un UAC |
| Git for Windows | WinGet en priorité ; un échec ne bloque pas l’installation principale. WinGet peut déclencher un UAC |
| WinGet | Utilisé s’il est disponible ; sinon Node.js utilise l’archive ZIP officielle |

Node.js est nécessaire pour find-skills et l’écosystème du CLI DSH. Python n’est pas requis pour l’installation de base ; les plugins optionnels ne nécessitent pas non plus Python système.

## Chemins par défaut

| Contenu | Chemin |
|---|---|
| DeepSeek Harness | %LOCALAPPDATA%\Programs\DeepSeek Harness |
| DSH_HOME | %USERPROFILE%\.dsh |
| Compétences | %USERPROFILE%\.dsh\skills |
| Journal d’installation | %LOCALAPPDATA%\DeepSeekHarnessInstaller |

Si DSH_HOME est défini, l’installateur le respecte.

## Plugins optionnels (10 plugins populaires, choix multiple)

Les plugins sont installés dans le profil Desktop via la commande intégrée **dsh plugin --profile desktop add**. L’installateur affiche les 10 plugins ci-dessous et ne les installe pas tous par défaut. La combinaison a été validée avec DSH **0.2.2-rc.1** dans un DSH_HOME temporaire et avec --dump-config.

| # | Plugin | Version | Étoiles | Fonction | GitHub |
|---:|---|---:|---:|---|---|
| 1 | dshmarket | 1.73.15 | 10,3 k | Place de marché visuelle des plugins | https://github.com/dsh-market/dsh-market |
| 2 | dsh-mnemon | 0.5.42 | 912 | Mémoire hiérarchique et gestion du contexte | https://github.com/omdsh-dev/dsh-mnemon |
| 3 | superpowers-dsh | 0.4.0 | 135 | Compétences TDD, débogage, planification et collaboration | https://github.com/LayneChai/superpowers-dsh |
| 4 | @liustack/modlens | 3.27.3 | 4,7 k | Vision, OCR et reconstruction d’interface | https://github.com/liustack/modlens |
| 5 | @next-ai-drawio/dsh | 0.1.0 | 36,7 k | Édition de diagrammes draw.io en direct | https://github.com/DayuanJiang/next-ai-draw-io |
| 6 | @linxin666/dsh-client-ui-task-board | 0.4.16 | 10,2 k | Tableau de tâches, sous-tâches et exécutions planifiées | https://github.com/zhu1090093659/dsh-web |
| 7 | dsh-whale-widget | 0.4.2 | 7,0 k | Widget de solde et d’utilisation DeepSeek | https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget |
| 8 | dsh-better-sidebar | 0.30.1 | 5,0 k | Barre latérale enrichie : terminal, fichiers, Git, sous-agents | https://github.com/omdsh-dev/DSH-better-sidebar |
| 9 | dsh-context | 0.82.2 | 4,4 k | Analyse du contexte, des tokens et des appels d’outils | https://github.com/bowenliang123/dsh-context |
| 10 | @michengai/dsh-skills-manager | 1.2.35 | 444 | Gestion des compétences Agent locales | https://github.com/MichengAI/dsh-skills-manager |

> Les nombres d’étoiles proviennent du catalogue Awesome DSH Plugin du 2026-11-07.

Remarques : ModLens nécessite un modèle de vision ou une configuration locale existante ; Next AI Draw.io télécharge son serveur MCP via npx au premier usage ; Task Board utilise le modèle actuellement configuré dans DSH ; le widget baleine utilise les identifiants DeepSeek gérés par DSH.

## Compétences optionnelles (choix multiple)

| Compétence | Source | Compatibilité |
|---|---|---|
| ponytail | https://github.com/DietrichGebert/ponytail | Format SKILL.md standard, détecté par DSH |
| find-skills | https://github.com/vercel-labs/skills | Utilise npx skills ; Node.js/npm est installé par l’installateur |
| find-plugins | https://github.com/Nagi-ovo/dsh-find-plugins | Recherche des plugins DSH sur GitHub |

Les compétences sont épinglées à un commit Git précis afin d’éviter les dérives de la branche main.

## GitHub uniquement pour cette langue

Les miroirs chinois ne sont activés que lorsque l’interface de l’installateur est en chinois. Cette interface française utilise uniquement GitHub et ne bascule jamais vers npmmirror ou jsDelivr.

Les plugins utilisent des commits GitHub épinglés. Les compétences utilisent GitHub Raw. Toutes les sources Git sont épinglées à des commits, jamais à une branche main flottante.

## Configuration de la clé API

L’installateur ne demande pas, ne valide pas et ne stocke pas la clé API DeepSeek. Après l’installation, lancez DeepSeek Harness et saisissez la clé sous **Settings → Models**. La clé est gérée par le mécanisme de paramètres et d’identifiants de DSH.

## Droits administrateur et UAC

- L’application de bureau DSH, les plugins, les compétences, le PATH utilisateur et les fichiers sous %USERPROFILE%\.dsh sont des opérations au niveau utilisateur et ne nécessitent pas de droits administrateur.
- Node.js et Git sont d’abord installés par WinGet avec --scope user. Si le paquet WinGet ne prend pas en charge la portée utilisateur, l’installateur peut revenir à l’installation système par défaut et Windows peut afficher un UAC.
- -CheckOnly ne fait que vérifier l’environnement et le réseau. Il n’installe rien et ne déclenche pas d’UAC.
- Pour une installation strictement sans UAC, préinstallez Node.js >=24.9.0 et Git, ou adaptez l’installateur pour utiliser uniquement des versions portables de Node.js et Git.
- Exécutez l’installateur en utilisateur normal. Le script avertit lorsqu’il détecte une session administrateur, car les droits administrateur sont généralement inutiles.

## Validation et sécurité

- L’installateur officiel DSH vérifie sa signature Authenticode et s’arrête en cas d’échec.
- Le SHA256 de l’installateur est affiché.
- La version portable de Node.js vérifie le fichier officiel SHASUMS256.txt.
- Les plugins et compétences utilisent uniquement des sources GitHub épinglées dans cette langue. Le secours par miroir chinois est activé uniquement pour l’interface chinoise.
- Les plugins tiers s’exécutent dans le processus DSH avec vos permissions utilisateur. Vérifiez chaque dépôt avant l’installation.

## Remarques

- Windows 10/11 64 bits, au moins 2 Go d’espace libre et une connexion Internet sont requis.
- L’installateur ne gère pas la clé API. Configurez-la sous **Settings → Models** après le premier lancement.
- L’application de bureau DSH est installée par utilisateur et ne nécessite généralement pas de droits administrateur.
- L’application peut apparaître brièvement pendant l’initialisation du profil Desktop ; l’installateur la ferme automatiquement.
- En cas d’échec d’un plugin, relancez l’installateur et sélectionnez-le à nouveau.
- DeepSeek Harness est en Developer Preview ; les interfaces de plugins peuvent évoluer. Les versions sont épinglées dans l’installateur.
- Avant de désinstaller DeepSeek Harness, supprimez la commande dsh enregistrée par l’application. Cet installateur ajoute seulement le dossier CLI intégré au PATH utilisateur.

## Références

- DeepSeek Harness : https://github.com/deepseek-ai/deepseek-harness
- Guide Web UI officiel : https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md
- Règles officielles des compétences : https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/skill/skill-filesystem/README.md
