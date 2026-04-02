Slide 1 : Le titre (1 min)
Ton but : Accrocher le public.

Ce que tu dis : "Salut à tous. Aujourd'hui on va parler de la recherche textuelle. Quand on va sur Netflix, Amazon ou Spotify, on tape un mot et on a les résultats instantanément. Comment font ces géants pour chercher parmi des millions de données en une fraction de seconde ? Ils n'utilisent pas des bases de données classiques. Ils utilisent des moteurs de recherche dédiés, comme Apache Solr. En 30 minutes, on va voir la théorie sous le capot, puis on va faire notre propre moteur de recherche."

Slide 2 : Le problème avec SQL (2 min)
Ton but : Prouver que SQL n'est pas fait pour la recherche de texte.

Ce que tu dis : "Pourquoi on n'utilise pas juste du SQL avec un LIKE '%mot%' ? Pour trois raisons. D'abord, la vitesse. Le SQL va lire les lignes une par une. Ensuite, la rigidité. Si je cherche 'basket' et que la base contient 'chaussure', le SQL ne trouvera rien, car il ne comprend pas les synonymes. Et enfin, le SQL ne sait pas ce qu'est la pertinence : il ne sait pas mettre le résultat le plus intéressant en premier."

Slide 3 : Qu'est-ce que Solr ? (2 min)
Ton but : Présenter l'outil techniquement.

Ce que tu dis : "La solution, c'est Apache Solr. C'est un logiciel open-source, de la fondation Apache, codé en Java. Concrètement, c'est un serveur web. On communique avec lui en lui envoyant des requêtes HTTP avec du JSON. Beaucoup de très grosses boîtes l'utilisent car il est conçu pour le Big Data et peut être distribué sur plusieurs serveurs sans crasher."

Slide 4 : Solr vs Lucene (2 min)
Ton but : Faire briller ta culture tech.

Ce que tu dis : "Petite nuance très importante : Solr n'est pas le moteur algorithmique de base. Le vrai moteur de recherche sous le capot s'appelle Apache Lucene. Mais Lucene, c'est juste une librairie de code, c'est difficile à utiliser seul. L'analogie est simple : Lucene, c'est le moteur de la voiture. Et Solr, c'est la carrosserie, le volant et le tableau de bord qui vous permettent de conduire ce moteur facilement via le réseau."

Slide 5 : Le secret absolu : L'Index Inversé (3 min) ⚠️ Moment fort
Ton but : Expliquer la magie de la recherche instantanée.

Ce que tu dis : (Pose la question au public) "Comment faire pour trouver le mot 'citron' dans un livre de 500 pages sans tout lire ? On va regarder l'index à la fin du livre ! Solr fait exactement pareil. Quand on lui envoie un document, il le découpe en mots, et crée un dictionnaire inversé. Donc, quand l'utilisateur tape 'souris', Solr ne parcourt pas les documents. Il va dans son dictionnaire, trouve 'souris', et voit instantanément que ça correspond aux documents 1 et 2. La réponse prend 2 millisecondes."

Slide 6 : L'usine de traitement : Les Analyzers (3 min)
Ton but : Expliquer la préparation des données.

Ce que tu dis : "Je vous ai parlé de l'Index Inversé, mais comment Solr le construit-il ? Il utilise un Analyzer. C'est une usine en deux étapes. D'abord, le Tokenizer vient découper la phrase en mots. Ensuite, ces mots passent dans des Filtres. Les filtres mettent tout en minuscules, suppriment les petits mots de liaison comme 'les' ou 'et', et réduisent les mots à leur racine ('chats' devient 'chat'). C'est ce résultat ultra-nettoyé qui est sauvegardé dans l'Index."

Slide 7 : Le Scoring et l'algorithme BM25 (3 min)
Ton but : Expliquer la notion de pertinence.

Ce que tu dis : "Maintenant que Solr a trouvé les documents, comment décide-il de l'ordre d'affichage ? Il utilise un algorithme appelé BM25. Il calcule un score basé sur deux choses : le TF (la fréquence du mot dans le document) et l'IDF (la rareté du mot dans la base entière). Trouver le mot 'ordinateur' rapporte beaucoup de points car il est rare. Trouver le mot 'le' ne rapporte rien car il est partout."

Slide 8 : Le cas Vinted / Leboncoin (2 min)
Ton but : Rendre ça très concret.

Ce que tu dis : "Pensez à Vinted ou Leboncoin. Un utilisateur cherche une chemise, mais il tape 'chemiseee' avec une faute. Une BDD classique va dire 'Aucun résultat' et l'entreprise perd une vente. Solr va analyser le mot, calculer la proximité phonétique, corriger l'erreur, et afficher les chemises. La recherche, c'est le nerf de la guerre dans l'e-commerce."

Slide 9 : Les super-pouvoirs (2 min)
Ton but : Lister les fonctionnalités clés.

Ce que tu dis : "En plus de corriger les fautes (Fuzzy Search), Solr permet de faire du surlignage : il renvoie le texte en mettant le mot recherché en gras. Mais surtout, il gère les Facettes. Ce sont ces petits filtres dynamiques sur le côté d'Amazon qui vous disent instantanément 'Il y a 3 produits rouges, 5 noirs'. Ça, c'est natif dans Solr."

Slide 10 : L'Architecture (2 min)
Ton but : Placer Solr dans une architecture web.

Ce que tu dis : "Attention, on ne remplace pas MySQL ou PostgreSQL par Solr. Ils travaillent en équipe. Votre BDD classique garde la source de vérité. Quand un produit est créé, le site l'enregistre dans MySQL, et envoie une copie à Solr. Quand un client tape une recherche, le site n'interroge QUE Solr."

Slide 11 : Sous le capot d'un Core (3 min)
Ton but : Préparer le TP sur un aspect technique.

Ce que tu dis : "Tout à l'heure, on va créer un 'Core'. Ce n'est pas juste une base de données. C'est l'assemblage de votre index physique sur le disque, et de fichiers de configuration stricts en XML. Le fichier schema.xml est crucial : c'est là qu'on définit si un champ doit passer dans l'Analyzer ou non. Le 'titre' doit être découpé, mais un 'code barre' doit rester intact."

Slide 12 : L'utilisation dans le code (2 min)
Ton but : Rassurer les développeurs.

Ce que tu dis : "Pour l'utiliser, pas besoin d'être un expert en infrastructure. Solr est une API REST. Si votre entreprise fait du Java, vous utiliserez la librairie SolrJ qui est la norme absolue. Mais on peut utiliser Solr avec n'importe quel langage, de Python (PySolr) à PHP, ou même avec un simple appel JavaScript depuis votre site web."

Slide 13 : Le grand match (3 min)
Ton but : Ouvrir sur le marché de l'emploi.

Ce que tu dis : "Pour terminer sur la théorie, parlons de la concurrence. Elasticsearch est le leader actuel. Le paradoxe, c'est qu'il utilise le même moteur que Solr : Lucene ! Mais Elasticsearch s'est spécialisé dans l'analyse de logs serveurs. Solr, lui, est resté le roi de la recherche textuelle pure et complexe. Enfin, des outils comme Meilisearch émergent pour les petits projets car ils ne demandent aucune configuration."

Slide 14 : Le Lexique (1 min)
Ton but : Préparer le vocabulaire pour le TP.

Ce que tu dis : "Juste avant de mettre les mains dans le cambouis, il faut parler la langue de Solr. On efface le SQL. Une ligne devient un Document. Une colonne devient un Champ. Et la base de données globale s'appelle un Core."