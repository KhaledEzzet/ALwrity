FROM node:20-bookworm-slim

WORKDIR /app/frontend

RUN apt-get update && apt-get install -y \
    python3 \
    make \
    g++ \
    git \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

COPY frontend/package*.json ./

RUN npm install --legacy-peer-deps

COPY frontend/ .

ENV HOST=0.0.0.0
ENV PORT=3000
ENV CI=false

EXPOSE 3000

CMD ["npm", "start"]