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

# Cas concret : Pourquoi Vinted ou Leboncoin en ont besoin ? 🛒

Imaginez un utilisateur qui tape **"chemiseee"** (avec une faute) sur un site e-commerce.

* **Avec du SQL classique :** 0 résultat. L'utilisateur part, l'entreprise perd une vente.
* **Avec Solr :** L'algorithme analyse phonétiquement et textuellement, comprend l'intention ("chemise"), et affiche les bons produits. 

C'est ce qu'on appelle la rentabilité par la recherche !

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

Solr ne remplace pas votre base de données principale (MySQL, PostgreSQL...). Il travaille avec !

1. **Ajout d'un produit :** Le site web sauvegarde le produit dans la BDD SQL (pour la sécurité) ET l'envoie à Solr (pour l'indexer).
2. **Recherche client :** Quand un client utilise la barre de recherche, le site n'interroge plus la BDD SQL, il demande directement à Solr.
3. **Réponse :** Solr renvoie les ID des produits pertinents en quelques millisecondes.

---

# 📚 Le lexique de survie (Avant le TP)

Oubliez le vocabulaire SQL pour les 20 prochaines minutes !

| Base de données (SQL) | Apache Solr |
| :--- | :--- |
| Ligne (Enregistrement) | **Document** (Ex: un film) |
| Colonne | **Champ / Field** (Ex: titre, réalisateur) |
| Base de données complète | **Core / Collection** |

---

# À vous de jouer ! 💻

Objectif du TP (20 min) :
1. Démarrer notre instance Solr
2. Créer notre premier "Core"
3. Injecter un catalogue de films au format JSON
4. Faire notre première recherche via l'interface d'administration

**Préparez vos terminaux !**