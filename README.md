🧠 NLP – Extraction de variables sociodémographiques
Extraction automatique de l’âge et de la ville de naissance à partir de textes non structurés

Ce projet Jupyter Notebook présente une démarche complète d’extraction d’informations sociodémographiques depuis des données textuelles. Il a été conçu comme exercice personnel pour démontrer une capacité à :

analyser un problème de NLP appliqué,

construire un pipeline propre et reproductible,

documenter chaque étape selon les bonnes pratiques,

combiner approches linguistiques classiques (regex, règles) et modèles modernes (NER CamemBERT).

📌 Objectifs du projet

L’objectif principal est d’extraire automatiquement deux variables présentes dans des descriptions textuelles :

l’âge,

la ville de naissance.

Le projet se concentre volontairement sur des extraits courts (profils de patients, descriptions sociales…), comportant fautes de frappe, variations linguistiques et formulations diverses afin de simuler des données réalistes.

🧱 Architecture de la solution
1. Génération d’un jeu de données simulé

Le dataset textuel a été généré automatiquement, en intégrant :

fautes d’orthographe,

formulations variées,

formes syntaxiques multiples,

ambiguïtés volontaires.

Cette diversité permet de tester la robustesse des méthodes d’extraction.

2. Prétraitement du texte

Un pipeline de nettoyage standard adapté au français a été appliqué :

mise en minuscules,

suppression des accents,

suppression de la ponctuation,

nettoyage des espaces multiples,

retrait des stopwords (NLTK).

⚠️ Discussion : certains stopwords peuvent perturber la détection des villes par NER. Le notebook explore ces limites et propose des ajustements.

3. Extraction de l’âge (approche régex + analyse contextuelle)

Étapes :

Recherche de nombres dans le texte,

Analyse exploratoire des mots entourant les nombres,

Construction d’un regex robuste couvrant différentes variantes :

25 ans, 25a, 25 an, 25 années, etc.

Une visualisation permet d’observer le taux de détection et les cas non capturés.

💡 Limites explorées : chiffres écrits en lettres, fautes importantes, ponctuation supprimée.

4. Extraction de la ville de naissance (NER + règles contextuelles)

Méthodologie hybride :

Utilisation du modèle CamemBERT NER (Jean-Baptiste/camembert-ner) pour détecter les lieux (étiquettes LOC).

Analyse du contexte autour des entités reconnues.

Détection de la ville associée à des mots-clés :

née à, originaire de, natale de, etc.

Gestion des fautes typographiques grâce au NER (ex: Zmiens détecté comme Amiens).

5. Normalisation et correction des noms de villes

Pour garantir la qualité de la variable extraite :

Utilisation d’un référentiel officiel des communes (INSEE – data.gouv.fr)

Nettoyage en utilisant le même pipeline

Correction automatique via fuzzy matching (RapidFuzz)

Récupération du nom officiel (accents, tirets, capitalisation)

Ce processus permet d’obtenir une variable finale fiable et standardisée.

📊 Résultat final

Le notebook produit un dataframe final contenant :

Texte original	Âge détecté	Ville de naissance (normalisée)
Patiente âgée de 72 ans, née à Pariis…	72	Paris
Étudiante de 23 ans, née à l’hôpital d’Annecy…	23	Annecy
Patiente 29a, née à Bonneval-sur-Arc…	29	Bonneval-sur-Arc
…	…	…

Les exemples montrent la capacité du pipeline à gérer :

fautes de frappe,

variations syntaxiques,

villes ambiguës,

incohérences de ponctuation.

⚠️ Limites actuelles

Difficulté à détecter les nombres écrits en toutes lettres.

Certaines fautes trop éloignées peuvent échapper à la correction fuzzy.

Le NER détecte des lieux génériques (régions, départements) : une étape de filtrage est donc nécessaire.

Jeu de données réduit (non représentatif de toutes les situations possibles).

🚀 Pistes d’amélioration

Entraînement (fine-tuning) d’un modèle NER sur un corpus réellement annoté.

Intégration d’algorithmes phonétiques (Soundex français, Metaphone).

Évaluation systématique (precision, recall, F1-score) sur un jeu de test annoté manuellement.

Adaptation du preprocessing selon la variable extraite :

conserver certains stopwords pour les villes,

pipeline plus strict pour l’âge.

🛠️ Technologies utilisées

Python 3

Pandas

SpaCy

Transformers (HuggingFace)

CamemBERT NER

RapidFuzz

NLTK

Matplotlib

WordCloud

📁 Fichiers du dépôt

notebook_nlp_extraction.ipynb → Notebook complet

communes-france-2025.csv → Référentiel officiel des communes

README.md → Documentation du projet

👤 Auteur

Maxime Anselme Martin
Projet réalisé dans le cadre d’une démarche personnelle afin d’illustrer mes compétences en NLP appliqué, data engineering léger, et conception de pipelines d’extraction robustes.
