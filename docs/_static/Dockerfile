FROM python:3.7

RUN apt-get update \
    && apt-get install -y curl software-properties-common \
    && curl -fsSL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs \
    && echo 'export HOME="$(mktemp -d)"' >> /entrypoint.sh \
    && echo 'rm -rf venv/' >> /entrypoint.sh \
    && echo 'python3 -m venv venv' >> /entrypoint.sh \
    && echo 'source venv/bin/activate' >> /entrypoint.sh \
    && echo 'npm install' >> /entrypoint.sh \
    && echo 'pip install nox' >> /entrypoint.sh \
    && echo 'exec "$@"' >> /entrypoint.sh \
    && chmod +x /entrypoint.sh

ENTRYPOINT ["/bin/bash", "-x", "/entrypoint.sh"]

# Must expand build:dev command to include --host flag in order for it to be exposed
CMD ["nox", "-s", "docs-live"]

# docker build --rm -f docs/_static/Dockerfile --iidfile build/id docs/_static
# docker run --rm -it -w "$PWD" -u "$(id -u):$(id -g)" -v "$PWD":"$PWD" -e LIVE_HOST="0.0.0.0" -e LIVE_PORT="1919" -p 1919:1919 --name sphinx --hostname sphinx "$(cat build/id)"
# http://localhost:1919/
