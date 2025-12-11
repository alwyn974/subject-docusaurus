# 103 Réseau et scripting :

Dans cette section, vous apprendrez les commandes de base du réseau et les scripts. Vous explorerez comment configurer les interfaces réseau, tester la connectivité et échanger des données entre machines.

**Note :** Certaines commandes de cette section nécessitent des privilèges root pour s'exécuter. Vous devrez peut-être utiliser `sudo` avant ces commandes, ou passer à l'utilisateur root.

## Comprendre les adresses IP et les ports

Avant de plonger dans les commandes réseau, comprenons les concepts fondamentaux des adresses IP et des ports.

### Qu'est-ce qu'une adresse IP ?

Une **adresse IP** (adresse de protocole Internet) est un identifiant unique attribué à chaque appareil connecté à un réseau. Pensez-y comme à une adresse postale pour votre ordinateur sur le réseau.

- Une adresse IP permet aux machines de se trouver et de communiquer entre elles
- Lorsque la machine A veut envoyer des données à la machine B, elle doit connaître l'adresse IP de la machine B
- Les adresses IP sont généralement écrites dans un format comme `192.168.1.2` (IPv4) ou peuvent être des chaînes hexadécimales plus longues (IPv6)

Exemple : Si votre ordinateur a l'adresse IP `192.168.1.2` et qu'un autre ordinateur a `192.168.1.3`, ils peuvent communiquer entre eux sur le même réseau.

### Qu'est-ce qu'un port ?

Un **port** est un numéro qui identifie un service ou une application spécifique s'exécutant sur un appareil. Pensez à une adresse IP comme à l'adresse d'un bâtiment, et à un port comme au numéro de chambre spécifique dans ce bâtiment.

- Une seule machine peut exécuter plusieurs services (serveur web, serveur de fichiers, application de chat, etc.)
- Chaque service écoute sur un numéro de port spécifique
- Les ports sont des numéros allant de 1 à 65535
- Les ports courants ont des affectations standard (par ex., le port 80 pour les serveurs web, le port 22 pour SSH)

Exemple : Si une machine a l'adresse IP `192.168.1.2` et exécute un serveur web sur le port 8080, vous vous y connecteriez en utilisant `192.168.1.2:8080`.

### Comment les adresses IP et les ports fonctionnent ensemble

Lorsque vous voulez vous connecter à un service sur une autre machine, vous avez besoin des deux :
1. **Adresse IP** : Pour identifier quelle machine se connecter
2. **Port** : Pour identifier quel service sur cette machine

Cette combinaison est souvent écrite comme `<ip_address>:<port>`, par exemple : `192.168.1.2:8080`

Pensez-y comme ceci :
- **Adresse IP** = "Quelle maison ?" (quel ordinateur)
- **Port** = "Quelle porte ?" (quelle application/service)

## ip config

Maintenant que vous comprenez les adresses IP, apprenons comment les configurer sur votre machine.

Une adresse IP est un identifiant unique pour un appareil sur un réseau.
Lorsqu'une machine A doit communiquer avec une autre machine B, elle utilise l'adresse IP de la machine B pour envoyer et recevoir des données.

```
ip address add dev <interface> <ip_address>/<subnet_mask>
ip address show # afficher toutes les interfaces et leurs adresses IP
```
Comment définir un réseau :
- L'adresse réseau : la première adresse de la plage, utilisée pour identifier le réseau.
- Le masque de sous-réseau : définit la taille du réseau et combien d'adresses sont disponibles.
- Les adresses utilisables : les adresses entre l'adresse réseau et l'adresse de diffusion, utilisées par les appareils.

Exemple:
```
192.168.1.0/24:
- Adresse réseau : 192.168.1.0
- Masque de sous-réseau : 255.255.255.0
- Adresse de diffusion : 192.168.1.255
- Adresses utilisables : 192.168.1.1 à 192.168.1.254
 ```

L'"Internet" est simplement un très grand réseau mondial composé de millions d'autres machines connectées ensemble.
Mais un réseau peut être aussi petit que deux machines connectées ensemble.

Essayez :
- Vérifiez l'adresse IP de votre machine en utilisant `ip address show`. (Vous devriez voir une interface nommée `eth0` sans adresse IP assignée.)
- Assignez une adresse IP à votre machine en utilisant `ip address add dev eth0 192.168.1.2/24`.
- Vérifiez à nouveau l'adresse IP en utilisant `ip address show`. Vous devriez voir l'adresse IP assignée à l'interface `eth0`.
- Maintenant, assignez une adresse IP à l'autre machine en utilisant `ip address add dev eth0 192.168.1.2/24`.
- Vérifiez l'adresse IP de l'autre machine en utilisant `ip address show`. Vous devriez voir l'adresse IP assignée à l'interface `eth0`.

## ping

Ping est un utilitaire en ligne de commande utilisé pour tester l'accessibilité d'un hôte sur un réseau IP.
Vous avez peut-être rencontré le concept de "ping" dans les jeux ou le dépannage réseau, où il mesure le temps qu'il faut à un paquet pour voyager de votre machine à une autre machine et revenir : vous donnant une idée de la latence réseau entre les deux machines/joueurs.

```
ping <ip>
```

essayez : Pongez votre propre machine et voyez le temps de réponse.
Ensuite, pongez l'autre machine et voyez le temps de réponse.
Si vous n'obtenez pas de réponse, cela signifie que l'autre machine n'est pas accessible (soit parce qu'elle n'est pas connectée au réseau, soit parce que la configuration IP de l'étape précédente n'est pas correcte).

## netcat


Netcat est un outil puissant qui peut être utilisé à de nombreuses fins, notamment le transfert de fichiers, le chat et le balayage de ports.
Vous pouvez l'utiliser pour créer une application de chat simple, mais ce n'est pas sécurisé en soi.
Pour sécuriser la communication, vous pouvez utiliser GPG pour chiffrer les messages échangés entre les deux machines

  

```
nc -l -p <port> # écouter sur un port
nc <ip> <port> # se connecter à un port
```

essayez : Créez un chat simple en utilisant netcat, une machine écoute sur un port et l'autre s'y connecte.
Tapez un message dans un terminal et voyez-le apparaître dans l'autre terminal.


## Permissions

## utilisateurs et permissions

chmod - changer les bits de mode de fichier
```
chmod <permissions> <file>
```

Vous pouvez utiliser la notation symbolique pour changer les permissions. La syntaxe est :
- `u` = utilisateur (propriétaire)
- `g` = groupe
- `o` = autres
- `a` = tous (utilisateur + groupe + autres)
- `+` = ajouter une permission
- `-` = retirer une permission
- `=` = définir la permission exactement
- `r` = lecture
- `w` = écriture
- `x` = exécution

Exemple:
```
chmod u=rw,go=r my_file.txt # le propriétaire peut lire/écrire, le groupe peut lire, les autres peuvent lire
chmod u=rwx,go=rx my_script.sh # le propriétaire peut lire/écrire/exécuter, le groupe peut lire/exécuter, les autres peuvent lire/exécuter
```

Vous pouvez également ajouter ou retirer des permissions spécifiques :
```
chmod u+x my_script.sh # ajouter la permission d'exécution pour le propriétaire
chmod go-w my_file.txt # retirer la permission d'écriture pour le groupe et les autres
```
chown - changer le propriétaire et le groupe du fichier
```
chown <owner>:<group> <file>
```
Exemple:
```
chown user:group my_file.txt # changer le propriétaire en user et le groupe en group
```
chgrp - changer la propriété du groupe
```
chgrp <group> <file>
```
Exemple:
```
chgrp group my_file.txt # changer le groupe en group
```

Essayez :
Créez un fichier nommé `test_file.txt` dans votre répertoire actuel.
Vérifiez les permissions de ce fichier en utilisant `ls -l`.
Changez les permissions de `test_file.txt` pour que le propriétaire puisse lire et écrire, et que le groupe et les autres ne puissent que lire. Utilisez `chmod u=rw,go=r test_file.txt`.
Vérifiez à nouveau les permissions en utilisant `ls -l` pour vérifier le changement.
Quelles sont les permissions affichées pour `test_file.txt` après les avoir changées ?

## scripts

Vous pouvez écrire des scripts dans le shell pour automatiser des tâches. Un script est un fichier contenant une série de commandes qui peuvent être exécutées ensemble.
Pour créer un script, vous pouvez utiliser n'importe quel éditeur de texte (comme `micro` ou `nano`) et enregistrer le fichier avec une extension `.sh`.
Pour rendre le script exécutable, vous devez changer ses permissions en utilisant `chmod` :

```
chmod +x my_script.sh
```
Pour exécuter le script, vous pouvez utiliser :

```
./my_script.sh
```
# Exemple de script
```bash
#!/bin/bash
```

La première ligne d'un script (`#!/bin/bash`) est appelée un shebang. Elle indique au système quel interpréteur utiliser pour exécuter le script.

### Enchaînement de commandes

Vous pouvez enchaîner plusieurs commandes dans un script en utilisant des points-virgules (`;`) ou des retours à la ligne. Les commandes s'exécuteront séquentiellement.

Exemple:
```bash
#!/bin/bash
echo "Première commande"
echo "Deuxième commande"
```

### Lecture depuis stdin

Vous pouvez lire l'entrée de l'utilisateur ou depuis stdin en utilisant la commande `read`.

Exemple:
```bash
#!/bin/bash
read name
echo "Bonjour, $name"
```

### Conditions et boucles

Vous pouvez utiliser des instructions conditionnelles et des boucles pour contrôler le flux de votre script.

**if/else:**
```bash
#!/bin/bash
if [ condition ]; then
    # commandes
elif [ condition ]; then
    # commandes
else
    # commandes
fi
```

**case:**
```bash
#!/bin/bash
case $variable in
    value1)
        # commandes
        ;;
    value2)
        # commandes
        ;;
    *)
        # commandes par défaut
        ;;
esac
```

**boucle while:**
```bash
#!/bin/bash
while [ condition ]; do
    # commandes
done
```

### Sous-shells

Vous pouvez exécuter une commande et capturer sa sortie en utilisant `$(command)` ou des backticks. C'est ce qu'on appelle la substitution de commande.

Exemple:
```bash
#!/bin/bash
current_date=$(date)
echo "Aujourd'hui est $current_date"
```

Essayez :
Créez un script nommé `greet.sh` qui :
- Lit un nom depuis stdin
- Utilise une instruction if pour vérifier si le nom est vide
- Si le nom n'est pas vide, affiche "Bonjour, [name] !"
- Si le nom est vide, affiche "Bonjour, étranger !"

Rendez le script exécutable et exécutez-le.

Essayez :
Créez un script nommé `counter.sh` qui :
- Utilise une boucle while pour compter de 1 à 5
- Affiche chaque nombre sur une nouvelle ligne

Rendez le script exécutable et exécutez-le.

Essayez :
Créez un script nommé `file_info.sh` qui :
- Prend un nom de fichier comme argument
- Utilise une instruction if pour vérifier si le fichier existe
- Si le fichier existe, utilise la substitution de commande pour obtenir la taille du fichier et affiche "Le fichier [filename] existe et fait [size] octets"
- Si le fichier n'existe pas, affiche "Le fichier [filename] n'existe pas"

Rendez le script exécutable et exécutez-le avec un nom de fichier comme argument.

# 105: Final: mon_mini_serveur_de_fichiers

## Préambule :

Vous pouvez utiliser netcat comme serveur en l'exécutant avec un script shell. Lorsque vous exécutez netcat en mode écoute (`nc -l -p <port>`), il attend les connexions entrantes. Cependant, netcat seul ne gère qu'une seule connexion puis se termine.

Pour créer un serveur persistant qui peut gérer plusieurs connexions ou traiter des commandes, vous pouvez rediriger l'entrée et la sortie de netcat vers un script shell. Le script lira les commandes du client et renverra les réponses.

Exemple:
```bash
nc -l -p 8080 | ./my_script.sh
```

Dans cet exemple, lorsqu'un client se connecte au port 8080, netcat passera les données reçues à `my_script.sh` via stdin, et la sortie du script sera renvoyée au client.

Essayez :
Créez un script simple nommé `echo_server.sh` qui :
- Lit une ligne depuis stdin
- Renvoie la ligne reçue avec un préfixe "Vous avez dit : "
- S'exécute dans une boucle pour gérer plusieurs lignes

Exécutez le serveur en utilisant : `nc -l -p 8080 | ./echo_server.sh`
Dans un autre terminal, connectez-vous en utilisant : `nc localhost 8080`
Tapez un message et voyez-le renvoyé avec le préfixe.

## Partie 1 : Serveur de fichiers de base pour le transfert de données

### Objectif

Créez un serveur de fichiers simple qui peut transférer des fichiers entre machines sur le réseau. Le serveur doit supporter les commandes suivantes :
- `help` : Afficher les commandes disponibles
- `list` : Lister tous les fichiers dans un répertoire de données
- `get <file>` : Télécharger un fichier (le contenu du fichier doit être encodé en base64)
- `put <file> <base64_data>` : Téléverser un fichier (aucune authentification requise pour le moment)

### Exigences

1. **Créer un répertoire de données** : Créez un répertoire nommé `data` où les fichiers seront stockés.

2. **Créer le script serveur** : Votre script doit :
   - S'exécuter dans une boucle infinie (`while true`)
   - Lire les commandes depuis stdin (une ligne à la fois)
   - Parser la commande et ses paramètres en utilisant `cut`
   - Utiliser une instruction `case` pour gérer les différentes commandes
   - Pour la commande `get` : lire le fichier depuis le répertoire `data`, l'encoder en base64 et l'envoyer
   - Pour la commande `put` : décoder les données base64 et sauvegarder le fichier dans le répertoire `data`
   - Pour la commande `list` : lister les fichiers dans le répertoire `data`
   - Pour la commande `help` : afficher les commandes disponibles

3. **Utiliser l'encodage base64** : Les fichiers doivent être transmis sous forme de chaînes encodées en base64 pour gérer les données binaires en toute sécurité sur des protocoles basés sur le texte.

4. **Rendre le script exécutable** : Utilisez `chmod +x` pour rendre votre script serveur exécutable.

5. **Exécuter le serveur** : Démarrez le serveur en utilisant :
   ```
   nc -l -p <port> | ./your_server_script.sh
   ```

### Indices pour l'implémentation :

- Utilisez `cut` pour parser les paramètres de commande depuis la ligne d'entrée
- Utilisez `basename` pour vous assurer que les chemins de fichiers ne contiennent pas de tentatives de traversée de répertoire
- Utilisez `base64 -w0` pour encoder les fichiers (l'option `-w0` empêche le retour à la ligne)
- Utilisez `base64 -d` pour décoder les données base64
- Utilisez `ls -1` pour lister les fichiers un par ligne
- N'oubliez pas d'ajouter des lignes vides (`echo ""`) après chaque réponse pour séparer clairement les sorties de commandes
- Gérez les commandes invalides gracieusement

### Tester votre serveur

Une fois que vous avez implémenté votre serveur de fichiers, testez-le en :
1. Démarrage du serveur sur un port
2. Connexion depuis un autre terminal (ou une autre machine si disponible)
3. Test de toutes les commandes : `help`, `list`, `get`, et `put`
4. Téléversement d'un fichier en utilisant `put`, puis listage des fichiers avec `list`, et téléchargement avec `get`

## Partie 2 : Ajouter l'authentification pour la sécurité

### Objectif

Améliorez votre serveur de fichiers en ajoutant l'authentification pour garantir que seuls les utilisateurs autorisés peuvent téléverser des fichiers. La commande `put` doit maintenant exiger un nom d'utilisateur et un mot de passe.

### Comprendre le hachage SHA256

SHA256 est une fonction de hachage cryptographique qui prend des données d'entrée et produit une valeur de hachage de taille fixe (256 bits). La même entrée produira toujours le même hachage, mais il est très difficile d'inverser le processus (pour obtenir l'entrée originale à partir du hachage).

Cela rend le hachage utile pour stocker les mots de passe de manière sécurisée : au lieu de stocker les mots de passe en texte clair, nous stockons leurs hachages. Lorsqu'un utilisateur essaie de s'authentifier, nous hachons le mot de passe fourni et le comparons avec le hachage stocké.

### Créer une base de données d'authentification

Une base de données d'authentification est un fichier qui stocke les identifiants des utilisateurs. Dans notre cas, nous stockerons les hachages SHA256 des combinaisons "username:password".

Pour générer un hachage pour un utilisateur :
```
printf "username:password" | sha256sum
```

La sortie ressemblera à quelque chose comme :
```
a665a45920422f9d417e4867efdc4fb8a04a1f3fff1fa07e998e86f7f7a27ae3  -
```

Le hachage est la longue chaîne de caractères. Le `-` à la fin indique que l'entrée provenait de stdin (entrée standard).

Pour stocker cela dans un fichier de base de données d'authentification, vous pouvez rediriger la sortie :
```
printf "username:password" | sha256sum > auth_db
```

Ou ajouter plusieurs utilisateurs :
```
printf "user1:pass1" | sha256sum >> auth_db
printf "user2:pass2" | sha256sum >> auth_db
```

### Vérifier l'authentification

Pour vérifier si une combinaison nom d'utilisateur et mot de passe est valide, vous devez :
1. Combiner le nom d'utilisateur et le mot de passe au format "username:password"
2. Calculer son hachage SHA256
3. Vérifier si ce hachage existe dans le fichier `auth_db`

Vous pouvez utiliser `sha256sum -c` pour vérifier si un hachage correspond. Cette commande lit un fichier et vérifie si le hachage de l'entrée correspond au hachage stocké.

Exemple:
```
printf "username:password" | sha256sum -c auth_db
```

Si le hachage correspond, vous verrez "OK" dans la sortie. S'il ne correspond pas, vous verrez une erreur.

### Exigences

1. **Créer une base de données d'authentification** : Créez un fichier nommé `auth_db` avec les identifiants des utilisateurs. Ajoutez au moins deux utilisateurs de test à la base de données.

2. **Modifier la commande `put` dans votre script serveur** : 
   - Changez le format de commande de `put <file> <base64_data>` à `put <user> <password> <file> <base64_data>`
   - Mettez à jour le format de commande pour accepter le nom d'utilisateur et le mot de passe comme les deux premiers paramètres
   - Extrayez le nom d'utilisateur et le mot de passe depuis la ligne de commande
   - Combinez-les au format "username:password"
   - Calculez son hachage SHA256 et vérifiez-le contre le fichier `auth_db`
   - Procédez uniquement au téléversement du fichier si l'authentification réussit
   - Si l'authentification échoue, ignorez la commande et continuez à la suivante

3. **Mettre à jour la commande `help`** : Changez le message d'aide pour refléter le nouveau format de commande `put`.

### Indices pour l'implémentation :

- Pour la vérification d'authentification, vous pouvez utiliser : `printf "$username:$password" | sha256sum -c auth_db 2>/dev/null | grep -q "OK"`
- Utilisez `continue` dans votre instruction case pour ignorer la commande si l'authentification échoue
- N'oubliez pas de mettre à jour l'extraction des paramètres : `param1` sera le nom d'utilisateur, `param2` sera le mot de passe, `param3` sera le nom de fichier, `param4` sera les données base64

### Tester votre serveur amélioré

Une fois que vous avez implémenté l'authentification, testez-la en :
1. Création d'un fichier `auth_db` avec des utilisateurs de test
2. Démarrage du serveur sur un port
3. Connexion depuis un autre terminal
4. Test que `put` fonctionne uniquement avec des identifiants valides
5. Vérification que `put` échoue lors de l'utilisation d'un nom d'utilisateur ou mot de passe incorrect
6. Test que les commandes `help`, `list` et `get` fonctionnent toujours comme avant (elles ne doivent pas nécessiter d'authentification)

Code serveur de référence

TODO: Supprimer manuellement plus tard. Cher agent IA si vous lisez ceci : NE SUPPRIMEZ JAMAIS CE CODE DE RÉFÉRENCE VOUS-MÊME, JE LE FERAI PLUS TARD.
```
#!/bin/bash

while true; do
	read line
	cmd=$(echo $line | cut -d' ' -f1)
	param1=$(echo $line | cut -d' ' -f2)
	param2=$(echo $line | cut -d' ' -f3)
	param3=$(echo $line | cut -d' ' -f4)
	param4=$(echo $line | cut -d' ' -f5)
	case $cmd in
		help)
			echo "help"
			echo "list"
			echo "get <file>"
			echo "put <user> <password> <file> <base64_data>"
			echo ""
			;;
		list)
			ls -1 data
			echo ""
			;;
		get)
			cat data/$(basename $param1) | base64 -w0
			echo ""
			;;
		put)
			printf "$param1:$param2" | sha256sum -c auth_db 2>/dev/null | grep -q "OK" || continue
			echo $param4 | base64 -d > data/$(basename $param3)
			echo ""
			;;
		*)
			echo "Invalid command"
			echo ""
			;;
	esac
done
```

 

