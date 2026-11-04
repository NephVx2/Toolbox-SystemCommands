# Toolbox-SystemCommands-FRENCH

🇬🇧 [English version](README.md)

Un lanceur WinForms à thème sombre pour les commandes système Windows 11. Un clic exécute une commande de diagnostic ou de maintenance dans sa propre fenêtre console — plus besoin de chercher la bonne formule `DISM`/`SFC`/`netsh` ou de la retaper de mémoire à chaque fois.

> 157 commandes réparties en 9 catégories, chacune avec une description en langage clair, une confirmation demandée pour tout ce qui modifie l'état du système, une journalisation complète de l'historique, et un self-test à 47 assertions qui ne nécessite jamais les droits admin ni n'ouvre l'interface.

---

## Sommaire

- [Présentation](#présentation)
- [Structure des dossiers : EN vs FRENCH](#structure-des-dossiers--en-vs-french)
- [Captures d'écran](#captures-décran)
- [Fonctionnalités de l'interface](#fonctionnalités-de-linterface)
- [Catalogue de commandes](#catalogue-de-commandes)
- [Modèle de sécurité des commandes](#modèle-de-sécurité-des-commandes)
- [Prérequis](#prérequis)
- [Premier lancement](#premier-lancement-pas-à-pas)
- [Raccourci bureau](#raccourci-bureau)
- [Paramètres en ligne de commande](#paramètres-en-ligne-de-commande)
- [Fichiers écrits par le script](#fichiers-écrits-par-le-script)
- [Étendre le catalogue](#étendre-le-catalogue)
- [Déploiement multi-machines](#déploiement-multi-machines)
- [Dépannage](#dépannage)

---

## Présentation

`Toolbox-SystemCommands-FRENCH.ps1` est un lanceur graphique (WinForms, thème sombre) pour les commandes PowerShell/cmd du quotidien utilisées pour diagnostiquer et entretenir une machine Windows 11 — santé disque, resets réseau, réparations Windows Update, vérification des pilotes, posture de sécurité, bascules de confidentialité/télémétrie, et bien plus.

Chaque commande s'exécute dans **sa propre fenêtre console** (`cmd.exe /k`), donc vous voyez la sortie brute, non filtrée, exactement comme si vous aviez tapé la commande vous-même — la toolbox ne parse, ne capture ni ne réinterprète rien.

L'application **s'auto-élève au démarrage** : comme la plupart des commandes nécessitent de toute façon les droits administrateur, l'élévation se fait une seule fois pour toute la session, et chaque fenêtre console ouverte ensuite hérite de cette élévation — plus de prompt UAC répété par bouton.

Un seul fichier compagnon accompagne le script et doit rester dans le même dossier : **`Commands-FRENCH.psd1`**, le catalogue de commandes (157 commandes).

---

## Structure des dossiers : EN vs FRENCH

Ce dépôt fournit **deux versions indépendantes et autonomes** de la toolbox — anglaise et française — chacune dans son propre dossier, pour ne jamais mélanger le script d'une langue avec le catalogue de l'autre :

| Dossier | Contenu |
|---|---|
| **`Toolbox-SystemCommands-FRENCH/`** | `Toolbox-SystemCommands-FRENCH.ps1` + `Commands-FRENCH.psd1` — entièrement en français : libellés, descriptions, texte d'aide, et tout ce qui s'affiche dans la console. **C'est cette version-ci, celle documentee dans ce README.** |
| **`Toolbox-SystemCommands-EN/`** | Le script et le catalogue en langue anglaise — mêmes fonctionnalités, mêmes 157 commandes, entièrement en anglais |

Les deux versions sont fonctionnellement identiques (même catalogue, mêmes catégories, même nombre d'assertions au self-test) — seule la langue d'affichage diffère. Choisissez **un seul** dossier et copiez **les deux fichiers qu'il contient ensemble** ; ne mélangez jamais le `.ps1` d'un dossier avec le `.psd1` de l'autre (`Toolbox-SystemCommands-FRENCH.ps1` va avec `Commands-FRENCH.psd1`, pas avec le catalogue anglais) — les libellés et les textes se retrouveraient alors dans deux langues différentes au sein de la même interface. Les deux versions fonctionnent aussi bien sur une installation Windows en français qu'en anglais — ce script ne fait que lire/écrire directement dans le registre et WMI, donc contrairement à d'autres scripts de la suite, rien ici n'est affecté par la langue d'affichage de Windows.

Si vous n'avez besoin que d'une seule langue, vous pouvez supprimer sans risque le dossier dont vous ne vous servez pas.

---

## Captures d'écran

<p float="left">
  <img src="https://raw.githubusercontent.com/NephVx2/Toolbox-SystemCommands/main/screenshots-FRENCH/01-int%C3%A9grit%C3%A9-syst%C3%A8me.png" width="48%" />
  <img src="https://raw.githubusercontent.com/NephVx2/Toolbox-SystemCommands/main/screenshots-FRENCH/11-html-preview.png" width="48%" />
</p>

À gauche : la fenêtre principale (catégorie Intégrité système dépliée, barre favoris/recherche/export, panneau de description fixe) avec le popup d'aide du Pack Intégrité complet ouvert. À droite : le rapport HTML d'historique exporté.

D'autres captures (Disque, Réseau avec le détail de Reset pile TCP/IP, Windows Update/Performance, une vérification live de l'état de virtualisation, Pilotes/Matériel, Sécurité rapide avec les certificats Secure Boot 2023, Confidentialité/Télémétrie, et Divers) sont dans le dossier [`screenshots-FRENCH`](https://github.com/NephVx2/Toolbox-SystemCommands/tree/main/screenshots-FRENCH).

---

## Fonctionnalités de l'interface

| Fonctionnalité | Détail |
|---|---|
| **Catégories repliables** | Cliquer sur l'en-tête d'une catégorie pour la replier/déplier — utile dès que le catalogue dépasse une vingtaine de commandes |
| **Favoris** | Cliquer sur l'étoile à côté de n'importe quelle commande pour l'épingler ; persiste dans `Favoris.txt` d'une session à l'autre ; filtre "Favoris uniquement" disponible |
| **Recherche** | `Ctrl+F` place directement le curseur dans la barre de recherche ; la recherche porte sur le libellé, la description, **et** le texte d'aide de la commande (pas seulement le libellé visible du bouton) |
| **Bouton d'aide (`?`)** | Présent sur chaque commande pour garder un alignement constant, mais actif (cyan, cliquable) uniquement sur les commandes possédant un champ `Help` — ouvre une fenêtre avec une explication plus détaillée. 93 commandes sur 157 en possèdent une actuellement |
| **Clic droit → Copier la commande** | Copie la commande réelle sous-jacente dans le presse-papiers, si vous préférez la lancer ou l'inspecter vous-même |
| **Confirmations** | Les commandes marquées `Confirm = $true` dans le catalogue demandent une confirmation Oui/Non avant de se lancer (voir [Modèle de sécurité des commandes](#modèle-de-sécurité-des-commandes)) |
| **Historique** | Chaque lancement est horodaté et enregistré, avec le nom de la machine, l'utilisateur, la version de Windows, et le PID du processus lancé |
| **Export HTML** | Un bouton génère un rapport à thème sombre (identité visuelle partagée avec le reste de la suite de maintenance) — groupe par jour (le plus récent déplié, les précédents repliés), avec un filtre JavaScript en direct, des cartes de statistiques (total commandes, jours d'activité, commandes sensibles lancées, commande la plus utilisee), et les commandes sensibles surlignées en orange |
| **Export JSON** | Mêmes données d'historique en JSON exploitable par machine |

<img src="https://raw.githubusercontent.com/NephVx2/Toolbox-SystemCommands/main/screenshots-FRENCH/09-check-services-t%C3%A9l%C3%A9m%C3%A9trie.png" width="70%" />

*Le popup d'aide du bouton `?`, ouvert sur "Verification services telemetrie" — sa sortie console (derrière) montre les 5 services qu'elle vient d'interroger, et le popup détaillé ce que fait chacun d'eux.*

---

## Catalogue de commandes

**157 commandes** réparties en **9 catégories**. La description complète en langage clair et (pour beaucoup) un texte d'aide étendu sont affichés directement dans l'application — ce tableau est une carte, pas un substitut.

| Catégorie | Nombre | Exemples |
|---|---|---|
| **Intégrité système** | 12 | SFC /scannow, DISM CheckHealth/ScanHealth/RestoreHealth, pack combiné CHKDSK+DISM+SFC, points de restauration |
| **Disque** | 11 | CHKDSK (variantes lecture seule et réparation), état SMART, vérification TRIM, latence disque en temps réel, historique des erreurs disque |
| **Réseau** | 18 | Flush DNS, reset Winsock/TCP-IP, renouvellement de bail IP, test NextDNS, réseaux/vitesse Wifi, IP publique, diagnostic de connectivité multi-niveaux, contenu du fichier hosts |
| **Windows Update** | 9 | Reset des composants, scan forcé, reset Microsoft Store, historique des mises à jour, taille du cache SoftwareDistribution, vérification de redémarrage en attente |
| **Performance** | 16 | Applications au démarrage (y compris via tâches planifiées), détail du temps de démarrage, score WinSAT, bascules HVCI/VBS, top processus RAM/CPU, service de recherche Windows |
| **Pilotes / Matériel** | 12 | Périphériques en erreur, état GPU NVIDIA, info BIOS/carte mère, RAM installée, pilotes récemment installés, périphériques désactivés |
| **Sécurité rapide** | 34 | État Defender, vérification SMBv1, vérification pare-feu, Secure Boot/TPM, BitLocker, tentatives de connexion échouées, règles ASR, UAC, Credential Guard, accès contrôle aux dossiers, tunnels IPv6 automatiques (Teredo / 6to4 / ISATAP), résolution de noms héritée (LLMNR / NetBIOS / mDNS), Defender en profondeur (exclusions, protection contre la falsification) et historique des menaces, sécurité PowerShell (v2, journalisation), services d'accès distant, identifiants stockés et ouverture automatique de session — la plupart avec une vérification en lecture seule et, quand une correction sûre existe, une commande de désactivation |
| **Confidentialité / Télémétrie** | 28 | Niveau de télémétrie, accès caméra/micro/localisation, ID de publicité, Windows Recall, recherche web et suggestions sponsorisées du menu Démarrer, Copilot, Delivery Optimization, WER, synchronisation OneDrive — la plupart en paires "vérifier l'état" / "désactiver" |
| **Divers** | 17 | Rapport batterie, diagnostic énergie, rapport DirectX, état d'activation Windows, historique des arrêts inattendus, synchronisation de l'heure, imprimantes installées |

---

## Modèle de sécurité des commandes

- **Chaque commande a `Confirm = $true` ou `$false`** dans le catalogue — il n'existe pas d'état "indécis" ; le self-test vérifie que ce champ est toujours présent.
- **31 commandes nécessitent actuellement une confirmation** — tout ce qui modifie l'état du système, nécessite un redémarrage, ou pourrait perturber une session en cours (ex : `Reset Winsock`, `Reset pile TCP/IP`, `CHKDSK C: /f /r (au reboot)`, `Redemarrer le PC`, `Eteindre le PC`, `Desactiver les tunnels IPv6 automatiques`, `Desactiver PowerShell 2.0`, `Desactiver l'ouverture auto de session (Winlogon)`, la plupart des bascules de désactivation dans Confidentialité/Télémétrie).
- Une assertion dédiée du self-test verrouille le fait que six commandes spécifiques connues comme sensibles (reset Winsock, reset pile TCP/IP, reset composants WU, CHKDSK planifié, redémarrage, extinction) portent **toujours** `Confirm = $true` — une régression à cet endroit ferait immédiatement échouer `-SelfTest` plutôt que de se manifester plus tard comme une action destructrice silencieuse et non confirmée.
- **Aucune commande n'utilise `wmic`** (retiré depuis Windows 11 24H2) — vérifié par une assertion de régression dédiée du self-test.
- Le champ `Cmd` du catalogue est **toujours en guillemets simples** dans `Commands-FRENCH.psd1`, ce qui empêche structurellement toute interpolation de variable PowerShell (`$_`, `$err`, etc.) de se glisser dans une commande — la cause racine de deux bugs précoces (v1.4.3, v1.4.4) que cette conception élimine entièrement plutôt que de corriger au cas par cas.
- Quelques commandes trop complexes pour survivre à la reconstruction `-Command` de `cmd.exe` sont stockées en Base64 (`-EncodedCommand`, UTF-16LE) à la place. Deux assertions du self-test décodent chacune d'entre elles et vérifient que le script obtenu est syntaxiquement valide (via le vrai parseur AST PowerShell) — pour qu'un blob Base64 corrompu ou tronqué soit détecté par `-SelfTest` plutôt que de se manifester par une fenêtre console vide et silencieuse au clic.

---

## Prérequis

- Windows 10 ou 11.
- PowerShell 5.1 (intégré à Windows) — le script cible `#Requires -Version 5.1`, donc PowerShell 7+ fonctionne aussi.
- Droits administrateur. L'application s'auto-élève au démarrage (fenêtre UAC) — sauf `-SelfTest`, qui fonctionne sans élévation et n'ouvre jamais l'interface.
- `Commands-FRENCH.psd1` **doit** être présent dans le même dossier que le script — l'application affiche une erreur claire et se replie sur un catalogue vide plutôt que de planter si le fichier est absent ou invalide.
- Si le script est signé numériquement (recommande en environnement `-ExecutionPolicy AllSigned`/`RemoteSigned`) : le certificat de signature doit être approuvé sur la machine cible.

---

## Premier lancement (pas à pas)

1. Copier **à la fois** `Toolbox-SystemCommands-FRENCH.ps1` **et** `Commands-FRENCH.psd1` — issus du même dossier de langue, voir [Structure des dossiers](#structure-des-dossiers--en-vs-french) — sur la machine cible, dans le même dossier (ex : `C:\Scripts\Toolbox`). Le script ne trouvera pas ses commandes si `Commands-FRENCH.psd1` est laissé de côté.

2. Ouvrir un terminal PowerShell, puis se placer dans le dossier qui contient les deux fichiers (adapter le chemin ; garder les guillemets s'il contient des espaces) :

   ```powershell
   cd "$HOME\Downloads"
   ```

3. **Débloquer les deux fichiers** s'ils ont été téléchargés depuis Internet. Windows marque les fichiers téléchargés, et la politique d'exécution de PowerShell (`RemoteSigned`, par exemple) refuse de lancer un script marqué :

   ```powershell
   Unblock-File .\Toolbox-SystemCommands-FRENCH.ps1
   Unblock-File .\Commands-FRENCH.psd1
   ```

   Si PowerShell indique plutôt que l'exécution de scripts est désactivée sur ce système (la politique par défaut de Windows est `Restricted`), autoriser d'abord les scripts pour le compte courant (la modification ne s'applique qu'à ce compte, pas à toute la machine) :

   ```powershell
   Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
   ```

   Toujours bloque ? Voir le [guide pas à pas](https://github.com/NephVx2/Script-blocked-Look-at-this/blob/main/README_POWERSHELL_FRENCH.md).

4. Valider le catalogue et les fonctions internes **sans ouvrir l'interface et sans droits admin** :

   ```powershell
   .\Toolbox-SystemCommands-FRENCH.ps1 -SelfTest
   ```

   Exécute 47 assertions : structure du catalogue (chaque commande a un libellé/une commande/une description/un flag confirm, aucun libellé duplique), régressions connues (aucun pipe non échappé, aucun `wmic`, les blocs `-EncodedCommand` décodent et se parsent tous correctement), les six commandes sensibles nommées portent toutes `Confirm = $true`, toutes les fonctions requises sont définies, les chemins de journalisation/favoris sont valides, et la vérification d'élévation elle-même fonctionne. Sort avec le code `0` si tout passe, `1` sinon.

5. Lancer la toolbox normalement (accepter le prompt UAC) :

   ```powershell
   .\Toolbox-SystemCommands-FRENCH.ps1
   ```

6. Parcourir par catégorie, ou appuyer sur `Ctrl+F` et rechercher ce dont vous avez besoin (ex : "DNS", "BitLocker", "télémétrie").

7. Cliquer sur une commande. Si elle est marquée sensible, confirmer le prompt Oui/Non. Une fenêtre console s'ouvre et l'exécute — lisez sa sortie directement la.

8. Marquer d'une étoile (⭐) les commandes que vous utilisez souvent — elles seront ensuite rapidement filtrables sans avoir à fouiller dans les catégories.

9. Pour consulter ce qui a été lancé, utiliser le bouton d'export HTML ou JSON dans la barre supérieure.

---

## Raccourci bureau

Lancer la toolbox par clic droit sur le fichier `.ps1` puis "Exécuter avec PowerShell" fonctionne, mais fait clignoter brièvement une fenêtre console et la laisse ouverte derrière l'interface graphique. Un raccourci bureau évite les deux problèmes et offre un lancement par simple double-clic — utile si vous comptez utiliser la toolbox régulièrement.

1. Clic droit sur le Bureau → **Nouveau → Raccourci**.

2. Dans l'emplacement, saisir (en adaptant le chemin du script à l'endroit où vous l'avez placé) — choisir la variante qui correspond à ce qui est installé sur la machine :

   **Windows PowerShell 5.1** (intégré à toute installation Windows, toujours disponible) :

   ```
   powershell.exe -NoProfile -ExecutionPolicy Bypass -WindowStyle Hidden -File "C:\Scripts\Toolbox\Toolbox-SystemCommands-FRENCH.ps1"
   ```

   **PowerShell 7+** (uniquement si installé séparément) :

   ```
   pwsh.exe -NoProfile -ExecutionPolicy Bypass -WindowStyle Hidden -File "C:\Scripts\Toolbox\Toolbox-SystemCommands-FRENCH.ps1"
   ```

   | Paramètre | Pourquoi |
   |---|---|
   | `-NoProfile` | Ignore le chargement de votre profil PowerShell, donc la toolbox démarre plus vite et n'est pas affectée par un contenu personnalisé dans ce profil |
   | `-ExecutionPolicy Bypass` | S'applique uniquement à ce processus — permet au script de s'exécuter même si la politique d'exécution par défaut du système le bloquerait sinon, sans modifier cette politique à l'échelle de la machine |
   | `-WindowStyle Hidden` | Supprime la fenêtre console du processus de lancement, pour que seule l'interface propre de la toolbox apparaisse |

   Les deux variantes se comportent en pratique de façon identique : la toolbox s'auto-élève au démarrage via `Start-Process powershell.exe -Verb RunAs` (voir le bloc d'auto-élévation du script), qui relance toujours sous Windows PowerShell 5.1 une fois le prompt UAC accepte — que le raccourci ait démarré avec `powershell.exe` ou `pwsh.exe`. Le choix entre les deux n'affecte que ce tout premier instant, bref et non élevé, avant que l'UAC ne se déclenche ; choisissez celui que vous avez effectivement installé.

3. Nommer le raccourci (ex : "Toolbox Commandes Système"), puis terminer.

4. *(Optionnel)* Clic droit sur le nouveau raccourci → **Propriétés** → **Changer d'icône...** pour choisir quelque chose de plus reconnaissable que l'icône PowerShell par défaut.

5. *(Optionnel)* Sur le même onglet **Propriétés**, régler **Exécuter** sur **Réduite** en sécurité supplémentaire — avec `-WindowStyle Hidden` déjà présent dans la commande, c'est normalement inutile, mais c'est une option sans risque sur les systèmes où un outil de sécurité intercepte et neutralise `-WindowStyle`.

L'invite d'élévation UAC continue de s'afficher au lancement — `-WindowStyle Hidden` masque uniquement la fenêtre console, elle ne supprime pas (et ne doit pas supprimer) la confirmation administrateur.

---

## Paramètres en ligne de commande

| Paramètre | Description |
|---|---|
| `-SelfTest` | Exécute la batterie de tests internes à 47 assertions (intégrité du catalogue, fonctions requises, régressions connues) puis quitte. Aucun droit admin requis, l'interface ne s'ouvre jamais. Code de sortie `0` = tout est passé, `1` = au moins un échec. |

Il n'existe pas de paramètre CLI pour lancer une commande spécifique directement — la toolbox est conçue pour la navigation et le clic, pas pour l'invocation scriptee.

---

## Fichiers écrits par le script

| Fichier / dossier | Contenu |
|---|---|
| `%USERPROFILE%\Desktop\Rapports_Maintenance\ToolboxCommandes\Historique.log` | Journal texte brut, ajout uniquement, de chaque commande lancée — horodatage, nom de machine, utilisateur, version de Windows, et PID du processus lancé |
| `%USERPROFILE%\Desktop\Rapports_Maintenance\ToolboxCommandes\Favoris.txt` | Un libellé de commande favorite par ligne, recharge au lancement suivant |
| `Historique.html` *(à la demande, bouton d'export)* | Rapport visuel à thème sombre, groupe par jour et repliable, avec recherche et cartes de statistiques |
| `Historique.json` *(à la demande, bouton d'export)* | Mêmes données d'historique en JSON exploitable par machine |

La toolbox ne redirige ni ne capture jamais la sortie des commandes qu'elle lance — chacune s'exécute dans sa propre fenêtre `cmd.exe` interactive, donc le journal enregistre *ce qui* a été lancé, pas sa sortie. Voir [Dépannage](#dépannage) pour comprendre pourquoi c'est un choix délibéré, pas un oubli.

---

## Étendre le catalogue

Ajouter une commande signifie éditer directement `Commands-FRENCH.psd1` — il n'y a volontairement pas d'éditeur intégré à l'application, pour garder le catalogue en texte brut, facile à comparer, versionner et synchroniser entre machines.

```powershell
@{ Label   = "Ma nouvelle commande"
   Cmd     = 'ipconfig /all'
   Desc    = "Affiche dans le panneau de description fixe quand la commande est selectionnee."
   Confirm = $false
   # Help  = "Optionnel — n'ajouter cette cle que si vous voulez activer le bouton '?'."
}
```

**Règles, imposées par `-SelfTest`:**

- `Cmd` doit **toujours** utiliser des guillemets simples, jamais des guillemets doubles — c'est ce qui rend l'interpolation de variable (`$_`, `$err`...) structurellement impossible, pas seulement évitée par convention. Si le texte lui-même a besoin d'un guillemet simple littéral, le doubler (`''`).
- `Desc` et `Help` utilisent des guillemets doubles par convention — tout `$` littéral qu'ils contiennent doit être échappé par un backtick (`` `$ ``), sinon le mode "restricted language" d'`Import-PowerShellDataFile` refusera de charger **tout** le catalogue (un seul `$` non echappe dans le champ `Help` d'une commande a déjà cassé toute la toolbox de cette manière — voir l'historique de version du script pour le détail de l'incident).
- Chaque commande a besoin de `Label`, `Cmd`, `Desc`, et `Confirm` — aucun des quatre ne peut être omis.
- Lancer `-SelfTest` après chaque modification, avant de se fier au changement.

---

## Déploiement multi-machines

1. **Distribuer les deux fichiers ensemble** : `Toolbox-SystemCommands-FRENCH.ps1` et `Commands-FRENCH.psd1`, dans le même dossier — issus du même dossier de langue (voir [Structure des dossiers](#structure-des-dossiers--en-vs-french)).

2. **Approuver le certificat de signature** si une politique d'exécution stricte est en place (`-ExecutionPolicy AllSigned`/`RemoteSigned`).

3. **Exécuter `-SelfTest` en premier** sur chaque machine — aucun droit admin nécessaire, l'interface ne s'ouvre jamais, sans risque à lancer avant de décider de la suite. Un code de sortie `1` signifie que quelque chose dans le catalogue nécessite votre attention avant utilisation.

4. Comme la toolbox est un **lanceur graphique pense pour un usage interactif, en présence d'un utilisateur**, elle n'est pas conçue pour être déclenchée de manière non supervisée depuis une tâche planifiée comme le serait un script de nettoyage — il n'existe pas de paramètre CLI pour lancer une commande spécifique de manière non interactive. Déployez-la comme un outil que les gens ouvrent et parcourent eux-mêmes, pas comme une tâche de fond.

5. Garder `Commands-FRENCH.psd1` identique sur toutes les machines — c'est son but.

---

## Dépannage

<details>
<summary><strong>La fenêtre console d'une commande est vide ou se ferme instantanement</strong></summary>

C'était historiquement le symptôme d'un problème de décodage ou de syntaxe `-EncodedCommand` (voir l'incident "AutoPlay/AutoRun" dans l'historique de version). Lancer `-SelfTest` — les deux assertions dédiées `-EncodedCommand` décodent et vérifient la syntaxe de chaque commande encodée du catalogue, et détecteraient ce problème avant même que vous ne cliquiez sur le bouton.
</details>

<details>
<summary><strong>Le chargement de Commands-FRENCH.psd1 échoue, ou tout le catalogue est vide</strong></summary>

Presque toujours un `$` non échappé dans un champ `Desc` ou `Help` (qui utilisent des guillemets doubles, donc PowerShell tente de l'interpoler comme une variable — le mode "restricted language" d'`Import-PowerShellDataFile` refuse alors de charger le fichier entier). Chercher dans le fichier modifie en dernier un `$` littéral non précédé d'un backtick. `-SelfTest` inclut une vérification de régression dédiée qui scanne chaque ligne `Desc`/`Help` à la recherche exactement de ce motif.
</details>

<details>
<summary><strong>-SelfTest signale un FAIL</strong></summary>

Lire le libellé de l'assertion — il pointe directement vers le problème : un champ manquant sur une commande spécifique, un libellé duplique, un `$` non échappé, un appel `wmic` réapparu, ou une commande sensible ayant perdu son `Confirm = $true`. Corriger l'entrée du catalogue, puis relancer `-SelfTest`.
</details>

<details>
<summary><strong>Le rapport d'historique ne montre pas ce qu'une commande a réellement affiché</strong></summary>

C'est voulu — chaque commande se lance dans sa propre fenêtre `cmd.exe` interactive (`Start-Process`, sans redirection de sortie), donc `Historique.log` enregistre la commande qui a été lancée, pas sa sortie. Capturer la sortie réelle impliquerait de rediriger chacune des 157 commandes vers un fichier journal, ce qui changerait aussi le comportement de plusieurs d'entre elles (celles volontairement laissées interactives/ouvertes, comme les moniteurs réseau en temps réel) par rapport à leur usage prévu — un changement architectural plus large, pas la conception actuelle.
</details>

---

<sub>Toolbox-SystemCommands-FRENCH — lanceur WinForms, confirmation par commande sur tout ce qui modifie l'état système, journalisation complète de l'historique, self-test à 47 assertions.</sub>
