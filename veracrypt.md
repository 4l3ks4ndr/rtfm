# VERACRYPT



Veracrypt est un outil opensource développé par la NSA permettant de sécuriser des données hautement confidentielles.

Il permet de créer et monter des volumes ou des systèmes d'exploitations cachés. Pour déverrouiller une partition ou un volume il faudra selon la configuration faite en amont avoir le mot de passe chiffré avec des algorithmes très puissant et/ou un fichier supplémentaire.

Cet outil est gratuit et est disponible sur Linux, Windows ou Mac, voici l'adresse du site :

https://veracrypt.fr/en/Home.html

Aller dans la section Downloads pour pouvoir télécharger l'outil en fonction de votre système d'exploitation.

Pour les utilisateurs Linux, il est possible de ne pas avoir d'interface graphique et tout gérer en CLI, il faudra dans ce cas prendre la version console au format .deb ou .rpm.



## Utilisation de Veracrypt en CLI

### Création d'un volume:

```
veracrypt --create /root/<my_container> --volume-type=normal --encryption=AES --hash=SHA-512 --filesystem=ext4 --size=100M
```

La commande ci-dessus prend plusieurs arguments :

- --create permet de spécifier le chemin d'accès du container, donner un nom au choix pour celui-ci. Pour information ce qui est appelé container est tout simplement le fichier qui servira de volume. Dans  ce dernier se trouvera toutes les informations sur le volume à monter avec les options de sécurité etc
- --volume-type=normal permet de spécifier que c'est pas un volume de type hidden à créer.
- --encryption et --hash permettent de choisir le type d'encryptage et le hash associé à utiliser pour le chiffrement de ce volume
- --filesystem spécifie quel type de système de fichier à utiliser, comme je suis sur linux je préfère l'ext4 qui est majoritairement utilisé de par sa conception
- --size permet de donner une taille de stockage à allouer à ce container. Taille à évidemment adapter selon la taille des données à stocker dedans.

Lors de la création du container il sera demandé le mot de passe qui servira à déverrouiller le volume (bien entendu ne pas mettre de mot de passe comme Azerty123* ou similaire). Je recommande fortement d'utiliser un générateur de mot de passe comme celui qui est fourni par bitwarden.

Ensuite il sera demandé le pim est un paramètre de sécurité supplémentaire permettant d'ajouter un nombre d'itérations qui sera utilisées ensuite pour dériver la clé de chiffrement à partir du mot de passe. Plus celui-ci sera élevé plus le temps nécessaire pour monter le volume sera important.Ensuite il sera demandé (optionnel) un fichier supplémentaire pour monter le volume. C'est souvent comme on voit dans les films où il faut brancher une clé usb dans laquelle se trouve un fichier qui permet de déverrouiller l'accès. C'est une mesure supplémentaire qui peut renforcer d'avantage l'accès à ce volume si le mot de passe venait à se faire reverse (par un gros "matheu"). Pour ça qu'il est important d'utiliser des mot de passe très très long (>= 64) avec caractères spéciaux.

<u>A noter aussi que plus la taille du volume sera élevé plus le temps de montage le sera en fonction également d'autres critères comme le pim, le hash etc</u>

### Monter un volume

Pour monter un volume il faudra avoir accès au fichier container de base et si nécessaire la clé supplémentaire ensuite exécuter la commande suivante:

```
veracrypt --mount /path/<my_container> --pim=0 (ou plus selon votre choix en amont)
```

Il sera demandé les informations suivantes :

- Le point de montage: Par défaut veracrypt va monter le volume dans /media/veracrypt<number>. Il est possible de spécifier un point de montage différent, pour ça il faudra lui indiquer par exemple "/home/user/my-mountpoint". Inutile de préciser que le répertoire doit exister avant.
- Le mot de passe choisi lors de la création du volume
- Le fichier supplémentaire si spécifié lors de la création du volume

Si toutes les informations correspondent, le volume sera monté directement. En exécutant la commande "lsblk", on devrait avoir un résultat similaire :

```
loop39             7:39   0   100M  0 loop  
└─veracrypt1     252:2    0  99,8M  0 dm    /media/veracrypt1
```

Ensuite y placer les fichiers que l'on souhaite cacher dedans.

Si l'une des informations est incorrecte, le "questionnaire" se relancera.

### Démonter le volume

```
veracrypt -d # Attention cette commande va démonter tout les volumes
```

Pour démonter un volume spécifique, il faudra exécuter la commande suivante :

```
veracrypt -d /meta/veracrypt<number>
```

