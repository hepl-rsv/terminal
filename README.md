# hepl-rsv/terminal

> Introduction course to the command line.

* * *

**Note:** the school where the course is given, the [HEPL](http://www.provincedeliege.be/hauteecole) from Liège, Belgium, is a french-speaking school. From this point, the instruction will be in french. Sorry.

* * *

**Note 2 :** certains puristes tombés ici par hasard pourraient avoir à redire sur l'approche et la méthode. Sachez que ce cours est destiné à des étudiants qui n'ont rien d'informaticiens, et qui n'auront de la ligne de commande qu'un approche d'utilisateur régulier _mais_ non-expert.  
Les plus intéressés apprendront les détails par eux-même, et le contenu de ce cours est suffisant pour couvrir les besoins pour le reste des cours.  
Si malgré tout, quelque chose vous chiffonne, n'hésitez pas à créer une issue qu'on puisse en discuter. Un grand merci.

* * *

## Réseaux & Serveurs : Introduction à la ligne de commande

### Introduction

Quand j'ai débuté dans le web, il était impensable d'avoir un jour besoin d'utiliser un terminal ; taper des lignes de commandes, c'était réservé aux informaticiens et administrateurs serveurs, _Dieu nous en préserve…_

Mais d'une part, ce cours se nomme **Réseaux & Serveurs** (ça tombe bien), et d'autre part, avec tous les _frameworks_ et outils actuels, il n'est pas rare de commencer un projet avec une petite ligne de commande de mise en place, ce que vous avez peut-être déjà dû faire (ou ferez dans un futur proche).  
Le présent cours a donc pour but de vous donner toutes les bases nécéssaires pour vous y retrouver, et ne pas vous limiter à _copier/coller_ des commandes toutes faites sans les comprendre (ce qui est très souvent assez dangereux avec ce genre d'outils).

> **Note :** toutes les notes qui suivent sont données à titre de référence, mais, comme d'habitude, tout sera détaillé oralement pendant le cours.  
> Si vous souhaitez une référence pratique à consulter au quotidien, je vous conseille le [**Mémento Unix/Linux**, aux éditions Eyrolles](http://www.eyrolles.com/Informatique/Livre/memento-unix-linux-9782212133066), et si vous voulez creuser la question, je vous recommande l'excellent [**Parlez-vous Shell ?**, aux éditions Ellipses](http://www.editions-ellipses.fr/product_info.php?products_id=8981).

### Démarrage

Pour les besoins du cours, nous allons travailler dans deux contextes : directement sur le serveur de développement de l'école, _via_ **ssh** (dont nous avons parlé aux cours prédédents) ; et localement, sur vos propres machines.  
Dans ce cas, il vous faudra ce qu'on appelle un **terminal** (ou plutôt un _émulateur de terminal_, pour les puristes).  
Ceux d'entre vous qui sont sur Mac ont déjà un programme installé, nommé **Terminal** (dans _Applications_ > _Utilitaires_) ; tandis que sur Windows, il vous faudra installer git bash, livré [avec l'installeur de git](https://git-for-windows.github.io).  
Dans les deux cas, vous trouverez plus d'informations à ce sujet sur la [toolbox](https://github.com/hepl-web/toolbox/blob/master/app/divers.md#terminal).

### Vocabulaire de base

#### Console / Interface en ligne de commande

Lorsque vous utilisez un ordinateur, vous utilisez ce qu'on appelle une interface graphique : vous demandez à votre ordinateur d'effectuer des tâches *via* des fenêtres, des boutons et des champs de texte.  
À l'opposé de l'interface graphique se trouve l'interface en ligne de commande (_command line interface_ en anglais), ou console.  
Ici, pas d'interface graphique, tout se fait en **texte** : vous tapez des **commandes**, et l'ordinateur affiche le résultat de leur exécution.

#### Terminal

Historiquement, un terminal était un ordinateur accédant à un réseau. Par extension, le terme en est venu à désigner le programme permettant d'accéder à la console (plus d'informations à ce sujet sur l'[article dédié de la wikipédia](https://fr.wikipedia.org/wiki/Terminal_informatique)).

#### Émulateur de terminal

Un émulateur de terminal est un logiciel qui "simule" une console, dans une fenêtre graphique. En règle générale, quand on utilise la console depuis une interface graphique comme macOS ou Windows, on le fait _via_ un émulateur de terminal.  
**Note:** sous macOS et certaines versions de linux, l'émulateur de terminal s'appelle "Terminal", histoire d'embrouiller encore un peu plus le monde. 😓

#### Shell

En simplifiant très fort, le shell (ou _interpréteur de commandes_ est le _programme_ qui s'occupe de l'interface de la ligne de commande : il traite les commandes (éventuellement les corrige), et s'occupe de l'affichage des résultats.  
Il en existe une floppée (`bash`, `zsh`, `fish`, `cmd.exe`, `powershell`), certains offrant des services de complétion des commandes, de raccourcis, de gestion d'historique, correction prédictive et j'en passe.  
Dans le cadre du cours, nous utiliserons [bash](https://fr.wikipedia.org/wiki/Bourne-Again_shell), le shell le plus commun sur les systèmes Linux/UNIX.

#### Invite de commande

Lorsque vous utilisez un shell, le programme affiche ce qu'on appelle une invite de commande : un petit morceau de texte _invitant_ à taper votre commande à sa suite.  
Aussi apellé **prompt**, il apparaît donc après chaque commande pour taper la suivante.  
Par défaut, le prompt affiche le *nom de l'utilisateur connecté*, suivi de sa *position dans l'arborescence* des fichiers, et enfin le signe `$`, qui indique traditionnellement la fin du prompt.

> **Note :** dans la suite de ce document, tout bloc de code commencant par un `$` signifie que la suite est une commande à entrer dans votre console. Le signe dollar ne fait bien sûr par partie de la commande en question.

Le prompt peut être personnalisé et agrémenté de toutes sortes d'informations utiles. Pour quelques exemples, je vous invite à consulter la [galerie de thèmes de OhMyZsh](https://zshthem.es/all/), un outil qui permet (entre autres) de configurer son prompt sous zsh. 

### Structure de base d'une commande

	$ command -f -b 12 --long-flag arg1 arg2…
	
Une commande se décompose en trois parties : 

1. la commande
2. les options
3. les arguments

Une commande peut-être un mot-clé ou petit programme fourni par le système, ou un programme tiers qui a requis une installation.

Certaines commandes peuvent recevoir des options, qui sont exprimées (la *plupart* du temps) sous la forme de _flag_, qui peuvent être **courts** (une lettre précédée d'un tiret) ou **longs** un mot précédé de deux tirets. Ces options peuvent parfois recevoir des valeurs, indiquées directement après l'option, ou avec le signe égal.

Certaines commandes peuvent recevoir des arguments, qui sont exprimés en fin de ligne, et désignent souvent la cible de la commande.

### Obtenir de l'aide

La plupart des commandes ont une documentation, accessible via la commande `man` (pour _manuel_).

	$ man ssh
	
Ou parfois via le flag `-h` ou `--help`, si `man` ne retourne rien.