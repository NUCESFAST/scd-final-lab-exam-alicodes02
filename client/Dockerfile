
FROM node:14 as build

WORKDIR /21I-1170_frontend


COPY package*.json ./


RUN npm install


COPY . .


RUN npm run build


FROM nginx:alpine


WORKDIR /usr/share/nginx/html


RUN rm -rf ./*


COPY --from=build /21I-1170_frontend/build .


COPY nginx.conf /etc/nginx/conf.d/default.conf


EXPOSE 1170

CMD ["nginx", "-g", "daemon off;"]
