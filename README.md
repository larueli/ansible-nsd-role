# Rôle Ansible pour NSD

Ce rôle Ansible vous permet de déployer NSD, un serveur DNS autoritatif solide et fiable sur des machines Linux (CentOS, Ubuntu).

Ce rôle peut déployer un serveur *maitre* et des serveurs *esclaves* rapidement et facilement.

Les fichiers de zones sont générés automatiquement.

## Fonctionnalités

* Support de plusieurs domaines
* Génération, sauvegarde et vérification des fichiers de configuration et de zones
* Mise à jour automatique du SOA
* AXFR sécurisé entre les DNS
* DNSSEC déployé automatiquement sur tous les domaines (génération, sauvegarde des clefs, signature des zones), affichage des clefs KSK et DS records


## Prérequis

Sur la machine exécutant le rôle :

* Ansible 2.8.5 minimum
* Une paire de clef SSH (voir ```ssh-keygen```)

## Utilisation

### 1) Récupération du repos :

> ```git clone https://github.com/larueli/ansible-nsd-role.git```


### 2) Modifier votre fichier *hosts* afin d'inclure tous vos serveurs dns dans le même groupe de machines, pour chacun définir une variable d'hôte :

> ```host_is_dnsmaster: yes``` si serveur maitre
> 
> ```host_is_dnsslave: yes``` si serveur esclave

### 3) Exécution du rôle ```ansible-ready``` pour s'assurer que les machines ont bien un accès SSH, et python installé

Si toutes vos machines ont les mêmes identifiants (machines ubuntu dans LXC par exemple), vous pouvez utiliser le playbook *init.yml* après l'avoir adapté.

> ```bash
> cd ansible-nsd-role
> ansible-playbook init.yml [-i hosts] -k -K

* -k : demande le mot de passe SSH
* -K : demande le mot de passe pour les commandes sudo

### 4) Modification des variables *vars* du rôle pour NSD

Modifiez le fichier ```roles/nsd/vars/main.yml``` pour : 

* ajouter vos domaines,
* spécifier les IP de vos serveurs
* spécifier la clef de transfert AXFR entre vos serveurs

Cette clef peut être générée avec la commande : 
>```dd if=/dev/random count=1 bs=32 2> /dev/null | base64```

La façon d'écrire les domaines est spécifiée dans le fichier.

### 5) Modification des variables *defaults* du rôle pour NSD

Modifiez le fichier ```roles/nsd/vars/main.yml``` pour : 

* définir la carte réseau utilisée (pour RHEL / CentOS)
* Modifier les niveaux de verbosité, les emplacements de log et de pidfile, et les valeurs de refuse ANY et hide version pour NSD (facultatif)
* Vous pouvez modifier l'algorithme DNSSEC ainsi que la taille des clefs (facultatif)

### 6) C'est parti !
Un playbook est déjà tout prêt pour ce rôle, adaptez-le à votre situation puis exécutez-le.
De même, en cas de modification des variables, ré-exécutez ce playbook.

> ```ansible-playbook nsd_playbook.yml [-i hosts]```


# Auteur

Je suis [Ivann LARUELLE](https://www.linkedin.com/in/ilaruelle/), étudiant-ingénieur en Réseaux et Télécommunications à l'[Université de Technologie de Troyes](https://www.utt.fr/), école publique d'ingénieurs.

Ce programme est réalisé originellement pour le [Service Informatique aux Associations](https://ung.utt.fr/tech/sia) de l'[UTT Net Group](https://ung.utt.fr/)

N'hésitez pas à me contacter pour me signaler tout bug ou remarque. Je suis joignable à ivann.laruelle[at]gmail.com.


# Licence et utilisation

Vous êtes libre de télécharger, utiliser, modifier et redistribuer ces sources à la seule condition de ne pas toucher à l'entête des fichiers yaml et de me créditer systématiquement.

Je ne suis en aucun cas responsable de toutes les conséquences (perte de données, interruption de service, pertes financières, ...) issues de l'utilisation de cet outil.

# Sources et remerciements

Un article du site [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-dnssec-on-an-nsd-nameserver-on-ubuntu-14-04#step-one-%E2%80%94-install-and-set-up-nsd-on-both-servers), écrit par [Jesin A](https://www.digitalocean.com/community/users/jesin) a été ma première piste.

La [documentation du site NLNETLABS](https://www.nlnetlabs.nl/documentation/), [organisation](https://www.nlnetlabs.nl/) qui développe notamment NSD (le serveur), et ldns (DNSSEC)

La [documentation d'Ansible](https://docs.ansible.com/) pour apprendre à utiliser Ansible.

# Après ?

* Traduction en anglais
* Utilisation d'OpenDNSSEC
