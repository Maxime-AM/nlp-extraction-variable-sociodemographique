# 🧠 NLP – Extraction de variables sociodémographiques  
Extraction automatique de l’âge et de la ville de naissance à partir de textes non structurés.

---

## 🎯 Objectifs du projet

Ce projet vise à extraire automatiquement deux informations à partir de descriptions textuelles :

- **Âge**
- **Ville de naissance**

Le notebook met en œuvre une approche hybride combinant :

- des techniques linguistiques classiques (nettoyage, regex, règles),
- un modèle moderne de reconnaissance d’entités nommées (CamemBERT NER),
- une normalisation à l’aide d’un référentiel officiel des communes françaises.

Le projet a été réalisé dans le cadre d’une démarche personnelle pour démontrer mes compétences en NLP appliqué, en structuration de pipeline et en documentation claire.

---

## 🧱 Pipeline de traitement

### 1. Génération du jeu de données
Création de textes simulés incluant :
- fautes de frappe réalistes,
- formulations variées,
- ambiguïtés lexicales et syntaxiques.

Ces données permettent de tester la robustesse des méthodes d’extraction.

---

### 2. Prétraitement du texte
Le pipeline de nettoyage inclut :

- passage en minuscules  
- suppression des accents  
- suppression de la ponctuation  
- retrait des espaces multiples  
- suppression des stopwords (NLTK)

> Remarque : la suppression de certains stopwords peut perturber la détection des villes par NER. Ce point est discuté dans le notebook.

---

### 3. Extraction de l’âge (Regex + analyse contextuelle)

Méthodologie :
- extraction des nombres et de leur contexte lexical,
- exploration des motifs entourant les chiffres,
- construction d’un regex robuste permettant de capturer différentes formes :  
  `25 ans`, `25a`, `25 an`, `25 année`, `25 annees`, etc.

Une analyse visuelle du taux de détection est fournie, ainsi que les cas non capturés.

---

### 4. Extraction de la ville de naissance (NER + règles)

Méthode en deux étapes :

#### a) Détection initiale avec CamemBERT NER  
Extraction de toutes les entités de type **LOC**, avec un seuil de confiance ajusté pour gérer les fautes typographiques.

#### b) Analyse contextuelle  
Détection d’un lien entre la ville et des indicateurs de naissance :

- « née à »
- « natale de »
- « originaire de »

---

### 5. Normalisation avec référentiel INSEE

Pour garantir la qualité de la variable extraite :

- utilisation d’un fichier officiel des communes françaises (data.gouv.fr),
- nettoyage identique à celui des textes,
- correspondance via similarité (RapidFuzz),
- récupération de la forme officielle de la ville.

Résultat : une variable standardisée, corrigée, et conforme au référentiel géographique.

---

## 📊 Exemple de résultats

| Texte | Âge | Ville normalisée |
|-------|-----|------------------|
| Patiente âgée de 72 ans, née à Pariis… | 72 | Paris |
| Étudiante née à l’hôpital d’Annecy… | 23 | Annecy |
| Patiente née à Bonneval-sur-Arc… | 29 | Bonneval-sur-Arc |
| … | … | … |

---

## ⚠️ Limites

- Nombres écrits en lettres non pris en charge  
- Certaines fautes trop importantes échappent au fuzzy matching  
- Le modèle NER détecte des lieux non municipaux (départements, régions)  
- Dataset réduit, utilisé pour un test conceptuel

---

## 🚀 Améliorations possibles

- Fine-tuning d’un modèle NER sur un corpus annoté  
- Ajout d’algorithmes phonétiques (Soundex, Metaphone français)  
- Mise en place d’un jeu de test annoté et calcul de métriques (Precision, Recall, F1)  
- Ajustement dynamique du prétraitement selon la variable à extraire

---

## 🛠️ Technologies utilisées

- Python 3  
- Pandas  
- SpaCy  
- HuggingFace Transformers  
- CamemBERT NER  
- RapidFuzz  
- NLTK  
- Matplotlib  
- WordCloud  

---

## 📁 Structure du dépôt

