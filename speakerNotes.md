Slide 1 : Le titre (1 min)

    Ton but : Accrocher le public.

    Ce que tu dis : "Salut à tous. Aujourd'hui on va parler de la recherche textuelle. Quand on va sur Netflix, Amazon ou Spotify, on tape un mot et on a les résultats instantanément. Comment font ces géants pour chercher parmi des millions de données en une fraction de seconde ? Ils n'utilisent pas des bases de données classiques. Ils utilisent des moteurs de recherche dédiés, comme Apache Solr. En 20 minutes, on va voir la théorie, puis on va installer Solr et faire notre propre moteur."

Slide 2 : Le problème avec SQL (2 min 30)

    Ton but : Prouver que SQL n'est pas fait pour la recherche de texte.

    Ce que tu dis : "Pourquoi on n'utilise pas juste du SQL avec un LIKE '%mot%' ? Pour trois raisons. D'abord, la vitesse. Le SQL va lire les lignes une par une. Ensuite, la rigidité. Si je cherche 'basket' et que la base contient 'chaussure', le SQL ne trouvera rien, car il ne comprend pas les synonymes. Et enfin, le SQL ne sait pas ce qu'est la pertinence : il ne sait pas mettre le résultat le plus intéressant en premier."

Slide 3 : Qu'est-ce que Solr ? (2 min)

    Ton but : Présenter l'outil techniquement.

    Ce que tu dis : "La solution, c'est Apache Solr. C'est un logiciel open-source, de la fondation Apache, codé en Java. Concrètement, c'est un serveur web. On communique avec lui en lui envoyant des requêtes HTTP avec du JSON. Beaucoup de très grosses boîtes l'utilisent car il est conçu pour le Big Data et peut être distribué sur plusieurs serveurs sans crasher."

Slide 4 : Solr vs Lucene (2 min)

    Ton but : Faire briller ta culture tech (les profs adorent cette précision).

    Ce que tu dis : "Petite nuance très importante : Solr n'est pas le moteur algorithmique de base. Le vrai moteur de recherche sous le capot s'appelle Apache Lucene. Mais Lucene, c'est juste une librairie de code, c'est difficile à utiliser seul. L'analogie est simple : Lucene, c'est le moteur de la voiture. Et Solr, c'est la carrosserie, le volant et le tableau de bord qui vous permettent de conduire ce moteur facilement via le réseau."

Slide 5 : Le secret absolu : L'Index Inversé (4 min) ⚠️ Moment fort

    Ton but : Expliquer la magie de la recherche instantanée.

    Ce que tu dis : (Pose la question au public) "Comment faire pour trouver le mot 'citron' dans un livre de 500 pages sans tout lire ? On va regarder l'index à la fin du livre ! Solr fait exactement pareil. Quand on lui envoie un document, il le découpe en mots, enlève les petits mots inutiles comme 'le' ou 'la', et crée un dictionnaire inversé. Donc, quand l'utilisateur tape 'souris', Solr ne parcourt pas les documents. Il va dans son dictionnaire, trouve 'souris', et voit instantanément que ça correspond aux documents 1 et 2. La réponse prend 2 millisecondes."

Slide 6 : Le cas Vinted / Leboncoin (2 min)

    Ton but : Rendre ça très concret pour les étudiants.

    Ce que tu dis : "Si ça vous paraît abstrait, pensez à Vinted ou Leboncoin. Un utilisateur cherche une chemise, mais avec ses gros doigts sur son smartphone, il tape 'chemiseee'. Une BDD classique va dire 'Aucun résultat'. L'utilisateur ferme l'appli, l'entreprise perd de l'argent. Solr va analyser le mot, calculer la proximité phonétique et orthographique, corriger l'erreur, et afficher les chemises. La recherche, c'est le nerf de la guerre dans l'e-commerce."

Slide 7 : Les super-pouvoirs (2 min)

    Ton but : Lister les fonctionnalités clés.

    Ce que tu dis : "En plus de corriger les fautes (Fuzzy Search), Solr permet de faire du surlignage : il renvoie le texte en mettant le mot recherché en gras pour qu'on comprenne pourquoi on a trouvé ce document. Mais surtout, il gère les Facettes."

    (Montre ou explique l'image de la facette) "Les facettes, ce sont ces petits filtres dynamiques sur le côté d'Amazon qui vous disent instantanément 'Il y a 3 produits rouges, 5 noirs'. Ça, c'est natif dans Solr."

Slide 8 : L'Architecture (3 min)

    Ton but : Bien placer Solr dans une architecture web classique.

    Ce que tu dis : "Attention, on ne remplace pas MySQL ou PostgreSQL par Solr. Ils travaillent en équipe. Votre BDD classique garde la source de vérité (les mots de passe, les vraies fiches produits, les factures). Quand un produit est créé, le site l'enregistre dans MySQL, et envoie une copie du titre et de la description à Solr. Quand un client tape une recherche, le site n'interroge QUE Solr."

Slide 9 : Le Lexique (1 min)

    Ton but : Préparer le vocabulaire pour le TP.

    Ce que tu dis : "Juste avant de mettre les mains dans le cambouis, il faut parler la langue de Solr. On efface le SQL de notre tête. Une ligne devient un Document. Une colonne devient un Champ (Field). Et la base de données globale s'appelle un Core."

Slide 10 : À vous de jouer ! (30 secondes)

    Ton but : Transition dynamique.

    Ce que tu dis : "Fini pour la théorie ! Ouvrez vos terminaux. On va utiliser un fichier JSON contenant des films, on va lancer un serveur Solr, créer notre Core et faire notre première recherche avec des fautes de frappe. C'est parti."