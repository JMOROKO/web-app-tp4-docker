<h1>1. Création du projet et du fichier docker file</h1>
<p><b>Création du projet HTML</b></p>
<img src="assets/00-fichier-html.png" alt="fichier html">
<P><b>Création du fichier dockerFile</b></P>
<img src="assets/01-fichier-docker-file.png" alt="docker file">
<h1>2. Installation de jenkins</h1>
<p><b>Téléchargement de jenkins et installation basique</b></p>
<img src="assets/02-telecharger-jenkins.png" alt="">
<p><b>Configuration</b></p>
<p>Aller dans les services de windows et rechercher le service jenkins</p>
<img src="assets/03-service-jenkins.png" alt=""> 
<p>Double cliquer sur jenkins, aller dans l'onglet connexion, cliquer sur ce compte, faire parcourir, ajouter le nom d'utilisateur de session de votre machine.
Mette le mot de passe d'ouverture de session de la machine et la confirmer.</p>
<img src="assets/04-jenkins-services.png" alt="">
<p><b>Configuration de docker tcp dans jenkins</b></p>
<img src="assets/06-etape-1.png" alt=""> <br>
<img src="assets/07-etape-2.png" alt=""> 
<p><b>Configuration de docker desktop</b></p>
Il faut utiliser le même compte que vous compter utiliser dans docker hub
<img src="assets/08-docker-desktop-3.png" alt="">
<h1>3. Installation des plugins</h1>
<img src="assets/05-docker-plugins.png" alt="Docker plugins">
<h1>4. Création du job jenkins</h1>
<img src="assets/9-docker-jobetape1.png" alt=""> <br>
<img src="assets/09-docker-job-etape2.png" alt=""> <br>
<h1>5. Configuration du job jenkins</h1>
<img src="assets/9-jenkins-job-config-etape-1.png" alt=""> <br>
<img src="assets/9-jenkins-job-config-etape-2.png" alt=""> <br>
<img src="assets/9-jenkins-job-config-etape-3.png" alt=""> <br>
<img src="assets/9-jenkins-job-config-etape-4.png" alt=""> 
<h1>6. Test de la configuration</h1>
<p><b>Lancer le job</b></p>
<img src="assets/10-lancer-job.png" alt="">
<p><b>En cas d'erreur</b></p>
<p>Cliquer sur le dernier numéro d'exécution du job</p>
<img src="assets/11-cas-erreur.png" alt="">
<p>Ensuite cliquer sur sortie console</p>
<img src="assets/11-sortie-console.png" alt="">
<p>Aller jusqu'en bas du fichier pour voir le message d'erreur</p>
<img src="assets/11-erreur.png" alt="">
<h1>7. Mise à jour du projet github</h1>
<p>Un push dans github enclanche automatiquement un build dans jenkins</p>
<img src="assets/12-build-automatique-jenkins.png" alt=""> <br>
<img src="assets/12-docker-hub.png" alt="">
<h1>8. Supprimer le job dans jenkins</h1>
<img src="assets/13-supprimer.png" alt="">
<br><br><br>
<h1>9. Partie 2 CI/CD => Intégration continue / Déploiement continue</h1>
<p>Il faut reprendre toutes les étapes précédente et ajouter un script shell</p>
<img src="assets/14-build-shell.png" alt=""> <br>
<img src="assets/15-commande-shell.png" alt="">
<p><b>Lancement automatique du job</b></p>
<img src="assets/16-lancement-auto-job.png" alt=""> <br>
<img src="assets/17-docker-image.png" alt=""> <br>
<h1>10. Créer un job 2 de type pipeline</h1>
<img src="assets/18-job2pipeline.png" alt=""> 
<p><b>Configuration</b></p>
<img src="assets/18-config-job2.png" alt="">
<pre>
pipeline {
    environment {
        registry = "jmoroko/tp4cicd"
        registryCredential = 'dockerhub'
        dockerImage = ''
        DOCKER_HOST = 'npipe:////./pipe/docker_engine'
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', 
                    credentialsId: 'jmoroko', 
                    url: 'https://github.com/JMOROKO/web-app-tp4-docker.git'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
</pre>
<h1>10. Créer un job 3 de type pipeline</h1>
<img src="assets/18-job3pipeline.png" alt="">
<p><b>Configuration contenu du fichier : Jenkinsfile </b></p>
<pre>
pipeline {
    environment {
        registry = "jmoroko/tp4cicd"
        registryCredential = 'dockerhub' // ID des credentials Docker Hub dans Jenkins
        dockerImage = ''
        DOCKER_HOST = 'npipe:////./pipe/docker_engine' // Pour Windows
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    credentialsId: 'jmoroko',
                    url: 'https://github.com/JMOROKO/web-app-tp4-docker.git'
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Testing Docker Image') {
            steps {
                script {
                    echo "Running tests on Docker image..."
                    // Remplacez par des tests réels si nécessaire
                    echo "Tests passed"
                }
            }
        }
        stage('Publishing Docker Image') {
            steps {
                script {
                    echo "Publishing Docker image to Docker Hub..."
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push() // Push avec le numéro de build
                        dockerImage.push('latest') // Push avec le tag 'latest'
                    }
                }
            }
        }
        stage('Deploying Docker Image') {
            steps {
                script {
                    echo "Deploying Docker image..."
                    // Commande pour lancer le conteneur Docker sur Windows
                    bat "docker run -d -p 8080:80 --name app-${BUILD_NUMBER} ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}

</pre>
<h1>11. Créer un job 3 et coupler le script Jenkinsfile au job 3</h1>
<img src="assets/19-script-path.png" alt=""> <br>
<img src="assets/20-config-jenkins-etape1.png" alt=""> <br>
<!--<img src="assets/20-config-jenkins-etape2.png" alt=""> <br>-->
<p><b>Lancer le build</b></p>
<img src="assets/111-lancer-build.png" alt="">
<h1>12. Resultat de la mise en place du pipeline</h1>
<p><b>Installation du plugins view stage</b></p>
<img src="assets/21-install-pipeline-stage-view-plugins.png" alt="">
<img src="assets/22-mise-en-place-pipeline.png" alt="">
<h2>13. Resultat du déploiement dans docker desktop</h2>
<img src="assets/22-mise-en-place-docker.png" alt=""> 
<h2>14. Affichage de l'application déployé sur mon navigateur</h2>
<img src="assets/23-app.png" alt="">