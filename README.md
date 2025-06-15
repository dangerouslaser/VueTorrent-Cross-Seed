# Vuetorrent with Cross-Seed functionality.

You need to use a caddy file for this to work due to CORS functionality with cross-seed.

Example below

<code>
:{$CADDY_PORT:-8081} {
  reverse_proxy {$CROSS_SEED_HOST:-cross-seed}:{$CROSS_SEED_PORT:-2468}

  header Access-Control-Allow-Origin "*"
  header Access-Control-Allow-Methods "GET, POST, OPTIONS"
  header Access-Control-Allow-Headers "Content-Type, Authorization"

  @preflight {
    method OPTIONS
  }

  handle @preflight {
    respond "" 204
  }
}
</code>

Docker Compose for caddy

<code>
  caddy:
    image: caddy:alpine
    container_name: caddy
    ports:
      - "8081:8081"
    volumes:
      - ./cross-seed-caddy:/etc/caddy/Caddyfile
    depends_on:
      - cross-seed
    environment:
      CADDY_PORT: 8081
      CROSS_SEED_HOST: cross-seed
      CROSS_SEED_PORT: 2468
</code>
