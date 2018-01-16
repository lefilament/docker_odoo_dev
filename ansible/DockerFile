FROM alpine:3.7

MAINTAINER Remi - Le Filament <www.le-filament.com>

WORKDIR /root/ansible
CMD [ "ansible-ansible", "--version" ]

ARG GIT_REPO=https://github.com/remi-filament/ansible
ARG GIT_EMAIL=test@example.com
ENV GIT_REPO="$GIT_REPO" \
    GIT_EMAIL="$GIT_EMAIL"

COPY .vault.txt /root/.vault.txt
COPY ssh /root/.ssh

RUN apk update \
    && apk add ansible git openssh \
    && rm -rf /tmp/* \
    && rm -rf /var/cache/apk/* \
    && rm -rf /var/lib/apt/lists/* \
	&& chmod -R u=rwX,go= /root/.ssh \
	&& chmod -x /root/.vault.txt \
	&& git clone --depth 1 $GIT_REPO /root/ansible \
    && cd /root/ansible \
	&& ansible -i inventory.inv Server1 -c local -m copy -a "dest=/root/.ssh/id_rsa content='{{ ansible_depl_ssh_key }}' mode=0600" \
	&& git config user.email $GIT_EMAIL

# Metadata
ARG VCS_REF
ARG BUILD_DATE
LABEL org.label-schema.schema-version="1.0" \
      org.label-schema.vendor=Le-Filament \
      org.label-schema.build-date="$BUILD_DATE" \
      org.label-schema.vcs-ref="$VCS_REF"
