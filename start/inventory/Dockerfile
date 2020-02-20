FROM alpine/git AS clone
WORKDIR /app
RUN git clone --single-branch --branch multi-stage_build https://github.com/secobau/guide-cloud-openshift.git

FROM maven:alpine AS build
WORKDIR /app
COPY --from=clone /app/guide-cloud-openshift/start /app 
RUN mvn package

FROM open-liberty
ADD --chown=1001:0 target/inventory.tar.gz /opt/ol
