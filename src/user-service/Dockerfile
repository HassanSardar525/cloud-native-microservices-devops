FROM nginx:alpine

LABEL service="user-service"
LABEL maintainer="majid1@live.co.uk"

COPY index.html /usr/share/nginx/html/index.html
COPY style.css /usr/share/nginx/html/style.css

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
