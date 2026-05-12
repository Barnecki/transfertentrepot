# Module TransfertEntrepot — Dolibarr

## Description

Ce module ajoute une **action de masse** sur la liste des mouvements de stock de Dolibarr (`Stock > Mouvements de stock`), permettant de générer un **bon de transfert PDF** entre deux entrepôts.

Le document PDF récupère automatiquement les informations complètes des entrepôts (adresse, téléphone, email) depuis les fiches entrepôt Dolibarr et liste les articles/mouvements sélectionnés dans un tableau détaillé.

---

## Fonctionnalités

- ✅ **Action de masse** intégrée via le système de hooks Dolibarr (contexte `stockmouvement`)
- ✅ Formulaire intermédiaire de sélection via `formconfirm` standard Dolibarr
- ✅ **Récupération automatique** des informations d'entrepôt (adresse complète, téléphone, email, pays)
- ✅ Tableau des mouvements : réf. produit, désignation, lot/série, quantité, unité, P.U. HT, Total HT
- ✅ Ligne de **totaux** (quantité + montant HT)
- ✅ Zone de **signatures** côte à côte (expéditeur / destinataire)
- ✅ **Numérotation automatique** configurable (préfixe + AAAAMM + séquence)
- ✅ Logo société optionnel (via constante Dolibarr)
- ✅ **Mentions légales** personnalisables en pied de document
- ✅ **Journalisation** des bons générés en base (`llx_transfert_bon` + `llx_transfert_bon_mouvement`)
- ✅ Gestion des **droits Dolibarr**
- ✅ Page d'**administration** (préfixe, logo, mentions)
- ✅ Langues : **fr_FR** / **en_US**
- ✅ Compatible **ModuleBuilder**

---

## Installation

### 1. Copier le dossier

```bash
cp -r transfertentrepot /var/www/html/dolibarr/htdocs/custom/
```

### 2. Structure requise

```
custom/transfertentrepot/
├── admin/
│   └── setup.php                                  ← Page de configuration admin
├── class/
│   └── actions_transfertentrepot.class.php        ← Hooks (action de masse)
├── core/modules/
│   ├── modTransfertEntrepot.class.php             ← Descripteur du module
│   └── entrepot/doc/
│       └── pdf_bon_transfert.modules.php          ← Générateur PDF
├── langs/
│   ├── fr_FR/transfertentrepot.lang
│   └── en_US/transfertentrepot.lang
├── lib/
│   └── transfertentrepot.lib.php                  ← Fonctions utilitaires
├── sql/
│   └── llx_transfert_bon.sql                      ← Tables BDD
├── bon_transfert.php                              ← Page formulaire + génération
├── modulebuilder.txt
└── VERSION
```

### 3. Activer le module

- Aller dans **Admin > Modules/Applications**
- Chercher **TransfertEntrepot** (famille : Produits)
- Cliquer sur **Activer**
- Les tables SQL sont créées automatiquement

### 4. Activer le hook

Dans **Admin > Autres paramètres**, ajouter :

| Constante | Valeur |
|---|---|
| `MAIN_HOOKS_stockmouvement` | `ActionsTransfertEntrepot` |

Ou via ligne de commande SQL :
```sql
INSERT INTO llx_const (name, value, type, visible, note, entity)
VALUES ('MAIN_HOOKS_stockmouvement', 'ActionsTransfertEntrepot', 'chaine', 0, 'Hook TransfertEntrepot', 1);
```

### 5. Attribuer les droits

- Aller dans **Admin > Utilisateurs > [utilisateur] > Permissions**
- Activer : **TransfertEntrepot > Générer un bon de transfert PDF**

---

## Utilisation

1. Aller sur **Stock > Mouvements de stock**
2. **Cocher** les lignes de mouvements à inclure
3. Dans le select **"Actions de masse"**, choisir **"Générer un bon de transfert PDF"**
4. Valider → formulaire de sélection des entrepôts (popup Dolibarr standard)
5. Choisir **Entrepôt Source**, **Entrepôt Destination**, **Date de transfert**
6. Cliquer **Confirmer**
7. Le PDF est téléchargé automatiquement

---

## Configuration (Admin > Modules > TransfertEntrepot)

| Paramètre | Valeur défaut | Description |
|---|---|---|
| Logo sur PDF | Oui | Affiche le logo de la société |
| Préfixe numérotation | `BT` | Génère ex. `BT-202506-0001` |
| Mentions légales | (vide) | Texte en pied du bon |

---

## Tables créées

### `llx_transfert_bon`
Journalise chaque bon de transfert généré.

| Champ | Type | Description |
|---|---|---|
| `ref` | VARCHAR(64) | Référence unique du bon |
| `fk_entrepot_src` | INT | Entrepôt source |
| `fk_entrepot_dst` | INT | Entrepôt destination |
| `date_transfert` | DATETIME | Date du transfert |
| `fk_user_creat` | INT | Utilisateur créateur |
| `statut` | SMALLINT | 0=brouillon |

### `llx_transfert_bon_mouvement`
Liaison entre un bon et ses mouvements de stock.

---

## Compatibilité
- Dolibarr ≥ 14.0
- PHP ≥ 7.0
- Module **Stock** obligatoire (`modStock`)
