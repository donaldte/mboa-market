# À FAIRE (étape 4, P2-04) : l'image du site.
#   FROM nginx:1.30-alpine
#   nginx.conf (à côté) devient /etc/nginx/conf.d/default.conf
#   le dossier site/ va dans /usr/share/nginx/html/
#   un HEALTHCHECK toutes les 10 s : wget -q --spider http://127.0.0.1/ || exit 1

FROM nginx:1.30-alpine 
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY site/ /usr/share/nginx/html/
HEALTHCHECK --interval=10s --timeout=3s --start-period=5s --retries=3 \ 
	CMD wget -q --spider http://127.0.0.1/ || exit 1 
