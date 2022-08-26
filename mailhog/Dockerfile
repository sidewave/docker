FROM debian:11-slim

LABEL authors="Michael Pavlista <pavlista@sidewave.io>"

ARG TARGETPLATFORM

RUN \
    apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates wget && \
    bash -c '[ "$TARGETPLATFORM" = "linux/amd64" ] && wget -O MailHog_linux https://github.com/mailhog/MailHog/releases/latest/download/MailHog_linux_amd64 || exit 0' && \
    bash -c '[ "$TARGETPLATFORM" = "linux/arm64" ] && wget -O MailHog_linux https://github.com/evertiro/MailHog/releases/download/v1.0.1-M1/MailHog_linux_arm64 || exit 0' && \
    \
    mv MailHog_linux /usr/local/bin/MailHog && \
    chmod +x /usr/local/bin/MailHog

CMD /usr/local/bin/MailHog

EXPOSE 1025 8025
