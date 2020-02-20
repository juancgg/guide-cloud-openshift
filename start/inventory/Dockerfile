FROM alpine/git AS clone
WORKDIR /app
RUN git clone --single-branch --branch multi-stage_build https://github.com/secobau/guide-cloud-openshift.git

FROM maven:alpine AS build
WORKDIR /app
COPY --from=clone /app/guide-cloud-openshift/start /app
WORKDIR /app/inventory
RUN mvn package

FROM open-liberty
COPY --from=build /app/inventory/target/inventory.tar.gz /opt/ol
USER root
RUN gunzip /opt/ol/inventory.tar.gz && tar -C /opt/ol -xf /opt/ol/inventory.tar && rm -f /opt/ol/inventory.tar
RUN chown -R 1001:0 /opt/ol/*
