Etapes preparation serveur mail

1/ Mise à jour du serveur et configuration des fichiers /etc/hosts et /etc/hostname
2/ Push des backups si il y a et des certificats liés au domaine principal
3/ Si il y a un fichier de config, set les variables d'environnements suivants pour permettre de le prendre en compte:
AUTO_USE_EXISTING_CONFIG_FILE=y
AUTO_INSTALL_WITHOUT_CONFIRM=y
AUTO_CLEANUP_REMOVE_SENDMAIL=y
AUTO_CLEANUP_REPLACE_FIREWALL_RULES=y
AUTO_CLEANUP_RESTART_FIREWALL=y
AUTO_CLEANUP_REPLACE_MYSQL_CONFIG=y
4/ Téléchargement et exécution du script iredmail depuis le lien suivant :
https://github.com/iredmail/iRedMail/archive/refs/tags/1.7.2.tar.gz
5/ Si backup les copier dans le répertoire /var/vmail
6/ envoi des certificats dans /etc/ssl/certs/iredmail.crt et /etc/ssl/private/iredmail.key
7/ vérification des records dns
8/ Vérification que le serveur fonctionne et envoi du mail pour prévenir qu'il est opérationnel