# ChangeLog — TransfertEntrepot

## [1.0.2] — 2026-06-25

### Corrections
- `pdf_bon_transfert.modules.php` : les blocs de signature ne sont plus decoupes sur plusieurs pages quand la liste de mouvements est longue
  - Avant d ecrire les signatures, on calcule l espace disponible restant sur la page
  - Si la hauteur disponible < 38mm (hauteur minimale des blocs signature), une nouvelle page est creee
  - `_pagefoot()` est appele avant le saut de page pour avoir le pied de page sur chaque page

---

## [1.0.1] — 2026-05-17

### Modifications
- Changement de l ID du module : 500110 → 250000
- Changement de l ID du droit : 500161 → 250001
- Cles de langue mises a jour : Module500110Name/Desc → Module250000Name/Desc

> Apres installation, desactiver puis reactiver le module pour que les droits soient recrees en base.

---

## [1.0.0] — 2026-05-17

### Version initiale

Premier module stable de generation de bons de transfert PDF entre entrepots pour Dolibarr 14+.

**Fonctionnalites incluses :**
- Bouton sticky sur la liste des mouvements de stock (hook printFieldListFooter)
- Formulaire de saisie avec auto-detection de l entrepot source
- Generation PDF TCPDF avec en-tete societe, blocs entrepots, tableau mouvements, signatures, pied de page
- Mode Distillerie : numero EA et mention DSA (article 307 du CGI)
- Colonnes prix optionnelles dans le PDF (P.U. HT et Total HT)
- Liste des bons avec pagination, tri, recherche, visionneuse PDF, regeneration et suppression
- Support multi-entite Dolibarr
