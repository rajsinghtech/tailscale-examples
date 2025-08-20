FROM mcr.microsoft.com/vscode/devcontainers/base:0-bullseye

# Install wiz cli
RUN curl -z wizcli -Lo wizcli https://downloads.wiz.io/wizcli/latest/wizcli-linux-amd64 && \
    chmod +x wizcli && \
    mv wizcli /usr/local/bin/

# Install Tailscale
RUN curl -fsSL https://pkgs.tailscale.com/stable/debian/bullseye.noarmor.gpg | tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null && \
    echo 'deb [signed-by=/usr/share/keyrings/tailscale-archive-keyring.gpg] https://pkgs.tailscale.com/stable/debian bullseye main' | tee /etc/apt/sources.list.d/tailscale.list && \
    apt-get update && \
    apt-get install -y tailscale && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Install supervisord
RUN apt-get update && \
    apt-get install -y supervisor && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Create supervisord configuration directory
RUN mkdir -p /etc/supervisor/conf.d

# Add supervisord configuration for tailscaled
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Add Tailscale startup script
COPY start-tailscale /usr/bin/start-tailscale
RUN chmod +x /usr/bin/start-tailscale

# Create tailscale state directory
RUN mkdir -p /var/lib/tailscale

# Start supervisord on container startup
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]