FROM node:20-bookworm-slim AS build

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /app/frontend

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    make \
    g++ \
    git \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

COPY frontend/package*.json ./

RUN npm install --legacy-peer-deps

COPY frontend/ .

ARG REACT_APP_API_URL
ENV REACT_APP_API_URL=$REACT_APP_API_URL
ENV CI=false

RUN npm run build

FROM nginx:1.27-alpine

COPY --from=build /app/frontend/build /usr/share/nginx/html
COPY frontend/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]