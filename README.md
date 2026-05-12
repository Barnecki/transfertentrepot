# TransfertEntrepot — Module Dolibarr

**Éditeur :** [BARNECKI SPIRITS](https://github.com/Barnecki)  
**Compatibilité :** Dolibarr 14+  
**Version courante :** voir fichier `VERSION`

---

## Description

Module Dolibarr permettant de générer des **bons de transfert PDF** entre entrepôts directement depuis la liste des mouvements de stock.

---

## Fonctionnalités

- **Bouton sticky** sur la liste des mouvements de stock (`Produits → Stocks → Mouvements`) : apparaît dès qu'au moins un mouvement est coché
- **Formulaire de saisie** : sélection des entrepôts source/destination et date de transfert, avec auto-détection de l'entrepôt source
- **Génération PDF** du bon de transfert avec :
  - En-tête : logo société, coordonnées, SIRET / EA / TVA
  - Blocs entrepôt source et destination avec adresses complètes
  - Tableau des mouvements : Réf. produit, Désignation, Lot/Série, Entrepôt, Quantité
  - Zone de signatures expéditeur / destinataire
  - Mentions légales configurables en pied de page
- **Liste des bons générés** (`Produits → Stocks → Bons de transfert`) avec lien PDF direct
- **Mode Distillerie** : affiche le numéro d'Entrepositaire Agréé (EA) et ajoute automatiquement la mention DSA (conformément à l'article 307 du CGI)

---

## Installation

1. Décompresser le ZIP dans `htdocs/custom/`
2. Activer le module dans `Admin → Modules` (section **Produits**)
3. Configurer dans `Admin → Modules → TransfertEntrepot → Configurer`

---

## Configuration

| Paramètre | Description |
|---|---|
| **Activité Distillerie** | Active l'affichage EA et la mention DSA dans le PDF |
| **Logo sur PDF** | Affiche le logo de la société dans l'en-tête |
| **Préfixe numérotation** | Préfixe des références (défaut : `BT`) |
| **Mentions légales** | Texte libre affiché en pied du bon de transfert |

**Variable globale Dolibarr requise pour le mode Distillerie :**  
`MAIN_INFO_ID_EA` → numéro d'Entrepositaire Agréé (ex. `FR000000N09999`)

---

## Structure des fichiers

```
TransfertEntrepot/
├── admin/setup.php                          Configuration du module
├── bon_transfert.php                        Formulaire de saisie + génération PDF
├── list_bons.php                            Liste des bons générés + fichiers PDF
├── class/actions_transfertentrepot.class.php  Hook sur movement_list.php
├── core/modules/
│   ├── modTransfertEntrepot.class.php       Descripteur du module
│   └── entrepot/doc/pdf_bon_transfert.modules.php  Générateur PDF TCPDF
├── lib/transfertentrepot.lib.php            Fonctions utilitaires
├── langs/fr_FR/transfertentrepot.lang       Traductions FR
├── langs/en_US/transfertentrepot.lang       Traductions EN
├── sql/llx_transfert_bon.sql               Tables SQL
└── VERSION                                  Version courante
```

---

## Tables SQL

- `llx_transfert_bon` : bons de transfert (ref, entrepôts src/dst, dates, statut)
- `llx_transfert_bon_mouvement` : liaisons bon ↔ mouvements de stock

---

## Droits

| Droit | Description |
|---|---|
| `transfertentrepot → stock → generate_bon_transfert` | Générer et consulter les bons de transfert |

---

## Licence

Ce module est distribué sous licence **GPL v3**.  
© 2024 BARNECKI SPIRITS — [https://github.com/Barnecki](https://github.com/Barnecki)
