FROM node:carbon

WORKDIR /var/www/webspider

RUN npm install -g nodemon

RUN npm install
# Используется при сборке кода в продакшене
# RUN npm install --only=production

# сборка проекта
# RUN npm run build

EXPOSE 3086
CMD [ "npm", "run", "start:debug" ]