FROM nginx
RUN \
apt-get update && \
apt-get upgrade && \
rm /etc/nginx/conf.d/default.conf && \
mkdir -p /homework
COPY index.html /homework/ 
COPY home.conf /etc/nginx/conf.d/
EXPOSE 8000
RUN service nginx start
CMD ["nginx", "-g", "daemon off;"]

