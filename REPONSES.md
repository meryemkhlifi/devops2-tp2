# Réponses aux questions de TP2 
## Q1 :
les variables attendu par Strapi :
DATABASE_CLIENT : postgres
DATABASE_HOST : postgres
DATABASE_PORT : 5432
DATABASE_NAME : ${POSTGRES_DB}
DATABASE_USERNAME : ${POSTGRES_USER}
DATABASE_PASSWORD : ${POSTGRES_PASSWORD}

## Q2 :
.env .example est le modèle a suivre contenant des valeurs fictives 
car .env contient des secrets qu'on peut pas commiter alors en commite env .example pour la documentation

## Q3 :
docker compse down vs docker compose down -v 
**down** supprime les conteneurs mais conserve les volumes
**down -v** supprime aussi les volumes

## Q4 :

$$ veut dire que dans compse le text doit etre envoyé vers le conteneur ou le shell va le remplacer par sa valeur

## Q5 :

 - **"Host": "postgres"** : pgAdmin tourne dans son propre conteneur
 - **:ro** : read only ; le conteneur lit **servers.json** mais ne le modifie pas
 - **${PGADMIN_PORT:-5050}** : prend la valeur de  PGADMIN_PORT sinoo 5050 par défaut
 
## Q6 :

On copie package*.json et puis en lance npm ci car tant que ces fichier ne change pas l'installation des dépendances est reprise du cache ou lieu de ce relancer à chaque build

## Q7 :
**USER node**  : le processus tourne avec un utilisateur **non root**. Si l'application est compromise, l'attaquant a moins de privilèges
**--chown=node:node** : **COPY --from**  crée les fichiers en **root ** par défaut. Le **chown**  donne la propriété à l'utilisateur **node **, qui peut alors lire et écrire ce dont Strapi a besoin

## Q8 :
Compose attend que le healthcheck de  soit **healthy** avant de démarrer strapi. Sans cette condition, Compose attend seulement que le conteneur soit démarré

## Q9 :

on trouve en général **2 lignes** pour un article publié : une version brouillon (published_at = NULL) et une version publiée (published_at  renseigné), qui partagent le même  document_id

## Q10 :
localhost est le conteneur frontend ici mais rien n'écoute sur le port 1337 , il faut alors utiliser le nom du service  http://strapi:1337

## Q11 :
Étapes du Dockerfile: 
FROM:.... (installation des dépendances)
npm run build : Builder
runner : (exécution)

 -L'image finale contient seulement l'étape runner : node:22-alpine, public/, .next/standalone
**restart: unless-stopped** : Docker redémarre automatiquement le conteneur s’il plante, ou quand Docker/la machine redémarre, **sauf si on l’a arrêté nous-mêmes**.## Q14 :

1/**Secrets forts** : générer de nouveaux secrets uniques (openssl rand -base64 16`) au lieu des valeurs d’exemple, et ne jamais les commiter.
2/  **Réduire l’exposition** : ne pas publier pgAdmin (ou le limiter par pare-feu/VPN) et mettre un reverse proxy HTTPS devant l’application.
## Q13 :

2/ Sans ports:, Postgres n'est accessible que depuis les conteneurs du réseau backend, pas depuis la machine hôte ni depuis l'extérieur. Cela réduit la surface d'attaque.
 
1/ strapi joue le rôle de passerelle : il doit joindre postgres (réseau backend) et être joignable par frontend (réseau frontend).
 - Sans output: "standalone", Next.js ne génère pas .next/standalone : le COPY --from=builder /app/.next/standalone ./ échoue (not found) et le build s'arrête.

