FROM quay.io/infinispan/server:14.0

COPY /org.jboss.marshalling.jboss-marshalling-osgi-2.1.1.Final.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-admin-ui-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-authz-policy-common-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-common-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-config-api-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-core-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-crypto-default-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-crypto-fips1402-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-js-adapter-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-kerberos-federation-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-ldap-federation-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-infinispan-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-jpa-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-legacy-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-legacy-private-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-legacy-services-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-map-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-map-file-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-map-hot-rod-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-model-map-jpa-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-rest-admin-ui-ext-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-saml-core-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-saml-core-public-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-server-spi-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-server-spi-private-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-services-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-sssd-federation-21.0.1.jar /opt/infinispan/server/lib
COPY /org.keycloak.keycloak-themes-21.0.1.jar /opt/infinispan/server/lib

COPY /common.sh /opt/infinispan/bin

USER 0

RUN true \
  && microdnf clean all \
  && microdnf install shadow-utils \
  && microdnf update --nodocs \
  && adduser ispn \
  && microdnf remove shadow-utils \
  && microdnf clean all

RUN chown -R ispn:0 /opt/infinispan

RUN curl https://jdbc.postgresql.org/download/postgresql-42.5.2.jar --output /opt/infinispan/lib/postgresql-42.5.2.jar

USER ispn

CMD [ "-c", "infinispan-keycloak.xml" ]
ENTRYPOINT [ "/opt/infinispan/bin/server.sh" ]