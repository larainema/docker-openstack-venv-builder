FROM ghcr.io/vexxhost/ubuntu-cloud-archive:caracal
# renovate: name=openstack/requirements repo=https://opendev.org/openstack/requirements.git branch=stable/2024.1
ARG REQUIREMENTS_GIT_REF=c1cde0f897da8c355d03eacda5cf3b610fe75e2c
ADD https://opendev.org/openstack/requirements/raw/commit/${REQUIREMENTS_GIT_REF}/upper-constraints.txt /upper-constraints.txt
RUN <<EOF sh -xe
sed -i '/glance-store/d' /upper-constraints.txt
sed -i '/horizon/d' /upper-constraints.txt
EOF

FROM ghcr.io/vexxhost/python-base:caracal
RUN \
  --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        git \
        libldap2-dev \
        libpcre3-dev \
        libsasl2-dev \
        libssl-dev \
        lsb-release \
        openssh-client \
        python3 \
        python3-dev \
        python3-pip \
        python3-venv
RUN python3 -m venv --upgrade-deps --system-site-packages /var/lib/openstack
COPY --from=requirements --link /upper-constraints.txt /upper-constraints.txt
RUN pip3 install \
    --constraint /upper-constraints.txt \
        cryptography \
        pymysql \
        python-binary-memcached \
        python-memcached \
        uwsgi
