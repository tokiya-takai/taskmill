# --------------------------------
# マルチステージビルド
# --------------------------------
FROM node:18.20.4-slim AS builder

RUN npm i -g pnpm

WORKDIR /build

COPY package.json pnpm-lock.yaml ./

RUN pnpm install

COPY . .

RUN pnpm run build

# --------------------------------
# 実行ステージ
# --------------------------------
FROM node:18.20.4-slim

RUN npm i -g pnpm

WORKDIR /var/www/html

COPY --from=builder /build/node_modules ./
COPY --from=builder /build/public ./public
COPY --from=builder /build/.next ./.next

COPY --from=builder /build/package.json ./
COPY --from=builder /build/pnpm-lock.yaml ./
COPY --from=builder /build/next.config.mjs ./

EXPOSE 3000

ENTRYPOINT ["pnpm", "dev"]
