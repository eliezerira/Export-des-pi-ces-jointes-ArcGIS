# Export-des-pièces-jointes-ArcGIS

Toolbox ArcGIS Pro (`.atbx`) permettant d'exporter en masse les pièces jointes (photos, documents, etc.) stockées dans une table d'attachements de géodatabase vers des fichiers sur disque.
 
## Contenu de la toolbox
 
La toolbox `downldPJ` contient deux outils :
 
### `ExportAttachments2` (recommandé)
 
Outil complet avec script Python embarqué, fonctionnel et autonome.
 
**Paramètres :**
 
| Paramètre | Nom interne | Type | Description |
|---|---|---|---|
| Table des pièces jointes | `pjtable` | Table View | Table d'attachements en entrée |
| Dossier de destination | `mondossier` | Dossier | Dossier où seront écrits les fichiers extraits |
| Champ de nommage | `nomphoto` | Champ | Champ de la table utilisé pour construire le nom des fichiers exportés (dépend de la table choisie) |
 
**Fonctionnement :**
 
1. Crée le dossier de destination s'il n'existe pas.
2. Parcourt la table d'attachements avec un `SearchCursor` sur les champs `DATA` (données binaires), `ATT_NAME` (nom original de la pièce jointe) et le champ de nommage choisi par l'utilisateur.
3. Pour chaque enregistrement, reconstruit un nom de fichier au format :
```
   <valeur_du_champ_de_nommage>_<nom_original_de_la_pièce_jointe>
```
4. Écrit les données binaires dans un fichier à cet emplacement.
5. Journalise chaque fichier sauvegardé dans les messages de l'outil (`arcpy.AddMessage`), et journalise les erreurs éventuelles (`arcpy.AddError`).
**Prérequis :** ArcGIS Pro 3.x (testé avec `app_ver: 13.0`), module `arcpy`.
 
## Utilisation
 
1. Ouvrir la toolbox `downldPJ.atbx` dans ArcGIS Pro (Catalogue → Ajouter une boîte à outils).
2. Lancer l'outil **ExportAttachments2**.
3. Renseigner :
   - la table d'attachements (généralement nommée `<nom_de_la_couche>__ATTACH`),
   - le dossier de destination,
   - le champ à utiliser pour nommer les fichiers exportés (ex. un identifiant terrain).
4. Exécuter : les fichiers sont extraits et nommés automatiquement.
## Structure du dépôt
 
```
downldPJ.atbx
├── toolbox.content                          # Métadonnées de la toolbox
├── ExportAttachments.tool/                  # Outil legacy (script externe manquant)
└── ExportAttachments2.tool/
    └── tool.script.execute.py               # Script Python de l'outil fonctionnel
```
 
## Cas d'usage
 
Extraction en masse de photos ou documents de terrain rattachés à des entités dans une géodatabase (par exemple des relevés SIG), avec renommage automatique selon un attribut métier (identifiant de point, de dossier, etc.).
