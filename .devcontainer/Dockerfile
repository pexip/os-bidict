# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.223.0/containers/python-3/.devcontainer/base.Dockerfile

# [Choice] Python version: 3, 3.10, 3.9, 3.8, 3.7, 3.6, 3-bullseye, 3.10-bullseye, 3.9-bullseye, 3.8-bullseye, 3.7-bullseye, 3.6-bullseye, 3-buster, 3.10-buster, 3.9-buster, 3.8-buster, 3.7-buster, 3.6-buster
# Note: If on arm64/Apple Silicon, must use -bullseye variant.
ARG VARIANT="3.10-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/python:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# Install dev dependencies globally in the image.
# (Still easy to experiment with upgrades/new dependencies by installing them in --user scope,
# and if you want to keep them, add them to the requirements file and rebuild the container.)
COPY dev-deps/*.txt /tmp/bidict-dev-deps/
RUN pip3 --disable-pip-version-check install -U pip && \
  pip3 --disable-pip-version-check --no-cache-dir install \
  -r /tmp/bidict-dev-deps/test.txt \
  -r /tmp/bidict-dev-deps/lint.txt \
  -r /tmp/bidict-dev-deps/docs.txt \
  -r /tmp/bidict-dev-deps/dev.txt \
  && rm -rf /tmp/bidict-dev-deps

# [Optional] Install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
  && apt-get -y install --no-install-recommends \
  graphviz optipng  `# for docs/_static/build-bidict-types-diagram.sh` \
  fish              `# make fish shell available for development`

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1
