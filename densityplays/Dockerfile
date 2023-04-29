FROM node:19

WORKDIR /app

COPY package.json ./
COPY package-lock.json ./
RUN npm install
COPY . ./
RUN npm run build
ENV HOST=0.0.0.0
EXPOSE 4173
CMD ["npm","run","preview","--","--host=0.0.0.0"]