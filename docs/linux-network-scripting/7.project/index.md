---
title: "Projet Final"
description: "Vue d'ensemble du projet de mini serveur de fichiers."
---

# Projet Final : Mon Mini Serveur de Fichiers

## Préambule

Vous pouvez utiliser netcat comme serveur en l'exécutant avec un script shell. Lorsque vous exécutez netcat en mode écoute (`nc -l -p <port>`), il attend les connexions entrantes. Cependant, netcat seul ne gère qu'une seule connexion puis se termine.

Pour créer un serveur persistant qui peut gérer plusieurs connexions ou traiter des commandes, vous pouvez rediriger l'entrée et la sortie de netcat vers un script shell. Le script lira les commandes du client et renverra les réponses.

```bash
nc -l -p 8080 | ./my_script.sh
```

Dans cet exemple, lorsqu'un client se connecte au port 8080, netcat passera les données reçues à `my_script.sh` via stdin, et la sortie du script sera renvoyée au client.

### Exercice préliminaire : Echo Server

Créez un script simple nommé `echo_server.sh` qui :
- Lit une ligne depuis stdin
- Renvoie la ligne reçue avec un préfixe "Vous avez dit : "
- S'exécute dans une boucle pour gérer plusieurs lignes

**Test :**
1. Exécutez le serveur : `nc -l -p 8080 | ./echo_server.sh`
2. Dans un autre terminal, connectez-vous : `nc localhost 8080`
3. Tapez un message et voyez-le renvoyé avec le préfixe.
