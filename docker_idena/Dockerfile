FROM ubuntu:22.04

ENV IDENA_VERSION 1.1.2
ENV IDENA_ARCH aarch64

WORKDIR /idena

# Add config file
ADD idena-conf.json /idena

# Install curl, tar, and git
RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y tar
RUN apt-get install -y git

# Download aarch64 binary archive
ADD https://github.com/idena-network/idena-go/releases/download/v${IDENA_VERSION}/idena-node-linux-${IDENA_ARCH}-${IDENA_VERSION}.tar.gz /idena/idena.tar.gz

# Extract the binary
RUN tar -xzf /idena/idena.tar.gz -C /

# Make it executable
RUN chmod +x /idena-node

# Clean up archive
RUN rm /idena/idena.tar.gz

# Create user and assign ownership
RUN adduser -u 6789 --disabled-password --gecos "" idenauser
RUN chown -R idenauser /idena

# Switch to unprivileged user
USER idenauser

# Healthcheck
HEALTHCHECK --interval=15s --timeout=10s --start-period=30s --retries=3 \
  CMD [ "/usr/bin/curl", "-f", "http://bna_idena:9009" ]

# Start command
CMD [ "/idena-node", "--config", "/idena/idena-conf.json" ]
