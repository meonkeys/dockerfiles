FROM node:0.12

# see https://github.com/vatesfr/xo/blob/a269af702992616f67b7bcf066eac0a1124ac8bf/doc/installation/manual_installation.md

#RUN apt-get update && apt-get install -y ruby-compass

WORKDIR /usr/src/xo/server

ENV XO_REF v4.7.0

RUN git clone -b "$XO_REF" --single-branch git://github.com/vatesfr/xo-server.git .
RUN git clone -b "$XO_REF" --single-branch git://github.com/vatesfr/xo-web.git ../web

RUN set -x \
	&& npm install \
	&& npm run build

RUN set -x \
	&& cd ../web \
	&& npm install \
	&& npm run build

RUN sed " \
		s!#'/': '/path/to/xo-web/dist/'!'/': '../web/dist/'!; \
		s!#uri: ''!uri: 'tcp://redis:6379'!; \
	" sample.config.yaml >.xo-server.yaml

EXPOSE 80
CMD ["npm", "start"]
