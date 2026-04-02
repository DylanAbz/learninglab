---
marp: false
theme: default
class: invert
paginate: true
style: |
  h1 { color: #D9411E; }
---

# Apache Solr 🔎
## Trouvez une aiguille dans une botte de paille en quelques millisecondes

---

# Le problème avec nos BDD classiques

❌ **La méthode SQL :**
`SELECT * FROM articles WHERE description LIKE '%chaussure%'`

* **Lent :** Parcourt chaque ligne de la table une par une.
* **Rigide :** Ne gère pas les synonymes (chaussure vs basket).
* **Intolérant :** Ne pardonne pas les fautes de frappe ("chausure").
* **Pas de tri :** Aucune notion de "pertinence" (le meilleur résultat en premier).

---

# Qu'est-ce qu'Apache Solr ?

* 🌐 **Open-Source** (Fondation Apache)
* ⚡ **Ultra-rapide** & Taillé pour le Big Data
* ☕ **Basé sur Java**
* 🏢 **Utilisé par les géants :** eBay, Disney, Adobe...

C'est un moteur de recherche complet qu'on interroge via de simples requêtes HTTP (API).

---

# Solr vs Lucene : Le moteur et la voiture

🚗 **Apache Solr (La carrosserie)**
* L'interface serveur et le tableau de bord
* Les API (HTTP, JSON)
* L'administration et la sécurité
* La gestion distribuée (SolrCloud)

⚙️ **Apache Lucene (Le moteur)**
* La librairie interne qui fait le calcul pur
* L'algorithme de recherche textuelle

---

# Le secret absolu : L'Index Inversé 🧠

*Question : Comment trouver le mot "citron" dans un livre de 500 pages sans tout lire ?*

**Documents initiaux :**
* Doc 1 : "Le chat mange la souris"
* Doc 2 : "La souris mange le fromage"

**L'index inversé de Solr (comme à la fin d'un livre) :**
* chat ➡️ [Doc 1]
* mange ➡️ [Doc 1, Doc 2]
* souris ➡️ [Doc 1, Doc 2]
* fromage ➡️ [Doc 2]

---

# L'usine de traitement : Les Analyzers ⚙️

Comment Solr passe-t-il d'une phrase à un Index Inversé propre ? Grâce au pipeline d'analyse !

**Texte brut :** *"Les petits chats mangent !"*

1. ✂️ **Le Tokenizer (Découpeur) :**
   * Sépare les mots : `[Les] [petits] [chats] [mangent]`
2. 🚰 **Les Filters (Filtres) :**
   * *Minuscules :* `[les] [petits] [chats] [mangent]`
   * *Stop-words (Mots vides) :* `[petits] [chats] [mangent]`
   * *Stemming (Racines) :* `[petit] [chat] [mang]`

**Résultat :** Ce sont ces racines qui sont stockées dans l'index !

---

# L'algorithme de Pertinence : Le cerveau de Solr 🧠

Pourquoi le film A sort en 1er et le film B en 2ème ? Grâce à l'algorithme **BM25** (l'évolution du célèbre TF-IDF). 

Il repose sur deux piliers :
* 📈 **TF (Term Frequency) :** Plus le mot cherché est répété dans le document, plus le score monte.
* 💎 **IDF (Inverse Document Frequency) :** Plus le mot cherché est *rare* dans la base de données globale, plus il a de la valeur. 

*Exemple :* Trouver le mot "ordinateur" rapporte beaucoup de points, trouver le mot "le" ne rapporte rien.

---

# Cas concret : Pourquoi Vinted ou Leboncoin en ont besoin ? 🛒

Imaginez un utilisateur qui tape **"chemiseee"** (avec une faute) sur un site e-commerce.

* **Avec du SQL classique :** 0 résultat. L'utilisateur part, l'entreprise perd une vente.
* **Avec Solr :** L'algorithme analyse phonétiquement et textuellement, comprend l'intention ("chemise"), et affiche les bons produits. 

C'est la rentabilité par la recherche !

---

# Les super-pouvoirs de Solr 🦸‍♂️

1. 🔍 **Fuzzy Search (Recherche floue) :**
   * Tolère les fautes de frappe et gère les synonymes.
2. 🖍️ **Highlighting (Surlignage) :**
   * Renvoie un extrait du texte avec le mot-clé mis en évidence.
3. 📊 **Facettes (Filtres dynamiques) :**
   * "3 téléphones Samsung, 5 téléphones Apple" (généré à la volée sur le côté de la page).

---

# L'Architecture : Où place-t-on Solr ? 🏗️

Solr ne remplace pas la base de données (MySQL, PostgreSQL...). 

1. **Ajout d'un produit :** Le site web sauvegarde le produit dans la BDD SQL (pour la sécurité) ET l'envoie à Solr (pour l'indexer).
2. **Recherche client :** Quand un client utilise la barre de recherche, le site n'interroge plus la BDD SQL, il demande directement à Solr.
3. **Réponse :** Solr renvoie les ID des produits pertinents en quelques millisecondes.

---

# Sous le capot : Qu'est-ce qu'un "Core" ? ⚙️

On l'utilisera dans le TP. Un **Core** n'est pas juste une boîte vide. C'est une instance de recherche indépendante qui contient :

1. **L'index physique :** Les données elles-mêmes (le dictionnaire inversé sur le disque dur).
2. **Le fichier `schema.xml` :** Le plan de construction. Il dit à Solr : *"Le champ 'titre' est du texte, le champ 'annee' est un entier"*.
3. **Le fichier `solrconfig.xml` :** Le cerveau technique (gestion du cache, de la mémoire RAM, et des plugins).

---

# Comment l'utiliser dans notre code ? 💻

Pas besoin d'être un expert en infrastructure pour parler à Solr. C'est une simple **API REST**. On lui envoie une requête HTTP, il répond en JSON.

* ☕ **En Java :** On utilise la librairie officielle **SolrJ** (très utilisée en milieu bancaire/assurance).
* 🐍 **En Python :** On utilise **PySolr**.
* 🐘 **En PHP :** On utilise **Solarium**.
* 🌐 **En JavaScript :** Un simple `fetch()` ou `axios.get()` suffit pour récupérer les résultats et les afficher sur un site web.

---

# Le grand match : Solr face à la concurrence 🥊

Pourquoi choisir Solr en 2026 ?

* 🟠 **Elasticsearch :** Le faux jumeau (basé sur Lucene aussi). Il domine le marché, mais il est surtout utilisé pour analyser des montagnes de logs serveurs (la stack ELK).
* 🔴 **Apache Solr :** Le vétéran. Reste le roi incontesté de la **recherche textuelle complexe** et de l'analyse de gros documents (PDF, Word).
* 🟣 **Meilisearch / Typesense :** Les challengers modernes. Plus légers, ils gèrent les fautes de frappe sans aucune configuration. Parfaits pour les petits sites e-commerce.

---

# 📚 Le lexique (Avant le TP)


| Base de données (SQL) | Apache Solr |
| :--- | :--- |
| Ligne (Enregistrement) | **Document** (Ex: un film) |
| Colonne | **Champ / Field** (Ex: titre, réalisateur) |
| Base de données complète | **Core / Collection** |

---

# À vous de jouer ! 💻 (TP - 20 min)

**Pour créer son propre moteur :**

1. **Démarrer le serveur** Solr via Docker.
2. **Injecter le catalogue** de films complet (JSON).
3. **Visiter l'usine (Analysis) :** Observer le découpage de vos mots en direct !
4. **Recherches Avancées :**
   * Trouver un film en faisant des fautes de frappe (*Fuzzy Search*).
   * Filtrer par réalisateur et année (*Facettes*).
   * Activer le surlignage des mots-clés (*Highlighting*).

* Bonus pour les rapides : supprimer un film via l'API