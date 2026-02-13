# selfsudoc-docker
Configuration pour le déploiement docker de l'application [selfsudoc](https://github.com/abes-esr/selfsudoc)

## Prérequis
Disposer de :
- ``docker``
- ``docker compose``

## Restauration

### Restauration de la base de données Oracle

- se connecter avec le compte oracle sur ononis (machine Oracle) :

- Récupérer la sauvegarde depuis sotora :
```bash
rsync --progress -av devel@socorro.v104.abes.fr:/backup_pool/ononis-prod-dumps/daily.0/racine/backup-sql/ABES/PRODUITSDERIVES/dumpPRODUITS_DERIVES.dmp /backup-sql/ABES/PRODUITSDERIVES/dump.dmp
```
*Pour sélectionner une sauvegarde autre que la plus récente, il suffit de remplacer daily.0 dans la commande par le jour souhaité (daily.1 pour la veille, daily.2 pour l'avant-veille, etc.)*

- Puis lancer les commandes suivantes pour importer le dump dans la base de données :

```bash
export NLS_LANG=AMERICAN_AMERICA.UTF8
export ORACLE_SID='ABES'
setsid impdp \'/ as sysdba\' SCHEMAS=PRODUITS_DERIVES TABLE_EXISTS_ACTION=REPLACE dumpfile='dump.dmp' logfile=importProduitsDerives.log directory=DPDUMP_PRODUITSDERIVES
```
Si le directory object n'existe pas dans la base de données, il doit être ajouté via :
```bash
CREATE DIRECTORY DPDUMP_PRODUITSDERIVES AS '/backup-sql/ABES/PRODUITSDERIVES';
```
_Procédure executée avec succès sur le test le 08/12/2025_
