FROM node:alpine As development

WORKDIR /app

COPY package*.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm

RUN pnpm fetch --prod

RUN pnpm install

COPY . .

RUN pnpm run build

FROM node:alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /app

COPY --chown=node:node package*.json ./
COPY --chown=node:node pnpm-lock.yaml ./

COPY --chown=node:node --from=development /app/node_modules ./node_modules

RUN npm install -g pnpm

RUN pnpm fetch --prod

RUN pnpm install --prod

COPY --from=development /app/dist ./dist

CMD ["node", "dist/apps/payments/main.js"]