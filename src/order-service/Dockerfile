FROM nginx:alpine

LABEL service="order-service"
LABEL maintainer="majid1@live.co.uk"

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
