FROM node:16-alpine3.16 AS builder
ARG ENV
WORKDIR /ng-app
COPY . .
RUN echo n|npm install 

RUN if [ "$ENV" = "dev" ]; then npm run dev; \
    elif [ "$ENV" = "test" ];then npm run test; \
    elif [ "$ENV" = "prod" ]; then npm run production; fi

FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /ng-app/dist/node /usr/share/nginx/html

RUN mkdir -p /var/run && \
    touch /run/nginx.pid && \
    chown -R nginx:nginx /var/cache/nginx && \
    chown  nginx:nginx /var/run/nginx.pid && \
    chmod -R 755 /var/cache/nginx && \
    chmod -R 755 /var/run/nginx.pid
USER nginx
EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
