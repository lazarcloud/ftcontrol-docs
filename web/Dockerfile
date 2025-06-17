FROM oven/bun:latest AS build

WORKDIR /app

RUN apt-get update && apt-get install -y git

COPY ./web/package.json ./web/package.json
COPY ./web/bun.lock ./web/bun.lock

COPY ./web ./web

RUN cd web && bun install

COPY ./test-codebase ./test-codebase

COPY ./library ./library

COPY ./ ./

RUN cd web && bun run build

# CMD ["sleep", "infinity"]

FROM busybox:latest AS runtime
COPY --from=build /app/web/dist /www
EXPOSE 80
CMD ["httpd", "-f", "-v", "-p", "80", "-h", "/www"]
