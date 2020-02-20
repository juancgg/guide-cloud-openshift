FROM alpine/git AS clone
WORKDIR /app
RUN git clone --single-branch --branch multi-stage_build https://github.com/secobau/guide-cloud-openshift.git

FROM maven:alpine AS build
WORKDIR /app
COPY --from=clone /app/guide-cloud-openshift/start/pom.xml /app/
COPY --from=clone /app/guide-cloud-openshift/start/system /app
RUN mvn package

FROM open-liberty
COPY --from=build /app/system/target/system.tar.gz /opt/ol
USER root
RUN gunzip /opt/ol/system.tar.gz && tar -C /opt/ol -xf /opt/ol/system.tar && rm -f /opt/ol/system.tar
RUN chown -R 1001:0 /opt/ol/*
