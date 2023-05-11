# RMV - R for MAIA and VMS

# VMS_EDIT_RAW()
Cette première fonction permet de faire une première transformation des données brute telles
qu’ils sont décrite dans la table 1.1 à un format texte plus simple avec une extension .vms. Ce
fichier ne garde que les informations les plus pertinentes telles que le matricule du navire, sa
position géographique décrite par la latitude et longitude en degré décimal, la date et heure
sous sa forme numérique, la vitesse du navire et la route.


# VMS_CREATE_DB()
Cette fonction prend en entrée le fichier .vms généré dans l’étape précédente et crée une base
de données SQLite en utilisant le package de R, RSQLite. Cette base de données contient en
plus de la table ping des points VMS, un ensemble de table qui seront alimentées au fur et à
mesure.


# VMS_CLEAN_DB()
Comme son nom l’indique, cette fonction permet de nettoyer les points VMS. Cette fonction
prendra en entrée la base de données générée, un shapeFile de la côte et un shapeFile des 
positions des ports de débarquement. Cette combinaison permet de détecter les points sur terre
et les points dans le port. Cette fonction permet aussi de détecter les incohérences entre les
points VMS en analysant les profils de vitesse de chaque navire.

# VMS_CUT_TRACKS()
Une étape très importante pour faire la jointure entre MAIA et VMS est de pouvoir découper
les points VMS en marées. Cette fonction utilise les points déjà nettoyer et une distance qui
représente la distance à partir de laquelle le navire se dirige généralement vers le port pour
déterminer le début et la fin d’une marée. Tenant compte de la fréquence des signaux, l’analyse
de de profils de vitesse permet de savoir si le navire est allé vers le port avant d’arriver au point
suivant ou non.


# VMS_INTERPOLATE_TRACKS()
Après détermination des marées, cette fonction permet en utilisant une méthode basée sur une
technique d’interpolation spline, la spline Hermite cubique (cHs), utilisant la position, le cap
et la vitesse pour interpoler la trajectoire du chalut d’un navire entre deux points de données
VMS successifs.


# VMS_ASSIGN_BATHY()
Cette fonction permet en se connectant à une base de données de la NOAA, de lier chaque
point d’interpolation à une bathymétrie qui atteint une résolution minimale de 1 mile nautique.


# VMS_ASSIGN_FISHING()
Cette fonction permet en analysant les profils de vitesse de chaque navire de déterminer si le
navire est en mode de navigation ou bien il est en pleine opération de pêche. L’intervalle de
vitesse de pêche est déterminé par les observateurs de l’INRH et à partir des déclarations dans
les journaux de pêche des navires.

# MAIA_EDIT_RAW()
Après traitement des données VMS. Il est temps de faire la même chose pour les données
MAIA, mais avec moins d’étapes. Cette première fonction permet de produire un fichier texte
avec les informations d’intérêt telles que le matricule, la date de début et de fin d’une marée
inférée à partir du découpage des données VMS en marées, l’espèce, et la quantité.

# MAIA_CREATE_DB()
Cette fonction permet de créer une base de données SQLite avec la table MAIA et la table de
jointure qui sera alimentée par la suite.


# MAIA_VMS_JOIN()
Cette fonction permet de faire le lien entre les deux table MAIA et la table d’interpolation de
VMS. Les clés de jointure sont les dates de début et de fin de la marée et le matricule du navire.
À la fin de cette jointure, un poids capturés pendant une marée sera lié à un ensemble de points
d’interpolation de VMS avec une vitesse qui permet de les considérer comme faisant partie
d’une opération de pêche.


# INSTALLATION
RMV est publié sur la platforme Github. Toute personne peut l’installer en utilisant la com-
mande : remotes::install_github(repo = ”intelligentica-net/rmv”).

