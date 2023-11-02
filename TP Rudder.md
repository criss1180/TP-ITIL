

# Déploiement de Rudder

​
## Contexte

Vous êtes un administrateur système dans une entreprise qui souhaite améliorer sa gestion des services informatiques en adoptant les principes ITIL. Votre mission consiste à configurer et à utiliser Rudder pour automatiser la gestion des configurations, renforcer la sécurité, installer des applications et gérer les utilisateurs.

​

## Tâche 1 : Installation et Configuration du Serveur Rudder

**1.1 Installation**

​
Installez Rudder Server sur la machine virtuelle serveur dédiée

​

>*sudo wget --quiet -O /etc/apt/trusted.gpg.d/rudder_apt_key.gpg "https://repository.rudder.io/apt/rudder_apt_key.gpg"*

​sudo echo "deb http://repository.rudder.io/apt/8.0/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/rudder.list


>*sudo apt-get update*

​
>*sudo apt-get install rudder-server*

​
>*sudo apt-get upgrade*

​
>*rudder server create-user -u cris*

​

**1.2 Validation**

​

Connectez-vous à l'interface web de Rudder, vérifiez l'état du serveur et assurez-vous qu'il est prêt à communiquer avec les agents.

​![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/a80de3ec-3336-44f3-8f87-ef959f187ab1)



​

## Tâche 2 : Installation et Configuration d'un Agent Rudder

**2.1 Machine client Linux**

​

Installez l'agent Rudder sur la machine cliente Linux, et configurez-la pour qu'elle communique avec le serveur.

​

>*sudo wget --quiet -O /etc/apt/trusted.gpg.d/rudder_apt_key.gpg "https://repository.rudder.io/apt/rudder_apt_key.gpg"*

​
echo "deb http://repository.rudder.io/apt/8.0/ $(lsb_release -cs) main" > /etc/apt/sources.list.d/rudder.list
​
apt-get update

>*sudo apt-get install rudder-agent*

​
>*rudder agent policy-server 212.47.237.144

​
>*sudo rudder agent inventory*

​

**2.2 Validation**

​

Assurez-vous que l'agent apparaisse dans l'interface web de Rudder et soit autorisée.

​
![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/46c50ee3-7668-40d8-95e9-35dbb805453b)


​

Tâche 3 : Renforcement des Configurations Systèmes

​
**3.1 Technique de Sécurité**

​
Sur les systèmes Linux :

​

>Création de comptes : S'assurer que les comptes "user01" et "user02" sont existants. User01 a un shell en "bash", user02 en "nologin". Les homes seront dans "/home/nom_user"

>>*sudo useradd -m -s /usr/bin/bash user01* 

>>*sudo useradd -m -s /usr/bin/nologin user02*
​

![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/c94a0ce9-b265-4db1-8fa6-b416ec652503)


​
>Désactivation des comptes : S'assurer que le compte "user02" est désactivé.

>>*sudo usermod -L user02*

​
>Restriction des permissions root : user01" doit avoir le droit d'utiliser sudo, uniquement pour consulter les log dans "/var/log/syslog".

>>*sudo nano /etc/sudoers*

>>Ajouter les lignes :

>>>#Allow user1 to view /var/log/syslog

>>>user1 ALL=(root) /bin/nano var/log/syslog/*

​

>Configuration du Pare-feu : Autoriser uniquement les ports 22 (SSH), 80 (HTTP), 5309 et 443 (HTTPS). Bloquer tous les autres ports.

>>*sudo ufw allow 22*

>>*sudo ufw allow 80*

>>*sudo ufw allow 5309*

>>*sudo ufw allow 443*

>>*sudo ufw enable*

​![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/ee388393-f778-4ec3-8a3d-a7c522e7eeed)


​

>Mise à jour des Packages : Appliquer toutes les mises à jour de sécurité disponibles.

>>*sudo apt-get update*

>>*sudo apt-get upgrade*

​

>Installation de Packages : Installer "fail2ban" et "apache2" et s'assurer du démarrage des services.

>>*sudo apt-get install apache2*


>>*sudo apt-get install fail2ban*

​
![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/cc7dc246-2b3d-4310-aec3-42bb38aa1d18)


​

>Déploiement de fichiers : Déployer une page web contenant l'IP du serveur.

![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/ed9d8523-9958-4ea7-96be-ca475cbe9cc8)




​

>Renforcer la configuration SSH, mais sans rester dehors.

>>Générer une paire de clé sur la machine locale

>>>*ssh-keygen*

>>>*cat ~/.ssh/id_rsa.pub*

>>>Copier le contenu

>>>Sur l'hôte distant, coller la clé dans /home/c.penu/.ssh/authorized_keys

>>>Passer le paramètre *PasswordAuthentification yes* => *PasswordAuthentification no* dans /etc/ssh/ssh_config

>>>*sudo nano /etc/ssh/ssh_config*

​
![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/65459c12-423c-4f04-b213-7142efea5842)



​## Tâche 4 : Audit et Conformité

​

**4.1 Audit des Configurations**

​

Utilisez les fonctionnalités d'audit de Rudder pour vérifier la conformité des machines avec les configurations souhaitées.

​
![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/f77c23af-4f03-4181-bf84-9b0ddc3e10c1)




![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/b196c06d-741e-455f-91eb-bfdd52c7c007)


​

**4.2 Rapport de Conformité**

​

Générez un rapport de conformité à partir de l'interface web de Rudder et analysez les résultats.

​​

## Tâche 5 : Résolution de Problèmes et Scénarios Pratiques

​

**5.1 Diagnostic**

​

![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/3ed2432a-8ab8-4ce2-b1a5-4f7865a6a0c2)


![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/1a720dda-c7b7-4563-b8f8-d6b45b5de2ba)


![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/8748a78f-7a6f-4517-964f-be2485b72a14)


**5.2 Scénario Pratique**

​

Répondez à une demande de changement fictive, en modifiant les configurations sur les machines via Rudder.

l'exécution d'une commande sudo apt update

![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/7df9ba57-1b48-4b79-ba91-e50d3bd13a46)
​

![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/c4225138-174a-44ae-b695-c9354b416dee)


​création d'un nouvel utilisateur

![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/ab139887-5933-41b1-98b2-ffaf0281bcd8)


![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/f41a85eb-b7fd-4b49-af40-89c8f8ab6cc5)


![image](https://github.com/criss1180/TP-ITIL-/assets/115303549/3ab40c4b-73c6-43f3-8b2c-ca428d58faf0)


## Tâche 6 : Intégration avec ITIL et Documentation

​

**6.1 Alignement ITIL**

​
Documentez comment les actions réalisées s'alignent avec les processus ITIL, en particulier la Gestion des Services et la Gestion des Configurations.

Chaque modification apportée fait l'objet d'un enregistrement décrivant la nature de la modification, son impact prévu, les risques potentiels et la validation effectuée avant le déploiement

Les configurations des serveurs, les fichiers système, les accès utilisateurs, sont gérées et enregistrées dans un référentiel centralisé

L'automatisation des déploiements assure la stabilité des services et la gestion efficace des configurations.  

**6.2 Rapport Final**

​

Rédigez un rapport détaillé incluant les étapes réalisées, les configurations appliquées OK

​

## Tâche 7 : Amélioration Continue et Recommandations

​

**7.1 Analyse**

​

Identifiez les domaines d'amélioration dans la manière dont Rudder est utilisé et proposez des actions concrètes.

Fiabilité et traçabilité des déploiements

Automatisation des correctifs

Analyse de conformité

Gestion des accès

Optimisation et évolution de l'infrastructure

​

**7.2 Recommandations**

​

Formulez des recommandations pour l'utilisation future de Rudder dans l'organisation, en vous basant sur les meilleures pratiques ITIL.

Compte tenu de sa capacité automatisation c'est un bon outil pour les déploiements en masse de correctifs et l'évolution de l'infra.
