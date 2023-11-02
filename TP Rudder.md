

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

​
![[Pasted image 20231102140015.png]]

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

![[Pasted image 20231102140611.png]]

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

<img src= "https://imgur.com/H0faw0L.jpg">

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

​
![[Pasted image 20231102140744.png]]
​

>Mise à jour des Packages : Appliquer toutes les mises à jour de sécurité disponibles.

>>*sudo apt-get update*

>>*sudo apt-get upgrade*

​

>Installation de Packages : Installer "fail2ban" et "apache2" et s'assurer du démarrage des services.

>>*sudo apt-get install apache2*


>>*sudo apt-get install fail2ban*

​
![[Pasted image 20231102140834.png]]

​

>Déploiement de fichiers : Déployer une page web contenant l'IP du serveur.

>>

​
![[Pasted image 20231102140858.png]]

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
<img src= "https://imgur.com/qFKnoLM.jpg">
![[Pasted image 20231102141016.png]]


​## Tâche 4 : Audit et Conformité

​

**4.1 Audit des Configurations**

​

Utilisez les fonctionnalités d'audit de Rudder pour vérifier la conformité des machines avec les configurations souhaitées.

​
![[Pasted image 20231102141208.png]]



![[Pasted image 20231102141134.png]]

​

**4.2 Rapport de Conformité**

​

Générez un rapport de conformité à partir de l'interface web de Rudder et analysez les résultats.

​​

## Tâche 5 : Résolution de Problèmes et Scénarios Pratiques

​

**5.1 Diagnostic**

​

![[Pasted image 20231102141435.png]]

![[Pasted image 20231102141517.png]]
​
![[Pasted image 20231102142426.png]]

**5.2 Scénario Pratique**

​

Répondez à une demande de changement fictive, en modifiant les configurations sur les machines via Rudder.

l'exécution d'une commande sudo apt update

![[Pasted image 20231102151130.png]]​

![[Pasted image 20231102151203.png]]

​création d'un nouvel utilisateur

![[Pasted image 20231102170344.png]]

![[Pasted image 20231102170544.png]]

![[Pasted image 20231102170530.png]]

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