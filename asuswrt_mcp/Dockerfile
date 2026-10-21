ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.20
FROM ${BUILD_FROM}

# Python runtime + SSH client
RUN apk add --no-cache \
    python3 \
    py3-pip \
    py3-virtualenv \
    openssh-client

WORKDIR /app

RUN python3 -m venv /app/venv

# Install from PyPI (no git during image build)
# mcp-proxy: stdio -> SSE for Home Assistant MCP client
# asuswrt-mcp: https://pypi.org/project/asuswrt-mcp/
RUN /app/venv/bin/pip install --no-cache-dir --upgrade pip \
 && /app/venv/bin/pip install --no-cache-dir \
      "mcp-proxy" \
      "asuswrt-mcp>=0.1.0"

COPY rootfs /
RUN chmod a+x /etc/services.d/asuswrt_mcp/run \
 && chmod a+x /etc/services.d/asuswrt_mcp/finish \
 && chmod a+x /etc/cont-init.d/00-asuswrt-env
