# Caddy builder Docker image with built-in WAF

This repository extends the default [`caddy-builder`](https://hub.docker.com/_/caddy)
Docker image and uses it to build Caddy with:

- [Mercure](https://mercure.rocks/)
- [Vulcain](https://github.com/dunglas/vulcain)
- [Coroza WAF for Caddy](https://github.com/corazawaf/coraza-caddy)

To use it in your projects, simply `COPY` the `/usr/bin/caddy` from this image
to your own container, and set up any additional configurations you may need.

## Sample usage

```dockerfile
# Copy Caddy from the pre-built Caddy image
COPY --from=eckinox/caddy-waf /usr/bin/caddy /usr/bin/caddy

# Add assets for Mercure
COPY --from=dunglas/mercure:v0.11 /srv/public /srv/mercure-assets/

# Copy over your Caddyfile
COPY path/to/your/Caddyfile /etc/caddy/Caddyfile

# Add Coraza Web Application Firewall configurations
COPY path/to/your/coraza.conf /etc/caddy/coraza.conf
# Download the core ruleset to your Coraza WAF configurations
RUN wget -O coreruleset.zip https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.2.zip; \
	unzip coreruleset.zip; \
	rm coreruleset.zip; \
	mv coreruleset-* /etc/caddy/coreruleset;

# Start the Caddy server
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
```

## Related resources
- [Official Caddy Docker image](https://hub.docker.com/_/caddy)
- [Docker for Symfony repo](https://github.com/dunglas/symfony-docker)