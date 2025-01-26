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