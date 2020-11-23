# Dockerfile for building docs is stored in a separate dir from the docs,
# otherwise the generated site will unnecessarily contain the Dockerfile

FROM python:3.9-alpine
LABEL maintainer="The M3DB Authors <m3db@googlegroups.com>"

WORKDIR /m3db
EXPOSE 8000

# mkdocs needs git-fast-import which was stripped from the default git package
# by default to reduce size
RUN pip install -i http://mirrors.aliyun.com/pypi/simple/  --trusted-host mirrors.aliyun.com  \
    mkdocs==0.17.3 \
    pymdown-extensions==6.0 \
    mkdocs-material==2.7.3

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
RUN apk update

RUN apk add --no-cache git-fast-import openssh-client
ENTRYPOINT [ "/bin/ash", "-c" ]
