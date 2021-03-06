#FROM kong/kong-gateway:2.8.1.1-alpine
FROM kong/kong:2.7.0

LABEL description="Alpine + Kong 2.0.3 + kong-oidc plugin"

ENV OIDC_PLUGIN_VERSION=1.1.0-0
ENV JWT_PLUGIN_VERSION=1.1.0-1
ENV ECHO_PLUGIN_VERSION=0.1.0-1
ENV CUSTOM_AUTH_PLUGIN_VERSION=0.1.0-1

USER root
RUN apk update && apk add git unzip luarocks
RUN luarocks install kong-oidc

RUN git clone https://github.com/PSheshenya/kong-oidc.git \
    && cd kong-oidc \
    && luarocks make

RUN luarocks pack kong-oidc ${OIDC_PLUGIN_VERSION} \
     && luarocks install kong-oidc-${OIDC_PLUGIN_VERSION}.all.rock

RUN git clone --branch 20200505-access-token-processing https://github.com/BGaunitz/kong-plugin-jwt-keycloak.git \
    && cd kong-plugin-jwt-keycloak \
    && luarocks make

RUN luarocks pack kong-plugin-jwt-keycloak ${JWT_PLUGIN_VERSION} \
     && luarocks install kong-plugin-jwt-keycloak-${JWT_PLUGIN_VERSION}.all.rock

RUN git clone https://github.com/metex/kong-plugin-header-echo.git \
    && cd kong-plugin-header-echo \
    && luarocks make

RUN luarocks pack kong-plugin-header-echo ${ECHO_PLUGIN_VERSION} \
     && luarocks install kong-plugin-header-echo-${ECHO_PLUGIN_VERSION}.all.rock

RUN git clone https://github.com/metex/kong-plugin-custom-auth.git \
    && cd kong-plugin-custom-auth \
    && luarocks make

RUN luarocks pack kong-plugin-custom-auth ${CUSTOM_AUTH_PLUGIN_VERSION} \
     && luarocks install kong-plugin-custom-auth-${CUSTOM_AUTH_PLUGIN_VERSION}.all.rock

USER kong
