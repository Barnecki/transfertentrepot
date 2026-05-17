# TransfertEntrepot — Module Dolibarr

**Editeur :** [BARNECKI SPIRITS](https://github.com/Barnecki)  
**Depot :** [https://github.com/Barnecki/transfertentrepot](https://github.com/Barnecki/transfertentrepot)  
**Version :** 1.0.0  
**Compatibilite :** Dolibarr 21+  
**Licence :** GPL v3

---

## Description

Module Dolibarr permettant de generer des **bons de transfert PDF** entre entrepots directement depuis la liste des mouvements de stock (`Produits > Stocks > Mouvements`).

---

## Fonctionnalites

### Bouton sticky sur la liste des mouvements
Apparait automatiquement des qu au moins un mouvement est coche. Affiche le nombre de mouvements selectionnes et permet de generer un bon de transfert en un clic.

### Formulaire de saisie
- Selection de l entrepot source (auto-detecte si tous les mouvements viennent du meme entrepot) et de l entrepot destination
- Choix de la date de transfert
- Tableau de previsualisation des mouvements selectionnes : Ref. mouvement, Ref. produit, Designation, Lot/Serie, Entrepot, Quantite

### Generation PDF
- **En-tete** : logo societe, coordonnees, SIRET, EA (mode Distillerie), TVA
- **Blocs entrepots** : source et destination avec adresses completes
- **Tableau mouvements** : Ref. mvt, Ref. produit, Designation, Lot/Serie, Entrepot, Quantite (colonnes Prix unitaire HT et Total HT optionnelles)
- **Ligne totaux** : nombre de mouvements (et total HT si prix actives)
- **Signatures** : expediteur (date et heure expedition) et destinataire (date et heure reception)
- **Pied de page** : mentions legales + version Dolibarr + version module + numero de page

### Liste des bons de transfert (`Produits > Stocks > Bons de transfert`)
- Tableau paginé avec tri sur toutes les colonnes
- Champs de recherche : Ref, Entrepot Source, Entrepot Destination
- Visionneuse PDF integree (loupe) + telechargement direct
- Bouton regenerer : recree le PDF sans modifier l enregistrement en base
- Bouton supprimer : supprime le bon en base, ses liaisons et le fichier PDF

### Mode Distillerie
Affiche le numero d Entrepositaire Agree (EA) dans l en-tete du PDF et ajoute automatiquement la mention DSA dans les mentions legales, conformement a l article 307 du CGI.

---

## Installation

1. Decompresser le ZIP dans `htdocs/custom/`
2. Activer le module dans `Admin > Modules` (section **Produits**)
3. Configurer via le bouton **Configurer** du module

---

## Configuration

| Parametre | Description | Defaut |
|---|---|---|
| **Activite Distillerie** | Active EA dans l en-tete PDF et la mention DSA | Non |
| **Logo sur PDF** | Affiche le logo de la societe dans l en-tete | Oui |
| **Afficher les prix dans le PDF** | Colonnes P.U. HT et Total HT dans le tableau | Non |
| **Prefixe numerotation** | Prefixe des references (ex : `BT` pour `BT-202506-0001`) | `BT` |
| **Mentions legales** | Texte libre en pied de page du bon de transfert | Vide |

### Variable globale Dolibarr pour le mode Distillerie

`MAIN_INFO_ID_EA` → numero d Entrepositaire Agree (ex. `FR052005N0749`)  
A definir dans `Admin > Autres parametres`

---

## Structure des fichiers

```
transfertentrepot/
├── CLAUDE.md                                              Guide pour Claude Code
├── ChangeLog.md                                           Historique des versions
├── README.md                                              Ce fichier
├── VERSION                                                Version courante
├── admin/
│   └── setup.php                                          Page de configuration
├── bon_transfert.php                                       Formulaire de saisie + generation PDF
├── bons_list.php                                          Liste des bons + actions
├── class/
│   └── actions_transfertentrepot.class.php                Hook sur movement_list.php
├── core/modules/
│   ├── modTransfertEntrepot.class.php                     Descripteur du module
│   └── entrepot/doc/pdf_bon_transfert.modules.php         Generateur PDF TCPDF
├── lib/
│   └── transfertentrepot.lib.php                          Fonctions utilitaires
├── langs/
│   ├── fr_FR/transfertentrepot.lang                       Traductions francais
│   └── en_US/transfertentrepot.lang                       Traductions anglais
└── sql/
    └── llx_transfert_bon.sql                              Creation des tables SQL
```

---

## Tables SQL

| Table | Description |
|---|---|
| `llx_transfert_bon` | Bons de transfert : ref, entrepots src/dst, dates, utilisateur, entity |
| `llx_transfert_bon_mouvement` | Liaisons bon <-> mouvements de stock |

---

## Droits

| Droit | Description |
|---|---|
| `transfertentrepot > stock > generate_bon_transfert` | Generer, consulter, regenerer et supprimer les bons |

---

## Verification des mises a jour

La derniere version disponible est consultable sur :  
[https://raw.githubusercontent.com/Barnecki/transfertentrepot/main/VERSION](https://raw.githubusercontent.com/Barnecki/transfertentrepot/main/VERSION)

---

## Licence

Ce module est distribue sous licence **GPL v3**.  
(c) 2024-2026 BARNECKI SPIRITS — [https://github.com/Barnecki](https://github.com/Barnecki)
