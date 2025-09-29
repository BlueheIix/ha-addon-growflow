ARG BUILD_FROM
FROM node:20-alpine AS build
RUN apk add --no-cache git
ARG GROWFLOW_REF=main
WORKDIR /usr/src
RUN git clone --depth 1 -b ${GROWFLOW_REF} https://github.com/grow-flow/growflow.git
WORKDIR /usr/src/growflow
RUN npm install && cd backend && npm install && cd ../frontend && npm install
ENV PUBLIC_URL=.
RUN npm run build || (cd backend && npm run build && cd ../frontend && npm run build)

FROM ${BUILD_FROM}
RUN apk add --no-cache nodejs npm
WORKDIR /opt/growflow
COPY --from=build /usr/src/growflow /opt/growflow
COPY rootfs/ /
EXPOSE 8080
ENV NODE_ENV=production \
    DB_PATH=/data/growflow.db \
    LOG_LEVEL=info \
    ALLOWED_FRAME_ANCESTORS="'self',*" \
    PUBLIC_URL=.
