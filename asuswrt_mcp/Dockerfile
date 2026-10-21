ARG BUILD_FROM
FROM ${BUILD_FROM}

# Python runtime + SSH client for key-based auth paths
RUN apk add --no-cache \
    python3 \
    py3-pip \
    py3-virtualenv \
    openssh-client \
    git

WORKDIR /app

# Virtualenv keeps add-on Python isolated from the base image
RUN python3 -m venv /app/venv

# mcp-proxy bridges stdio MCP servers to SSE for Home Assistant
# asuswrt-mcp provides the router tools (installed from upstream git)
RUN /app/venv/bin/pip install --no-cache-dir --upgrade pip \
 && /app/venv/bin/pip install --no-cache-dir \
      "mcp-proxy" \
      "asuswrt-mcp @ git+https://github.com/Teeflo/asuswrt-mcp.git"

COPY rootfs /
RUN chmod a+x /etc/services.d/asuswrt_mcp/run \
 && chmod a+x /etc/cont-init.d/00-asuswrt-env

