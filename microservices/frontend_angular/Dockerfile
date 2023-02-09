FROM node:18.14-alpine AS build
WORKDIR /opt/src

# Copy all files (except those mentioned in .dockerignore) into the working directory
COPY . /opt/src

# Run post install script without risk of leaking the auth token
RUN npm rebuild && npm run prepare --if-present

# Run the build command specified in the `package.json`. By default, this is a production build
RUN npm run build

# Create another stage which is strictly nginx
FROM nginx:latest AS nginx

# Copy the application build from the previous stage and put it in nginx's default serve folder
COPY --from=build /opt/src/dist/frontend_angular /usr/share/nginx/html
# Copy the `nginx.conf` from our working directory as the default configuration for nginx
COPY --from=build /opt/src/nginx.conf  /etc/nginx/conf.d/default.conf

ENTRYPOINT ["nginx", "-g", "daemon off;"]