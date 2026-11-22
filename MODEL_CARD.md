# Model Card - fraude (version servie par l'alias production) 
  
## Modele 
Nom et version dans le registre MLflow : fraude version 1 alias production
Famille et reglages : RandomForestClassifier 500 arbres profondeur 16 random_state 42
Run d'origine (MLflow) : 3997327bb2b64883a6876fa82c3d8298
  
## Objectif et usage prevu 
Detecter les paiements frauduleux et aider un conseiller a decider d'un blocage ou d'une verification
  
## Usages exclus 
Bloquer definitivement un compte sans revue humaine
Utiliser le modele sur d'autres types de donnees que ces 4 variables
Prendre une decision sans pouvoir donner d'explication au client
  
## Donnees 
Fichier, nombre de lignes, part de fraudes, empreinte (tag donnees_md5) : data/transactions.csv 5000 lignes 568 fraudes soit environ 11 pour cent
Origine des donnees (reelles ou synthetiques) : synthetiques generees par un script avec graine fixe
donnees_md5: cf9f6f86c141e7df32c61715c493483f

## Performances (jeu de test, 1 250 transactions) 
F1 : 0.8669   precision : 0.8837   rappel : 0.8507   AUC : 0.9872 
  
## Methodes d'explicabilite 
Globale : importance par permutation et SHAP global (beeswarm)
Locale : SHAP waterfall (exact et additif) et LIME (instable selon la graine)
Variables les plus influentes : montant puis freq_24h anciennete et nuit (permutation) montant seuil vers 1000 MAD client recent et paiement de nuit augmentent le risque
  
## Comparaison avec un modele interpretable 
Regression logistique avec log10(montant) F1 0.8593 et 113 fraudes detectees sur 134 ecart dans le bruit du test
Arbre de profondeur 3 F1 0.5737 trop faible
La logistique est une alternative lisible credible
  
## Limites et risques connus 
Donnees synthetiques performances a confirmer sur des donnees reelles
Seulement 134 fraudes dans le test donc les metriques sont bruitees
LIME depend du hasard et SHAP montre une association pas une cause
Risque de faux positifs (15 clients normaux bloques sur 1116)
Effet de nuit et de montant a surveiller
  
## Recommandations d'utilisation et de surveillance 
Revue humaine avant tout blocage definitif
Suivre F1 rappel et precision chaque mois
Reentrainer et relancer les explications si les donnees changent
Relancer les explications a chaque changement d'alias
  
## Tracabilite 
Run des explications (experience tp7-explicabilite) : ... 
Auteurs : Ayoub Najimi   Date : 12/11/2026   Prochaine revue : ...