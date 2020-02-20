FROM alpine/git AS clone
WORKDIR /app
RUN git clone --single-branch --branch multi-stage_build https://github.com/secobau/guide-cloud-openshift.git

FROM maven:alpine AS build
WORKDIR /app
COPY --from=clone /app/guide-cloud-openshift/start /app 
RUN mvn package

FROM open-liberty
COPY --from=build /app/inventory/target/system.tar.gz .
RUN mv system.tar.gz /opt/ol && gunzip /opt/ol/inventory.tar.gz && tar xf /opt/ol/inventory.tar.gz && rm -f /opt/ol/inventory.tar.gz
RUN chown -R 1001:0 /opt/ol/inventory*
