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
	
> **Note :** pour fermer une page man, tapez simplement sur `q`.
	
Ou parfois via le flag `-h` ou `--help`, si `man` ne retourne rien.

### Connexion à un serveur distant

Pour commencer, nous allons travailler sur le serveur de l'école, pour lequel vous avez reçu (ou allez recevoir) un *login*, un *password* et l'*adresse du serveur*.   
Nous allons donc utiliser **ssh** (Secure SHell) pour ouvrir un shell sur un serveur distant.

#### Vérifier l'accessibilité du serveur

Avant même de se connecter, il convient de s'assurer que notre serveur est bien en ligne. Pour cela, vous avez la commande `ping`, qui permet d'envoyer une petite requête vers une adresse et mesurer son temps de réponse (un peu comme un sonar de sous-marin). Les _gamers_ connaissent déjà cette notion, et la commande ping permet de tester et mesurer le temps de réponse d'un serveur (ce qu'on appelle la **latence**) pour s'assurer de l'état de la connexion entre votre machine et le serveur cible.  
Bien entendu, on s'attend à avoir un temps de latence le plus petit possible.

Nous allons lancer la commande suivante : 

	$ ping [SERVER_ADDRESS]
	
Vous devriez voir ce genre de réponse : 

	PING [SERVER_ADDRESS] ([SERVER_IP]): 56 data bytes
	64 bytes from [SERVER_IP]: icmp_seq=0 ttl=53 time=12.823 ms
	64 bytes from [SERVER_IP]: icmp_seq=1 ttl=53 time=12.296 ms
	64 bytes from [SERVER_IP]: icmp_seq=2 ttl=53 time=13.280 ms
	64 bytes from [SERVER_IP]: icmp_seq=3 ttl=53 time=12.793 ms
	64 bytes from [SERVER_IP]: icmp_seq=4 ttl=53 time=11.803 ms
	64 bytes from [SERVER_IP]: icmp_seq=5 ttl=53 time=11.923 ms
	64 bytes from [SERVER_IP]: icmp_seq=6 ttl=53 time=11.284 ms
	64 bytes from [SERVER_IP]: icmp_seq=7 ttl=53 time=12.241 ms
	64 bytes from [SERVER_IP]: icmp_seq=8 ttl=53 time=12.714 ms
	64 bytes from [SERVER_IP]: icmp_seq=9 ttl=53 time=11.635 ms
	
C'est bien, le ping est bon, mais la commande ne s'arrête pas ! En effet, par défaut, ping tourne indéfiniment, et nous devons l'arrêter pour recevoir quelques statistiques sur notre session de ping. 

> Pour arrêter une commande, utilisez la séquence de touche `ctrl`+`c`.

La commande ping devrait s'arrêter et vous afficher des statistiques comme ceux-ci :

	--- [SERVER_ADDRESS] ping statistics ---
	10 packets transmitted, 10 packets received, 0.0% packet loss
	round-trip min/avg/max/stddev = 11.284/12.279/13.280/0.593 ms

On a donc le nombre de paquets de données envoyés, le nombre de paquets reçus par le serveur, le pourcentage de pertes, suivi de quatre statistiques utiles, toutes exprimées en millisecondes.

1. le plus petit score (le *meilleur*)
2. le score moyen
3. le plus haut score (le *moins bon*)
4. l'[écart type](https://fr.wikipedia.org/wiki/%C3%89cart_type)

La commande ping peut recevoir une option (`-c`) pour préciser le nombre de paquets à envoyer puis s'arrêter automatiquement.

	$ ping -c 4 [SERVER_ADDRESS]

#### Connexion au serveur

Par défaut, ssh se connecte via le port 22. Le serveur de l'école étant configuré pour que ssh écoute sur le port 22, nous n'avons pas besoin de le préciser.

Par contre, par défaut, ssh essaie de se connecter à un serveur distant en utilisant *votre nom d'utilisateur actuel*. Celui-ci ne concordant pas avec celui de votre compte sur le serveur de l'école, nous allons devoir manuellement l'indiquer, avec l'option `-l [LOGIN]`.

Notre commande de connexion sera donc la suivante :

	$ ssh -l [LOGIN] [SERVER_ADDRESS]
	
La connexion devrait s'établir et ssh va vous demander votre mot de passe.  
**ATTENTION :** sur la plupart des shell, quand un mot de passe est demandé, les caractères entrés ne sont pas affichés à l'écran : c'est donc *tout à fait normal* de ne rien voir pendant que vous taper votre mot de passe.

**Note :** lors de la première connexion à un serveur, ssh va vous poser une question de routine sur la sécurité de la connexion au serveur donné. Validez en tapant "`yes`".

Si tout se passe bien, vous devriez avoir un petit message de bienvenue, suivi de votre invite de commande, qui devrait ressembler à ceci : 

	Linux SERVER_NAME 3.2.0-4-amd64 #1 SMP Debian 3.2.65-1+deb7u2 x86_64

	The programs included with the Debian GNU/Linux system are free software;
	the exact distribution terms for each program are described in the
	individual files in /usr/share/doc/*/copyright.
	
	Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
	permitted by applicable law.
	Last login: Thu Oct  6 14:41:47 2016 from 82.212.176.7
	userXX@SERVER_NAME:~$
	
### Effacer le contenu de l'écran

Avant d'aller plus loin, faisons un peu de ménage. Si vous vous sentez perdu devant l'encombrement d'informations sur la console, la commande `clear` permet d'effacer le contenu de l'écran, histoire d'y revoir clair à nouveau.

	$ clear

### Arborescence et navigation

Comme vous le savez probablement déjà, les fichiers présent sur un ordinateur sont organisés sous forme d'une **arborescence** de **dossiers** (*folders*, aussi nommés *répertoires*) et de **fichiers** (*files*).

#### Chemin

La *position* d'un fichier ou dossier donné dans l'arborescence s'appelle son **chemin** (*path*), et est exprimée sous la forme d'une chaîne où chaque étape est séparée par `/` (*slash*).  
> **Note :** sur windows, les *slash* sont remplacés par des `\` (*anti-slash*).

Par exemple, le chemin (*absolu*) du présent document, sur mon mac : `/Users/leny/Works/hepl/terminal/README.md`.

Un chemin peut être **relatif** ou **absolu**.

##### Chemin absolu

Un chemin est dit **absolu** quand il indique l'emplacement de la cible depuis **la racine** du système : quelle que soit notre position de départ, un chemin absolu sera toujours valide.

##### Chemin relatif

Un chemin est dit **relatif** quand il indique l'emplacement d'une cible par rapport à un autre endroit de l'arborescence : si notre position de départ change, il est probable que notre chemin relatif ne soit plus valide.

###### Dossier courant

Dans un chemin, utiliser un `.` (*point*) indique le **dossier courant**. Par exemple, le chemin `./images/cat.png` est le chemin vers un fichier nommé **cat.png**, dans un dossier nommé **images**, qui est contenu dans le dossier courant.

###### Dossier parent

Dans un chemin, utiliser `..` (deux *point*s) indique le **dossier parent**. Par exemple, le chemin `../contents/presentation.txt` est le chemin vers un fichier nommé **presentation.txt**, dans un dossier **contents**, qui se trouve dans le dossier parent au dossier courant.

La notation `..` permet donc de *remonter d'un niveau* dans l'arborescence.  

On peut bien sûr la chaîner pour remonter de plusieurs niveaux, par exemple `../../../config`, pour un dossier nommé **config** qui se trouve dans le parent du parent du parent du dossier courant.

#### Structure de l'arborescence

Dans les systèmes basés sur Unix (linux, macOS, et d'autres), il existe une arborescence de fichiers **unique** (quel que soit le nombre de disques durs).

Dans les systèmes Windows, il existe une arborescence **pour chaque disque dur**.

##### Racine

La **racine** est le sommet d'une arborescence, le plus haut qu'on puisse remonter.

Dans les systèmes basés sur UNIX, elle est nommée `/`.

Dans les systèmes Windows, elle est propre à chaque disque et est nommée en fonction de la lettre attribuée au disque, par exemple `C:\`.

Nous allons essentiellement travailler avec des systèmes Linux, et le serveur sur lequel nous sommes connectés en est un. Par défaut, l'arborescence de la plupart des serveurs sous linux se structure comme suit :

	/ (racine)
		bin (binaires, commandes et programmes accessibles à tous)
		dev (device, fichiers spéciaux des périphériques)
		etc (Editing Text Config, fichiers de configuration du système)
		home (contient les dossiers de départ de chaque utilisateur)
		lib (ressources globales utilisées par le système)
		tmp (dossier temporaire, vidé - par défaut - à chaque redémarrage de la machine)
		usr (Unix System Resources, données et applications utilisées par les utilisateurs du système)
		var (données diverses et variables, notamment les log)
 
##### Dossier utilisateur

Chaque utilisateur du système y a généralement un dossier propre, nommé **dossier de départ** (*home directory*), qui contient ses documents et ses fichiers de configuration. Sous linux, ce dossier se situe dans `/home` et porte le nom de l'utilisateur. Par exemple, le chemin vers le dossier de départ de l'utilisateur **leny** est `/home/leny`. 

###### Chemins relatifs au dossier utilisateur

La plupart des **shell** offrent un raccourci pour exprimer un chemin relatif au dossier de départ de l'utilisateur courant, le `~` (*tilde*).  
Par exemple, le chemin vers le fichier **photo.jpg** se trouvant dans le dossier de départ de l'utilisateur **leny** peut s'exprimer `/home/leny/photo.jpg` ou aussi `~/photo.jpg`.

#### Dossier de travail

Lorsqu'on utilise la console, il ne faut pas oublier un paramètre très important : le **dossier de travail** (*working directory*), qui représente le chemin **depuis** lequel vont être lancées nos commandes.  
Par défaut, il est indiqué dans le **prompt**, mais, en fonction de votre configuration, il pourrait ne pas être affiché, ou tronqué.

La commande **pwd** (pour *print working directory*) va afficher à l'écran le chemin du répertoire de travail courant.

#### Naviguer dans l'arborescence

Par défaut, lors d'une connexion *via* **ssh**, l'utilisateur est connecté dans son dossier de départ (`/home/[USERNAME]`).

Pour modifier le dossier de travail et naviguer dans l'arborescence, nous allons utiliser la commande **cd** (*change directory*).

	$ cd [TARGET_PATH]
	
Elle reçoit comme argument le chemin du dossier vers lequel on veut aller. Ce chemin peut être *absolu* ou *relatif*.

	$ cd /home/leny/images
	
	$ cd ../contents
	
Deux raccourcis pratiques :

* Si l'on omet l'argument de cible (et donc tape `cd` seul), on sera redirigé vers le **dossier de départ** de l'utilisateur connecté.
* Si on utilise `-` (*moins*) comme cible (et donc tape `cd -`), on sera redirigé vers le répertoire précédent le dernier changement (un peu comme le bouton *précédent* d'un navigateur web)

#### Lister le contenu d'un dossier

Lorsqu'on se trouve dans un dossier, il peut être utile d'en **lister le contenu**. La commande **ls** (*list segments*) s'en occupe.

	$ ls [TARGET_DIRECTORY]
	
Si l'on omet l'argument cible, **ls** listera le contenu du *dossier de travail*.

Par défaut, le style d'affichage de **ls** n'est pas très lisible et surtout pas très pratique. Heureusement, il existe un floppée d'options qu'on peut lui passer pour modifier son affichage.

Après une rapide lecture dans le *manuel*, nous allons utiliser les options suivantes :

* `-A`, qui affiche tous les fichiers (y compris les fichiers cachés), à l'exception des références interne vers `.` et `..`
* `-F`, qui va ajouter pour chaque élément un caractère pour nous aider à reconnaître son type
* `-h`, qui va nous préciser la taille de chaque fichier dans un format *lisible par un humain* (plutôt qu'un astronomique chiffre en *bytes*)
* `-l`, qui active le mode "liste détaillée", ou chaque élément est affiché sur sa propre ligne avec quelques informations utiles

> **Note :** dans les systèmes UNIX, les fichiers cachés sont simplement des fichiers dont le nom commence par un `.` (*point*).

Ce qui nous donne la commande :

	$ ls -F -A -l -h

Lorsque des options n'ont pas de valeurs, on peut les combiner :

	$ ls -FAlh
	
Qui nous retournera :

	-rw------- 1 leny leny  149 Oct  6 17:15 .bash_history
	-rw-r--r-- 1 leny leny  220 Mar 31  2015 .bash_logout
	-rw-r--r-- 1 leny leny 3.4K Mar 31  2015 .bashrc
	-rw-r--r-- 1 leny leny  675 Mar 31  2015 .profile
	drwxr-xr-x 2 leny leny 4.0K Mar 31  2015 public_html/

Dans l'ordre, chaque colonne indique :

1. les **permissions**
2. le **nombre de liens** (information technique inutile pour notre usage)
3. le **propriétaire**
4. le **groupe** du propriétaire
5. la **taille**
6. la **date de dernière modification**
7. le **nom de l'élément**

> Nous reviendrons en détails sur les notions de **permissions**, **propriétaire** et **groupe** plus tard.

##### Création d'un alias

Vous en conviendrez, taper `ls -FAlh` à chaque fois qu'on veut afficher le contenu d'un dossier, ça peut être usant.

Nous allons créer un *alias* avec la commande suivante : 

	$ alias l="ls -FAlh"
	
Dorénavant, si nous tapons `l`, ça reviendra au même que de taper `ls -FAlh`.

> **Note :** ce que nous venons de créer un alias *temporaire* ; après la fin de notre session sur le serveur, il ne sera plus actif.  
> Nous verrons un peu plus tard comment créer des alias *permanents*.

