# Projet 8 Déployez un modéle dans le Cloud 

## Description du projet 
    Vous êtes donc chargé de développer dans un environnement Big Data une première chaîne de traitement des
    données qui comprendra le preprocessing et une étape de réduction de dimension.

    Il n’est pas nécessaire d’entraîner un modèle pour le moment.

    -Mise en place d'une infrastructure de prétraitement de données dans le cloud, en vue
    d'une augmentation du volume des données
    -Configuration d'une instance AWS EC2 (OS Ubuntu Server 18.04)
    -Réalisation de scripts pyspark et exécution dans le cloud
    -Lecture et enregistrement de données sur Amazon S3
    -Appel à la librairie VGG16  pour utiliser le  Transfer Learning en vue de trouver les descripteurs de nos images 

Source des données : https://www.kaggle.com/moltean/fruits

## Compétences acquises


    -Utiliser les outils du cloud pour manipuler des données dans un environnement Big Data
    -Paralléliser des opérations de calcul avec Pyspark
    -Identifier les outils du cloud permettant de mettre en place un environnement Big Data


## Contraintes


Vous devrez tenir compte dans vos développements du fait que le volume de données va augmenter très rapidement après la livraison de ce projet. Vous développerez donc des scripts en Pyspark et utiliserez par exemple le cloud AWS pour profiter d’une architecture Big Data (EC2, S3, IAM), basée sur un serveur EC2 Linux. La mise en œuvre d’une architecture Big Data sous (par exemple) AWS peut nécessiter une configuration serveur plus puissante que celle proposée gratuitement (EC2 = t2.micro, 1 Go RAM, 8 Go disque serveur).

## Livrables 

    -Un notebook sur le cloud contenant les scripts en Pyspark exécutables (le preprocessing et une étape de réduction de dimension).
    -Les images du jeu de données initial ainsi que la sortie de la réduction de dimension (une matrice écrite sur un fichier CSV ou autre) disponible dans un espace de stockage sur le cloud.
    -Un support de présentation pour la soutenance, présentant :
        les différentes briques d'architecture choisies sur le cloud ;
        leur rôle dans l’architecture Big Data ;
        les étapes de la chaîne de traitement.

## Création de mon instance AWS et différents services d'AWS

Pour la preuve-de-concept (et à cause des ressources limitées), j'ai sélectionné arbitrairement 5 catégories de fruits dans le jeux de données. Je les ai transféré  sur mon serveur S3 grâce au SDK de python Boto3. J'ai utilisé une instance EC2 payant (t2.medium, 4 Go RAM, 30 Go disque serveur, OS = Linux 18.x, 64-bit). L'étape d'installation de l'environement PySpark est très compliquée. Car certaines versions de logiciels ne sont pas compatibles  entre eux.

## Récapitulation des différents étapes d'installation :

    -Sélectionner la bonne instance (EC2 ou SageMaker), avec la bonne mémoire et suffisamment de stockage,
    -créer un groupe de sécurité (configurer ssh particulièrement),
    -Installer PuttY (convertir clé .pem en .ppk, setup la connextion ssh, attention au username = "ec2-user" ou "ubuntu"),
    -Créer un bucket S3 (et setup les autorisations/droits en .json, grâce au générateur de stratégie),
    -Créer un AMI user et bien garder les clés access et private,
    -Dans le serveur EC2, tout en ligne de commande :
        *télécharger puis bash anaconda, 
        https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh vraiment faire attention aux versions,
        *installer java sudo apt install openjdk-8-jre-headless, vraiment faire attention aux versions, prenez la version java8.
        *Installer scala
        *Télécharger puis décompresser spark-3.0.3-hadoop-2.7, https://spark.apache.org/downloads.html
         vraiment faire attention aux versions,gérer les credentials jupyter,
        envoyer tout ça dans le path,
        *Gérer les crédentials du S3 via les clés données par le AMI user (fichier .aws), ou aws configure (le cas échéant installer awscli : sudo apt install awscli ou 
        pip install awscli
        *Ajouter tous les packages nécessaires (entre-autre tensorflow, py4j),
        *Ajuster le swap RAM de l'instance pour éviter les memory crashes, utilisation de 30 Go.
