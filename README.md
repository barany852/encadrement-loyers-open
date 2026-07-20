# Encadrement des loyers — données ouvertes (France)

Une **agrégation ouverte et réutilisable** des barèmes d'encadrement des loyers
(loyer de référence, majoré, minoré) des villes françaises concernées, dans des
formats simples à réutiliser : **CSV**, **JSON**, **GeoJSON**.

L'encadrement des loyers existe dans plusieurs agglomérations, mais les données sont
publiées de façon **hétérogène** (un portail open data par-ci, un simple PDF d'arrêté
par-là) et il n'existe pas de jeu unifié. Ce dépôt rassemble, quand la source le permet,
ces barèmes sous une forme homogène.

> ⚠️ **Ce jeu de données n'est PAS officiel.** La seule référence juridiquement
> opposable est l'**arrêté préfectoral** de chaque agglomération. Ces fichiers sont une
> compilation de données publiques, fournie à titre informatif, **sans garantie**
> d'exactitude ni d'exhaustivité. Vérifiez toujours auprès de la source officielle
> (colonne `source` de chaque ligne) avant tout usage engageant.

C'est un travail modeste : on ne remplace pas les administrations, on rend simplement
réutilisable une donnée publique qui ne l'était pas partout. Merci aux collectivités qui
publient ces données en open data.

## Couverture actuelle

| Agglomération | Millésime | Secteurs/zones | Barèmes | Source |
|---|---|---|---|---|
| **Paris** | 2025 | 80 | 2 560 | [opendata.paris.fr](https://opendata.paris.fr/explore/dataset/logement-encadrement-des-loyers/) |
| **Métropole de Lyon** (Lyon + Villeurbanne) | 2025-2026 | 233 | 9 320 | [data.grandlyon.com](https://data.grandlyon.com/portail/fr/jeux-de-donnees/encadrement-des-loyers-de-la-metropole-de-lyon-2025-2026/info) |
| **Montpellier** | 2026 | 93 | 3 720 | Arrêté préfectoral DDTM34-2026-06-17135 (11/06/2026) ; géométrie IRIS [IGN](https://geoservices.ign.fr/) |
| **Est Ensemble** (93 : Montreuil, Bagnolet, Bondy, Bobigny, Pantin, Noisy-le-Sec, Romainville, Les Lilas, Le Pré-Saint-Gervais) | 2026 | 10 | 320 | [DRIHL Île-de-France](http://www.referenceloyer.drihl.ile-de-france.developpement-durable.gouv.fr/) (période 01/06/2026→31/05/2027) |
| **Plaine Commune** (93 : Saint-Denis, Aubervilliers, Épinay-sur-Seine, La Courneuve, L'Île-Saint-Denis, Pierrefitte-sur-Seine, Saint-Ouen-sur-Seine, Stains, Villetaneuse) | 2026 | 10 | 320 | [DRIHL Île-de-France](http://www.referenceloyer.drihl.ile-de-france.developpement-durable.gouv.fr/) (période 01/06/2026→31/05/2027) |
| **Total** | | **426** | **16 240** | |

D'autres villes (Lille, Bordeaux, EPT du 93, Pays Basque…) sont concernées
par le dispositif ; elles seront ajoutées au fur et à mesure que des sources fiables et à
jour sont disponibles (certaines ne publient qu'un PDF d'arrêté, à intégrer manuellement).

## Fichiers

- **`data/baremes.csv`** / **`data/baremes.json`** — le barème, une ligne par
  (secteur × nombre de pièces × époque de construction × meublé/nu).
- **`data/zones.geojson`** — la géométrie des secteurs (polygones) pour localiser une
  adresse dans le bon secteur (point-dans-polygone).

### Schéma `baremes`

| Colonne | Description |
|---|---|
| `commune` | Nom de la commune / arrondissement |
| `insee_code` | Code INSEE de la commune/arrondissement |
| `secteur_id` | Identifiant du secteur/quartier/IRIS (selon la source), clé de jointure avec `zones.geojson` (`properties.secteur_id`) |
| `label` | Libellé lisible du secteur |
| `rooms` | Nombre de pièces (`1`, `2`, `3`, `4` = « 4 et plus ») |
| `epoch` | Époque de construction : `<1946`, `1946-1970`, `1971-1990`, `>1990`, `1991-2005`, `>2005` (selon la ville) |
| `furnished` | Meublé (`true`) ou nu (`false`) |
| `ref_minore` | Loyer de référence minoré (€/m²/mois, hors charges) |
| `ref_median` | Loyer de référence (médian) |
| `ref_majore` | **Loyer de référence majoré = le plafond légal** (€/m²/mois, hors charges) |
| `year` | Millésime du barème |
| `source` | Source officielle + licence, tracée par ligne |

## Licence

Les données proviennent de sources publiques diffusées sous **Licence Ouverte / Open
Licence 2.0 (Etalab)** ; les arrêtés préfectoraux sont des actes publics librement
reproductibles. Cette compilation est redistribuée sous la même **[Licence Ouverte 2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence/)**.
Attribution demandée : *Ville de Paris*, *Métropole de Lyon*, *Préfecture de l'Hérault (DDTM34)*, *DRIHL Île-de-France*, *IGN*, et ce dépôt.

## Provenance & mises à jour

Compilé à partir des sources officielles listées ci-dessus, normalisé dans un schéma
commun. Chaque ligne porte sa `source` et son `year`. Les villes à source open data
« vive » (Paris, Lyon) sont rafraîchies automatiquement à chaque nouvel arrêté ; les
villes à source PDF nécessitent une reprise manuelle (le millésime `year` indique la
fraîcheur).

Maintenu par [Logio](https://logio.immo) — intelligence immobilière. Signalements et
contributions bienvenus via les *issues*.
