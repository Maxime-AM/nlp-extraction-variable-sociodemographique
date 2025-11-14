# 🧠 NLP – Extraction de variables sociodémographiques  
Extraction automatique de l’âge et de la ville de naissance à partir de textes non structurés.

---

## 🎯 Objectifs du projet

Ce projet vise à extraire automatiquement deux informations à partir de descriptions textuelles :

- **Âge**
- **Ville de naissance**

Le notebook met en œuvre une approche hybride combinant :

- des techniques linguistiques classiques (nettoyage, regex, règles),
- un modèle de reconnaissance d’entités nommées (CamemBERT NER),

Le but étant également de produire un code facilement explicable à des équipes non techniques.
Le projet a été réalisé dans le cadre d’une démarche personnelle pour démontrer mes compétences en NLP et en documentation, le tout en produisant un code lisible et conforme aux bonnes pratiques.

---

## 📊 Exemple de résultats

| Texte | Âge | Ville de naissance (normalisées) |
|-------|-----|------------------|
| Patiente âgée de 72 ans, née à Pariis, veuve, vit seule à domicile. Ancienne enseignante. | 72 | Paris |
| Étudiante de 23 ans, née à l'hopital d'Annecy, célibataire, vit en colocation à Marseile. | 23 | Annecy |
| Patiente 29a, professeure, née à Bonneval-sur-Arc, célibataire, vit en appartement en périphérie de Lille. | 29 | Bonneval-sur-Arc |

---

## Étapes du projet :	

### 1. Génération du jeu de données
Création de textes simulés incluant :
- fautes de frappe réalistes,
- formulations variées,
- ambiguïtés lexicales et syntaxiques.

---

### 2. Prétraitement du texte
Le pipeline de nettoyage inclut :

- passage en minuscules  
- suppression des accents  
- suppression de la ponctuation  
- retrait des espaces multiples  
- suppression des stopwords (NLTK)

---

### 3. Extraction de l’âge (Regex + analyse contextuelle)

Méthodologie :
- extraction des nombres et de leur contexte lexical,
- exploration des motifs entourant les chiffres,
- construction d’un regex robuste permettant de capturer différentes formes :  
  `25 ans`, `25a`, `25 an`, `25 année`, `25 annees`, etc.

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

---

## ⚠️ Limites

- Nombres écrits en lettres non pris en charge  
- Certaines fautes trop importantes échappent au fuzzy matching  
- Dataset réduit

---

## 🚀 Améliorations possibles

- Fine-tuning d’un modèle NER sur un corpus annoté  
- Mise en place d’un jeu de test annoté et calcul de métriques (Precision, Recall, F1)  
- Ajustement dynamique du prétraitement selon la variable à extraire

---

## 🛠️ Technologies utilisées

- Python 
- Pandas  
- SpaCy  
- HuggingFace Transformers  
- RapidFuzz  
- NLTK  
- Matplotlib  
