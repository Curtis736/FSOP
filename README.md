# FSOP
# Tickets Jira - Application FSOP

Ce document contient tous les tickets formatés pour import dans Jira.

---

## ÉPIC 1 : Infrastructure et connexions

### FSOP-001 : Configuration initiale du projet

**Type :** Story  
**Priorité :** Highest  
**Sprint :** Sprint 1  
**Étiquettes :** setup, infrastructure, foundation

**Description :**
Initialiser la structure complète du projet avec frontend et backend, configurer l'environnement de développement et mettre en place l'organisation du code.

**Critères d'acceptation :**
- [ ] Structure de dossiers créée (frontend/backend séparés)
- [ ] Configuration environnement de développement opérationnelle
- [ ] Dependencies installées (package.json, requirements.txt)
- [ ] Configuration Git initialisée avec .gitignore
- [ ] Documentation README avec instructions de démarrage complètes

**Fichiers à créer :**
- `backend/package.json` ou `backend/requirements.txt`
- `frontend/package.json`
- `.gitignore`
- `README.md`
- Structure de dossiers : `src/`, `config/`, `tests/`

**Notes techniques :**
- Choisir entre Node.js/Express ou Python/FastAPI pour le backend
- Frontend : Vue.js 3 avec Vite
- Configuration des outils de développement (ESLint, Prettier si nécessaire)

---

### FSOP-002 : Connexion et extraction de données depuis Silog

**Type :** Story  
**Priorité :** Highest  
**Sprint :** Sprint 1  
**Étiquettes :** silog, integration, data-extraction  
**Blocante pour :** FSOP-007, FSOP-009

**Description :**
Analyser la méthode de connexion disponible à Silog et développer le module permettant de se connecter et récupérer les données nécessaires à la génération des formulaires FSOP.

**Données à récupérer depuis Silog :**
- Numéro de lancement (clé principale)
- Numéro OT
- Fiche de suivi F88
- PN Latecoere
- Référence (RCT-XXX-LATXX.XXX)
- Numéro de série (S/N)
- Désignation produit
- Références MO associées (MO 1183, MO 1120, MO 956, MO 1206, etc.)
- Liste des composants avec leurs lots
- Spécifications techniques (longueur, tolérances, etc.)

**Critères d'acceptation :**
- [ ] Méthode de connexion Silog identifiée et documentée (API REST, ODBC, fichier, etc.)
- [ ] Module de connexion/authentification Silog fonctionnel
- [ ] Service de récupération des données suivant opérationnel :
  - [ ] Numéro de lancement
  - [ ] Numéro OT
  - [ ] Fiche de suivi F88
  - [ ] PN Latecoere
  - [ ] Référence
  - [ ] Désignation produit
  - [ ] Références MO
  - [ ] Liste composants et lots
  - [ ] Spécifications techniques
- [ ] Tests de connexion et extraction réussis
- [ ] Gestion des erreurs de connexion implémentée (timeout, erreurs réseau, etc.)
- [ ] Documentation de la configuration Silog créée

**Fichiers à créer :**
- `backend/src/services/silogService.js`
- `backend/config/silog.json`
- `backend/tests/services/silogService.test.js`
- `docs/CONFIG_SILOG.md`

**Notes techniques :**
- À déterminer la méthode de connexion exacte à Silog
- Gérer les cas d'erreur et les timeouts
- Mettre en cache les données si nécessaire

---

### FSOP-003 : Connexion SQL Server pour récupération opérateurs (dspo)

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 1  
**Étiquettes :** database, sql-server, operators  
**Blocante pour :** FSOP-014

**Description :**
Configurer la connexion à SQL Server et développer le service permettant de récupérer les informations des opérateurs, notamment le champ `dspo` (numéro opérateur) utilisé pour l'auto-remplissage des formulaires.

**Critères d'acceptation :**
- [ ] Connexion SQL Server configurée et testée
- [ ] Service de récupération des informations opérateurs fonctionnel
- [ ] Récupération du champ `dspo` (numéro opérateur) opérationnelle
- [ ] Récupération des informations opérateur (nom, initiales si disponibles)
- [ ] Gestion des erreurs de connexion (serveur indisponible, timeout)
- [ ] Tests unitaires du service opérateurs avec mocks
- [ ] Pool de connexions configuré pour performance

**Fichiers à créer :**
- `backend/src/services/operatorService.js`
- `backend/src/config/database.js` (si pas déjà créé)
- `backend/tests/services/operatorService.test.js`

**Notes techniques :**
- Utiliser mssql pour Node.js ou pymssql pour Python
- Implémenter un pool de connexions
- Prévoir la gestion de la reconnexion automatique

---

### FSOP-004 : Accès et lecture des fichiers réseau X:/Traçabilité

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 1  
**Étiquettes :** network, file-system, tracabilite  
**Blocante pour :** FSOP-009

**Description :**
Configurer l'accès au dossier réseau `X:/Traçabilité` et développer le service permettant de lire les fichiers de traçabilité et rechercher des données par numéro de lancement pour remplir automatiquement les balises ** dans les formulaires.

**Critères d'acceptation :**
- [ ] Accès configuré au dossier réseau `X:/Traçabilité`
- [ ] Service de lecture des fichiers de traçabilité fonctionnel
- [ ] Support de différents formats de fichiers (Excel, CSV, etc.)
- [ ] Fonction de recherche par numéro de lancement implémentée
- [ ] Extraction des données nécessaires depuis les fichiers trouvés
- [ ] Gestion des erreurs (fichier non trouvé, accès réseau refusé, fichier verrouillé)
- [ ] Performance acceptable pour la recherche dans de gros fichiers
- [ ] Cache des résultats de recherche si applicable

**Fichiers à créer :**
- `backend/src/services/tracabiliteService.js`
- Configuration chemin réseau dans `.env`
- `backend/tests/services/tracabiliteService.test.js`

**Notes techniques :**
- Gérer les permissions d'accès réseau
- Parser les fichiers Excel/CSV selon le format réel
- Optimiser la recherche dans de gros fichiers

---

## ÉPIC 2 : Gestion des formulaires Word

### FSOP-005 : Système de gestion des templates Word

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 2  
**Étiquettes :** word, templates, upload

**Description :**
Créer un système permettant de gérer les templates de formulaires Word : upload, stockage, validation de la structure, et récupération pour génération de formulaires.

**Critères d'acceptation :**
- [ ] Endpoint API pour upload de templates Word (.docx)
- [ ] Validation du format (uniquement .docx accepté)
- [ ] Stockage sécurisé des templates dans un dossier dédié
- [ ] Validation de la structure des templates (détection des balises)
- [ ] Liste des templates disponibles via API GET
- [ ] Récupération d'un template spécifique par ID
- [ ] Gestion des versions de templates (historique)
- [ ] Interface admin pour upload (optionnel mais recommandé)
- [ ] Limite de taille de fichier configurable

**Fichiers à créer :**
- `backend/src/controllers/templateController.js`
- `backend/src/services/templateService.js`
- `backend/src/models/templateModel.js`
- `backend/src/routes/templateRoutes.js`
- `backend/uploads/templates/` (dossier de stockage)

**Notes techniques :**
- Utiliser multer pour l'upload de fichiers
- Stocker les métadonnées des templates en base de données
- Prévoir une interface d'administration pour gérer les templates

---

### FSOP-006 : Parser des templates Word et détection des balises

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 2  
**Étiquettes :** word, parser, balises  
**Blocante pour :** FSOP-007, FSOP-008, FSOP-009

**Description :**
Développer un parser permettant d'identifier les champs à remplir dans les templates Word et de détecter les différents types de balises (simples et balises ** pour traçabilité).

**Types de balises à détecter :**
- Balises simples : `{{numero_lancement}}`, `{{numero_ot}}`, `{{eref}}`, `{{fiche_suivi_F88}}`, etc.
- Balises traçabilité : `**` (double astérisque) - nécessitent recherche dans X:/Traçabilité

**Critères d'acceptation :**
- [ ] Parser capable de lire un fichier Word (.docx)
- [ ] Détection des balises simples dans le format `{{nom_variable}}`
- [ ] Détection des balises ** pour données de traçabilité
- [ ] Extraction de la liste complète des champs à remplir
- [ ] Identification de la position de chaque balise dans le document
- [ ] Détection des tableaux et champs dans les tableaux
- [ ] Détection des cases à cocher
- [ ] Retourner la structure complète du document analysée (JSON)
- [ ] Gérer les balises dans les en-têtes/pieds de page
- [ ] Tests unitaires avec différents templates

**Fichiers à créer :**
- `backend/src/services/wordParserService.js`
- `backend/tests/services/wordParserService.test.js`

**Notes techniques :**
- Utiliser python-docx (via wrapper Python) ou docx pour Node.js
- Parser tous les paragraphes, tableaux, en-têtes, pieds de page
- Retourner une structure de données claire pour le remplissage

---

### FSOP-007 : Remplissage automatique des balises simples (données Silog)

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 2  
**Étiquettes :** word, fill, silog  
**Blocante pour :** FSOP-010, FSOP-016  
**Dépend de :** FSOP-002, FSOP-006

**Description :**
Implémenter le remplacement automatique des balises simples dans les templates Word par les données récupérées depuis Silog lors de la génération d'un formulaire FSOP.

**Mapping des balises :**
- `{{numero_lancement}}` → Numéro de lancement depuis Silog
- `{{numero_ot}}` → Numéro OT depuis Silog
- `{{fiche_suivi_F88}}` → Fiche de suivi F88 depuis Silog
- `{{pn_latecoere}}` → PN Latecoere depuis Silog
- `{{reference}}` → Référence depuis Silog
- `{{designation}}` → Désignation produit depuis Silog
- `{{numero_serie}}` → Numéro de série généré (ou depuis Silog)

**Critères d'acceptation :**
- [ ] Remplacement de toutes les balises simples par les valeurs Silog correspondantes
- [ ] Remplacement dans les paragraphes, tableaux, en-têtes, pieds de page
- [ ] Conservation de la mise en forme du document
- [ ] Génération d'un nouveau fichier Word avec valeurs remplacées
- [ ] Gestion des balises non trouvées (laisser vide ou erreur selon configuration)
- [ ] Logs des remplacements effectués
- [ ] Tests avec templates réels

**Fichiers à créer :**
- `backend/src/services/wordFillService.js`
- `backend/tests/services/wordFillService.test.js`

**Notes techniques :**
- Préserver la mise en forme originale
- Gérer les cas où une balise n'a pas de valeur correspondante
- Optimiser les performances pour les gros documents

---

### FSOP-008 : Insertion de l'eref client dans le formulaire

**Type :** Story  
**Priorité :** Medium  
**Sprint :** Sprint 2  
**Étiquettes :** word, eref, client-data  
**Dépend de :** FSOP-007

**Description :**
Permettre l'insertion d'un champ "eref" fourni par le client dans le formulaire Word. L'eref doit être saisi par l'opérateur dans l'interface et inséré dans le document généré.

**Critères d'acceptation :**
- [ ] Champ eref disponible dans l'interface de saisie (FormEditor)
- [ ] Remplacement de la balise `{{eref}}` dans le template Word par la valeur saisie
- [ ] Validation du format de l'eref si nécessaire (caractères autorisés, longueur)
- [ ] Sauvegarde de l'eref dans les données du formulaire
- [ ] Affichage de l'eref dans l'interface de visualisation
- [ ] Possibilité de modifier l'eref avant validation finale

**Fichiers modifiés :**
- `frontend/src/components/FormEditor.vue`
- `backend/src/services/wordFillService.js`

**Notes techniques :**
- L'eref peut être saisi manuellement ou venir d'une source externe
- Prévoir une validation selon les règles métier

---

### FSOP-009 : Remplacement des balises ** par données X:/Traçabilité

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 2  
**Étiquettes :** word, tracabilite, network  
**Dépend de :** FSOP-004, FSOP-006  
**Blocante pour :** FSOP-010, FSOP-016

**Description :**
Implémenter la détection des balises ** dans les templates Word, rechercher les données correspondantes dans `X:/Traçabilité` selon le numéro de lancement, et remplacer automatiquement ces balises lors de la génération du formulaire.

**Processus :**
1. Détecter toutes les balises ** dans le document
2. Pour chaque balise, identifier le contexte (colonne, ligne, type de donnée)
3. Rechercher dans X:/Traçabilité avec le numéro de lancement
4. Extraire la ligne correspondante
5. Remplacer la balise ** par la valeur trouvée

**Critères d'acceptation :**
- [ ] Détection de toutes les balises ** dans le document Word
- [ ] Pour chaque balise **, recherche dans `X:/Traçabilité` avec le numéro de lancement
- [ ] Identification du contexte de la balise (quelle colonne, quel type de donnée)
- [ ] Extraction de la ligne correspondante dans le fichier de traçabilité
- [ ] Remplacement de la balise ** par la valeur trouvée
- [ ] Gestion du cas où aucune donnée n'est trouvée (message d'erreur, champ vide, ou marquage)
- [ ] Logs des recherches et remplacements
- [ ] Performance acceptable même avec plusieurs balises **

**Fichiers modifiés :**
- `backend/src/services/wordFillService.js` (extension)
- `backend/src/services/tracabiliteService.js` (extension)

**Notes techniques :**
- Le contexte de la balise ** peut être déterminé par la position dans un tableau
- Prévoir un mapping entre les colonnes des fichiers de traçabilité et les balises

---

### FSOP-010 : Gestion des champs multiples pour plusieurs lots

**Type :** Story  
**Priorité :** Medium  
**Sprint :** Sprint 3  
**Étiquettes :** word, lots, multiple-fields  
**Dépend de :** FSOP-007, FSOP-009

**Description :**
Développer la logique permettant de gérer plusieurs lots dans un même formulaire en générant des champs multiples ou des lignes de tableau supplémentaires selon le nombre de lots associés au lancement.

**Critères d'acceptation :**
- [ ] Détection du nombre de lots associés à un lancement (depuis Silog)
- [ ] Identification des sections du formulaire concernées par les lots multiples
- [ ] Génération automatique de champs multiples dans le formulaire (lignes de tableau, sections)
- [ ] Chaque lot possède ses propres champs dans le document Word
- [ ] Formulaire adapté dynamiquement selon le nombre de lots
- [ ] Préservation de la structure et mise en forme lors de la duplication
- [ ] Numérotation automatique des lots si nécessaire

**Fichiers à créer/modifier :**
- `backend/src/services/lotService.js`
- `backend/src/services/wordFillService.js` (extension)

**Notes techniques :**
- Gérer la duplication de tableaux et de sections
- Préserver la mise en forme lors de la duplication

---

## ÉPIC 3 : Interface utilisateur opérateurs

### FSOP-011 : Dashboard responsive pour tablettes - Liste des lancements

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 3  
**Étiquettes :** frontend, dashboard, tablet  
**Blocante pour :** FSOP-012

**Description :**
Créer l'interface dashboard principale optimisée pour tablettes affichant la liste des lancements disponibles et des FSOP à compléter, avec filtres et recherche.

**Critères d'acceptation :**
- [ ] Page dashboard responsive, optimisée pour tablettes (taille d'écran ≥ 768px)
- [ ] Affichage de la liste des lancements disponibles (depuis Silog)
- [ ] Pour chaque lancement, affichage des informations principales :
  - [ ] Numéro de lancement
  - [ ] Numéro OT
  - [ ] Référence
  - [ ] Statut (à compléter, en cours, complété)
  - [ ] Nombre de FSOP associés
- [ ] Filtres par :
  - [ ] Numéro de lancement
  - [ ] Numéro OT
  - [ ] Statut
  - [ ] Date
- [ ] Barre de recherche dans la liste
- [ ] Affichage des FSOP associés à chaque lancement (expandable)
- [ ] Indicateur visuel du statut de chaque FSOP (couleurs, icônes)
- [ ] Bouton pour générer un nouveau FSOP depuis un lancement
- [ ] Bouton pour ouvrir un FSOP existant
- [ ] Pagination si beaucoup de lancements
- [ ] Design moderne et intuitif pour manipulation tactile

**Fichiers à créer :**
- `frontend/src/components/Dashboard.vue`
- `frontend/src/views/DashboardView.vue`
- `frontend/src/services/api.js` (si pas déjà créé)
- `frontend/src/stores/dashboardStore.js` (Pinia store)

**Notes techniques :**
- Utiliser Vue 3 Composition API
- Design mobile-first puis tablette
- Prévoir des animations pour une meilleure UX

---

### FSOP-012 : Interface de remplissage de formulaire - Visualisation

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 3  
**Étiquettes :** frontend, form-editor, word-viewer  
**Dépend de :** FSOP-011

**Description :**
Créer l'interface permettant de visualiser et modifier le formulaire Word avec des champs interactifs. L'interface doit permettre de remplir le formulaire étape par étape sur tablette.

**Critères d'acceptation :**
- [ ] Visualisation du formulaire Word (conversion HTML ou viewer)
- [ ] Options de visualisation :
  - [ ] Conversion du Word en HTML pour affichage
  - [ ] Ou utilisation d'un viewer Word (mammoth.js, docx-preview, etc.)
- [ ] Formulaire interactif avec champs modifiables :
  - [ ] Champs de texte
  - [ ] Champs numériques
  - [ ] Cases à cocher
  - [ ] Sélecteurs (PASS/FAIL, etc.)
  - [ ] Champs date/heure
- [ ] Champs correctement positionnés selon le template Word
- [ ] Identification visuelle des champs obligatoires vs optionnels
- [ ] Interface optimisée pour manipulation tactile (tablette)
  - [ ] Boutons et champs de taille adaptée au tactile
  - [ ] Pas de hover, seulement click/tap
  - [ ] Zones de clic suffisamment grandes
- [ ] Responsive design (adaptation selon taille d'écran)
- [ ] Navigation entre les sections/étapes du formulaire
- [ ] Sauvegarde automatique périodique
- [ ] Indicateur de progression (étapes complétées)

**Fichiers à créer :**
- `frontend/src/components/FormEditor.vue`
- `frontend/src/components/WordViewer.vue` (si nécessaire)
- `frontend/src/components/FormField.vue` (composant réutilisable pour champs)
- `frontend/src/stores/formStore.js` (Pinia store pour gestion état formulaire)

**Notes techniques :**
- Évaluer mammoth.js ou docx-preview pour conversion Word → HTML
- Gérer l'état du formulaire (valeurs saisies, étapes complétées)
- Prévoir un système de mapping entre les balises Word et les composants Vue

---

### FSOP-013 : Bouton "Date aujourd'hui" auto-insérant la date

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 4  
**Étiquettes :** frontend, auto-fill, date  
**Dépend de :** FSOP-012

**Description :**
Implémenter un bouton pour chaque étape permettant d'insérer automatiquement la date du jour au moment du clic dans le formulaire. Élimine la saisie manuelle de la date.

**Critères d'acceptation :**
- [ ] Bouton "Date aujourd'hui" visible pour chaque étape nécessitant une date
- [ ] Au clic, insertion automatique de la date actuelle au format défini (ex: DD/MM/YYYY)
- [ ] Date insérée dans le bon champ du formulaire Word (mise à jour du document)
- [ ] Horodatage précis (date + heure si nécessaire)
- [ ] Possibilité de modifier manuellement la date après auto-insertion (si autorisé)
- [ ] Indication visuelle que la date a été auto-remplie
- [ ] Sauvegarde automatique après insertion
- [ ] Bouton accessible et bien visible (taille adaptée tablette)

**Fichiers modifiés :**
- `frontend/src/components/FormEditor.vue`
- `frontend/src/components/FormField.vue`

**Notes techniques :**
- Format de date configurable
- Gérer les fuseaux horaires si nécessaire
- Prévoir l'option heure également si besoin

---

### FSOP-014 : Champ opérateur auto-rempli depuis SQL (dspo)

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 4  
**Étiquettes :** frontend, backend, operators, auto-fill  
**Dépend de :** FSOP-003, FSOP-012

**Description :**
Implémenter le champ opérateur qui se remplit automatiquement avec les données SQL (dspo - numéro opérateur) tout en permettant à l'opérateur de noter ses initiales. Le dspo est récupéré depuis la base SQL lors de la connexion.

**Critères d'acceptation :**
- [ ] Récupération automatique du numéro opérateur (dspo) depuis SQL lors de l'ouverture du formulaire
- [ ] Champ opérateur auto-rempli dans le formulaire Word avec le dspo
- [ ] Affichage du dspo dans l'interface (lecture seule ou modifiable selon règles)
- [ ] Champ supplémentaire "Initiales" disponible pour saisie manuelle
- [ ] Sauvegarde du dspo ET des initiales dans les données du formulaire
- [ ] Affichage clair de l'opérateur connecté dans l'interface (en-tête, profil)
- [ ] Traçabilité : enregistrement du dspo pour chaque action (qui a fait quoi, quand)
- [ ] Gestion du cas où l'opérateur n'est pas connecté ou non trouvé en SQL

**Fichiers modifiés :**
- `frontend/src/components/FormEditor.vue`
- `backend/src/controllers/formController.js`
- `backend/src/services/operatorService.js` (extension)

**Notes techniques :**
- Le dspo peut venir d'une session utilisateur ou être passé en paramètre
- Prévoir un système d'authentification/identification des opérateurs

---

### FSOP-015 : Système de validation des étapes avec blocage

**Type :** Story  
**Priorité :** Highest  
**Sprint :** Sprint 4  
**Étiquettes :** validation, security, workflow  
**Dépend de :** FSOP-012

**Description :**
Implémenter un système de validation des étapes empêchant de passer à l'étape suivante si l'étape courante n'est pas complètement remplie. C'est une fonctionnalité critique pour garantir la qualité et la traçabilité.

**Critères d'acceptation :**
- [ ] Validation de tous les champs obligatoires de l'étape courante avant passage à la suivante
- [ ] Identification des champs obligatoires dans le template (balises spéciales ou configuration)
- [ ] Blocage de l'accès aux étapes suivantes si l'étape n'est pas validée (boutons désactivés, navigation bloquée)
- [ ] Messages d'erreur clairs indiquant les champs manquants ou invalides
- [ ] Indicateur visuel de progression des étapes (barre, étapes complétées/en cours/bloquées)
- [ ] Validation des formats (dates, nombres, etc.) avant validation d'étape
- [ ] Impossible de modifier une étape validée sans autorisation appropriée (rôle admin)
- [ ] Système de validation par étape (coche de validation, bouton "Valider étape")
- [ ] Historique des validations (qui a validé quelle étape, quand)
- [ ] Possibilité de "dévalider" une étape si erreur détectée (avec permissions)

**Fichiers à créer :**
- `frontend/src/components/StepValidator.vue`
- `backend/src/services/validationService.js`
- `backend/src/models/stepModel.js`
- `frontend/src/components/StepProgress.vue` (barre de progression)

**Notes techniques :**
- Définir la structure des étapes dans le template ou dans une configuration séparée
- Gérer les règles de validation (champs obligatoires, formats, valeurs min/max)
- Prévoir un système de permissions pour la dévalidation

---

## ÉPIC 4 : Fonctionnalités avancées

### FSOP-016 : Génération automatique de multiples FSOP identiques

**Type :** Story  
**Priorité :** Medium  
**Sprint :** Sprint 5  
**Étiquettes :** generation, batch, serial-numbers  
**Dépend de :** FSOP-007, FSOP-009

**Description :**
Développer la fonction permettant de générer plusieurs FSOP identiques à partir d'un numéro de lancement, avec attribution automatique de numéros de série uniques. Chaque FSOP sera un fichier Word séparé.

**Critères d'acceptation :**
- [ ] Fonction de génération à partir d'un numéro de lancement
- [ ] Interface permettant de spécifier le nombre de FSOP à générer
- [ ] Attribution automatique de numéros de série uniques pour chaque FSOP
- [ ] Format de numéro de série configurable :
  - [ ] Format standard : `XX-ZZ-SN` (ex: LT2500706-01-001)
  - [ ] Format personnalisé : `ZZ // XX/YY/ZZ` (selon besoin)
- [ ] Génération de fichiers Word individuels (un par FSOP)
- [ ] Tous les FSOP générés avec données Silog pré-remplies
- [ ] Chaque FSOP a un numéro de série unique
- [ ] Sauvegarde des FSOP générés dans le système
- [ ] Liste des FSOP générés accessible depuis le dashboard
- [ ] Indication du nombre de FSOP générés et statut
- [ ] Gestion des erreurs si génération partielle

**Fichiers à créer :**
- `backend/src/services/fsopGenerationService.js`
- `backend/src/controllers/generationController.js`
- `frontend/src/components/FSOPGenerator.vue` (dialog ou page)

**Notes techniques :**
- Gérer l'incrémentation des numéros de série
- Prévoir un système de réservation de numéros pour éviter les doublons
- Optimiser les performances pour génération de beaucoup de FSOP

---

### FSOP-017 : Sauvegarde automatique et export des formulaires

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 5  
**Étiquettes :** save, export, word  
**Dépend de :** FSOP-012

**Description :**
Implémenter la sauvegarde automatique des formulaires remplis et permettre l'export des FSOP complétés en format Word pour archivage. Élimine le risque de perte de données.

**Critères d'acceptation :**
- [ ] Sauvegarde automatique périodique pendant le remplissage (toutes les X secondes/minutes)
- [ ] Sauvegarde manuelle disponible (bouton "Sauvegarder")
- [ ] Indicateur visuel de l'état de sauvegarde (sauvegardé/en cours/non sauvegardé)
- [ ] Export du formulaire complété en format Word (.docx)
- [ ] Génération du fichier Word final avec toutes les données saisies
- [ ] Historique des modifications sauvegardé (versions)
- [ ] Récupération possible d'une version précédente (annulation)
- [ ] Export incluant toutes les données : Silog, saisies manuelles, dates auto, opérateur
- [ ] Téléchargement du fichier Word généré
- [ ] Archivage automatique du formulaire complété (stockage serveur)
- [ ] Gestion des conflits si plusieurs personnes modifient en même temps

**Fichiers à créer :**
- `backend/src/services/saveService.js`
- `backend/src/services/exportService.js`
- `frontend/src/components/SaveStatus.vue`
- `backend/src/models/formVersionModel.js` (pour historique)

**Notes techniques :**
- Utiliser des WebSockets ou polling pour la sauvegarde automatique
- Prévoir un système de versioning (Git-like ou simplifié)
- Optimiser la génération du Word final

---

### FSOP-018 : Contrôle des champs obligatoires et validation des formats

**Type :** Story  
**Priorité :** High  
**Sprint :** Sprint 5  
**Étiquettes :** validation, formats, quality  
**Dépend de :** FSOP-015

**Description :**
Implémenter la validation des champs obligatoires et le contrôle des formats (dates, numéros de série, mesures, etc.) avec messages d'erreur appropriés pour garantir la qualité des données saisies.

**Validations à implémenter :**
- Champs obligatoires (non vides)
- Format des dates (DD/MM/YYYY)
- Format des heures (HH:MM)
- Format des numéros de série (selon règles définies)
- Format des mesures (nombres avec décimales, unités)
- Plages de valeurs (ex: longueur 3677 +/-50mm)
- Valeurs min/max (ex: perte d'insertion ≤ 0,3dB)

**Critères d'acceptation :**
- [ ] Détection des champs obligatoires dans le template (balises ou configuration)
- [ ] Validation en temps réel lors de la saisie (feedback immédiat)
- [ ] Validation du format des dates (avec sélecteur de date si possible)
- [ ] Validation du format des heures
- [ ] Validation du format des numéros de série (règles configurables)
- [ ] Validation des mesures selon spécifications (tolérances, plages)
- [ ] Validation PASS/FAIL pour contrôles (ex: ≤ 0,3dB)
- [ ] Messages d'erreur clairs et contextuels (à côté du champ)
- [ ] Empêchement de la validation d'étape si erreurs présentes
- [ ] Liste récapitulative des erreurs avant export
- [ ] Indicateurs visuels (rouge = erreur, vert = OK)

**Fichiers modifiés :**
- `backend/src/services/validationService.js` (extension)
- `frontend/src/components/FormEditor.vue`
- `frontend/src/components/FormField.vue` (validation par champ)

**Notes techniques :**
- Définir les règles de validation dans une configuration (JSON, YAML)
- Utiliser des regex pour les formats
- Prévoir des validateurs personnalisables

---

## ÉPIC 5 : Tests et déploiement

### FSOP-019 : Tests unitaires et d'intégration

**Type :** Task  
**Priorité :** High  
**Sprint :** Sprint 6  
**Étiquettes :** testing, quality, coverage

**Description :**
Développer une suite complète de tests unitaires pour tous les services et tests d'intégration pour le flux complet de l'application, garantissant la qualité et la maintenabilité du code.

**Critères d'acceptation :**
- [ ] Tests unitaires pour tous les services :
  - [ ] `silogService` - connexion et extraction
  - [ ] `operatorService` - récupération opérateurs
  - [ ] `tracabiliteService` - lecture fichiers réseau
  - [ ] `wordParserService` - parsing templates
  - [ ] `wordFillService` - remplissage Word
  - [ ] `validationService` - validation étapes
  - [ ] `fsopGenerationService` - génération batch
- [ ] Tests d'intégration pour le flux complet :
  - [ ] Connexion Silog → Récupération données → Génération FSOP → Remplissage → Validation → Export
- [ ] Tests frontend :
  - [ ] Composants Vue (Dashboard, FormEditor, etc.)
  - [ ] Stores Pinia
  - [ ] Services API
- [ ] Couverture de code > 70%
- [ ] Tous les tests passent avant merge
- [ ] Tests CI/CD configurés
- [ ] Documentation des tests et exemples d'utilisation

**Fichiers créés :**
- `backend/tests/` (suite complète)
- `frontend/tests/` (suite complète)
- Configuration Jest/Vitest
- `.github/workflows/tests.yml` (CI)

**Notes techniques :**
- Utiliser Jest pour Node.js ou pytest pour Python
- Utiliser Vitest pour Vue.js
- Mocker les dépendances externes (Silog, SQL, fichiers réseau)

---

### FSOP-020 : Tests sur appareils tablettes réels

**Type :** Task  
**Priorité :** Medium  
**Sprint :** Sprint 6  
**Étiquettes :** testing, tablet, ux  
**Dépend de :** FSOP-011, FSOP-012

**Description :**
Effectuer des tests utilisateurs sur des tablettes réelles pour valider l'ergonomie, la responsivité et le fonctionnement de l'interface tactile. Identifie les problèmes d'UX spécifiques aux tablettes.

**Critères d'acceptation :**
- [ ] Tests effectués sur au moins 2 modèles de tablettes différents :
  - [ ] iPad (si applicable)
  - [ ] Tablette Android
  - [ ] Autres modèles selon disponibilité
- [ ] Validation de l'ergonomie tactile :
  - [ ] Taille des boutons et champs adaptée
  - [ ] Zones de clic suffisamment grandes
  - [ ] Pas de problèmes de double-clic
- [ ] Validation de la responsivité :
  - [ ] Affichage correct sur différentes résolutions
  - [ ] Pas de débordement de contenu
  - [ ] Navigation fluide
- [ ] Tests de performance :
  - [ ] Temps de chargement acceptables
  - [ ] Pas de lag lors de la saisie
- [ ] Tests utilisateurs avec opérateurs réels :
  - [ ] Facilité d'utilisation
  - [ ] Compréhension de l'interface
  - [ ] Satisfaction générale
- [ ] Correction des bugs identifiés
- [ ] Rapport de tests documenté avec :
  - [ ] Modèles de tablettes testés
  - [ ] Bugs identifiés et corrigés
  - [ ] Améliorations UX suggérées
  - [ ] Feedback utilisateurs

**Fichiers créés :**
- `docs/TESTS_TABLETTE.md`
- Liste des bugs identifiés dans le système de tickets

**Notes techniques :**
- Organiser des sessions de test avec les utilisateurs finaux
- Capturer des screenshots/vidéos si nécessaire
- Documenter tous les retours

---

### FSOP-021 : Documentation technique et utilisateur

**Type :** Task  
**Priorité :** Medium  
**Sprint :** Sprint 7  
**Étiquettes :** documentation, knowledge

**Description :**
Rédiger la documentation technique (API, architecture, configuration) et le guide utilisateur pour les opérateurs, garantissant la maintenabilité et la facilité d'utilisation.

**Critères d'acceptation :**
- [ ] Documentation technique complète :
  - [ ] API REST (endpoints, paramètres, réponses)
  - [ ] Architecture détaillée (diagrammes)
  - [ ] Configuration (variables d'environnement, fichiers config)
  - [ ] Structure de la base de données (si applicable)
- [ ] Guide utilisateur opérateur :
  - [ ] Instructions pas à pas avec captures d'écran
  - [ ] Comment générer un FSOP
  - [ ] Comment remplir un formulaire
  - [ ] Comment valider les étapes
  - [ ] Comment exporter
  - [ ] FAQ et résolution de problèmes courants
- [ ] Documentation de configuration Silog :
  - [ ] Méthode de connexion
  - [ ] Paramètres nécessaires
  - [ ] Mapping des données
  - [ ] Dépannage
- [ ] README maintenu à jour avec :
  - [ ] Instructions d'installation
  - [ ] Instructions de démarrage
  - [ ] Liens vers autres documentations
- [ ] Documentation des scripts et commandes utiles

**Fichiers créés :**
- `docs/API.md`
- `docs/ARCHITECTURE.md` (déjà créé, à enrichir)
- `docs/GUIDE_UTILISATEUR.md`
- `docs/CONFIG_SILOG.md`
- `README.md` (déjà créé, à enrichir)

**Notes techniques :**
- Utiliser des diagrammes pour l'architecture
- Inclure des exemples de code si pertinent
- Rédiger de manière claire pour non-techniques (guide utilisateur)

---

### FSOP-022 : Déploiement en production

**Type :** Task  
**Priorité :** High  
**Sprint :** Sprint 7  
**Étiquettes :** deployment, production, infrastructure  
**Dépend de :** FSOP-019, FSOP-021

**Description :**
Configurer l'environnement de production et déployer l'application avec toutes les configurations nécessaires (Silog, SQL Server, réseau X:/Traçabilité).

**Critères d'acceptation :**
- [ ] Environnement de production configuré :
  - [ ] Serveur web (Node.js ou Python)
  - [ ] Serveur de base de données SQL Server
  - [ ] Configuration des variables d'environnement production
- [ ] Application déployée et accessible :
  - [ ] Backend API accessible
  - [ ] Frontend accessible (navigateur)
  - [ ] Tests de connectivité réussis
- [ ] Base de données configurée en production :
  - [ ] Connexion SQL Server opérationnelle
  - [ ] Tables créées si nécessaire
  - [ ] Permissions configurées
- [ ] Accès réseau X:/Traçabilité configuré en production :
  - [ ] Dossier accessible depuis le serveur
  - [ ] Permissions de lecture configurées
  - [ ] Tests d'accès réussis
- [ ] Connexion Silog opérationnelle en production :
  - [ ] Connexion testée et validée
  - [ ] Extraction de données testée
- [ ] Sécurité mise en place :
  - [ ] HTTPS configuré
  - [ ] Authentification si nécessaire
  - [ ] Firewall configuré
- [ ] Tests de régression en production réussis :
  - [ ] Génération FSOP testée
  - [ ] Remplissage formulaire testé
  - [ ] Export testé
- [ ] Monitoring et logs configurés :
  - [ ] Logs applicatifs
  - [ ] Monitoring des erreurs
  - [ ] Alertes si nécessaire
- [ ] Procédure de déploiement documentée :
  - [ ] Étapes de déploiement
  - [ ] Rollback en cas de problème
  - [ ] Maintenance

**Fichiers créés :**
- `docs/DEPLOYMENT.md`
- Configuration production dans `config/production/`
- Scripts de déploiement si nécessaire
- `docker-compose.yml` (optionnel)

**Notes techniques :**
- Prévoir un environnement de staging avant production
- Documenter la procédure de rollback
- Mettre en place un monitoring

---

## Récapitulatif des tickets par Sprint

**Sprint 1 :** FSOP-001, FSOP-002, FSOP-003, FSOP-004  
**Sprint 2 :** FSOP-005, FSOP-006, FSOP-007, FSOP-008, FSOP-009  
**Sprint 3 :** FSOP-010, FSOP-011, FSOP-012  
**Sprint 4 :** FSOP-013, FSOP-014, FSOP-015  
**Sprint 5 :** FSOP-016, FSOP-017, FSOP-018  
**Sprint 6 :** FSOP-019, FSOP-020  
**Sprint 7 :** FSOP-021, FSOP-022

---

## Dépendances critiques

- **FSOP-002** bloque : FSOP-007, FSOP-009
- **FSOP-004** bloque : FSOP-009
- **FSOP-006** bloque : FSOP-007, FSOP-008, FSOP-009
- **FSOP-007 et FSOP-009** bloquent : FSOP-010, FSOP-016
- **FSOP-011** bloque : FSOP-012
- **FSOP-003 et FSOP-012** bloquent : FSOP-014
- **FSOP-012** bloque : FSOP-013, FSOP-015, FSOP-017

---

## Notes d'import dans Jira

Pour importer ces tickets dans Jira :
1. Créer les ÉPICs correspondants (ÉPIC 1 à 5)
2. Créer les tickets dans l'ordre des Sprints
3. Lier les dépendances ("Blocante pour", "Dépend de")
4. Ajouter les étiquettes (labels)
5. Assigner les priorités
6. Configurer les workflows selon vos processus

