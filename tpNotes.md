🛠️ Étape 1 : Démarrer l'instance et le Core
L'objectif : Lancer le moteur avec un core nommé videoclub.

Ce que tu leur dis de taper :
docker run -d -p 8983:8983 --name solr_tp solr:9 solr-precreate videoclub

Vérification : Allez sur http://localhost:8983. Dans le menu déroulant "Core Selector" (à gauche), choisissez videoclub.

🛠️ Étape 2 : Injecter les données
L'objectif : Remplir Solr avec notre JSON.

La petite ruse : Demande-leur d'ouvrir le fichier films.json sur leur PC et d'y ajouter 2 de leurs films préférés en respectant la syntaxe.

Dans le menu de videoclub (sur le navigateur), cliquez sur Documents.

Dans "Document Type", sélectionnez JSON.

Copiez tout le contenu du fichier modifié, et collez-le dans le grand encadré blanc.

Cliquez sur Submit Document. (Si certains ont une erreur, c'est qu'ils ont mal formaté leur JSON, aide-les à débugger !)

🕵️ Étape 3 : Investigation (Les Missions)
C'est là qu'ils cherchent. Laisse-les tester dans l'onglet Query et demande-leur de lever la main quand ils ont la solution pour que tu vérifies.

Mission 1 (Échauffement) : Trouver les films des années 90.

Ce qu'ils doivent trouver : Dans la case fq (Filter Query), taper annee:[1990 TO 1999].

Mission 2 : Trouver les films d'Action, avec le mot-clé en gras.

Ce qu'ils doivent trouver : fq=genre:Action, puis descendre cocher la case hl et taper la colonne visée dans hl.fl.

Mission 3 : Le client tape "dinosore". Trouvez Jurassic Park.

Ce qu'ils doivent trouver : Dans la case q, utiliser le Fuzzy Search en tapant : description:dinosore~2

🚀 Étape 4 : Les Bonus (Pour ceux qui ont fini tôt)
Bonus "Usine de découpage" : Fais-les aller dans l'onglet Analysis. Demande-leur de taper une phrase dans "Field Value", de choisir text_general et de cliquer sur Analyze pour voir l'Index Inversé se créer visuellement.

Bonus "Hacker" : Retourner dans Documents, choisir JSON, et envoyer la commande {"delete": {"id":"1"}} pour supprimer un film de la base.