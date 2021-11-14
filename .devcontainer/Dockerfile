# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.203.0/containers/dotnet/.devcontainer/base.Dockerfile

# [Choice] .NET version: 6.0, 5.0, 3.1, 6.0-bullseye, 5.0-bullseye, 3.1-bullseye, 6.0-focal, 5.0-focal, 3.1-focal
ARG VARIANT="6.0-bullseye-slim"
FROM mcr.microsoft.com/vscode/devcontainers/dotnet:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

ARG HTTP_PORT_RANGE=1100-1200

# Opt out of telemetry until after we install jupyter when building the image, this prevents caching of machine id
ENV DOTNET_INTERACTIVE_CLI_TELEMETRY_OPTOUT=true

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
     && apt-get -y install --no-install-recommends \
         wget \
        bzip2 \
        ca-certificates \
        sudo \
        locales \
        fonts-liberation \
        run-one \
        python3.8 \
        python3-pip \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

RUN python3 -m pip install setuptools
RUN python3 -m pip install jupyter
RUN python3 -m pip install jupyterlab

# Add package sources
RUN dotnet nuget add source "https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json" -n "dotnet-tools"
RUN dotnet nuget add source "https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet5/nuget/v3/index.json" -n "dotnet5"
RUN dotnet nuget add source "https://pkgs.dev.azure.com/dnceng/public/_packaging/MachineLearning/nuget/v3/index.json" -n "MachineLearning"

# Install lastest build from master branch of Microsoft.DotNet.Interactive
RUN dotnet tool install --tool-path /usr/share/dotnet-interactive Microsoft.dotnet-interactive --add-source "https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json"
RUN ln -s /usr/share/dotnet-interactive/dotnet-interactive /usr/bin/dotnet-interactive
RUN dotnet interactive jupyter install --http-port-range ${HTTP_PORT_RANGE}

# Enable telemetry once we install jupyter for the image
ENV DOTNET_INTERACTIVE_CLI_TELEMETRY_OPTOUT=false

EXPOSE 8888
EXPOSE ${HTTP_PORT_RANGE}

# ENTRYPOINT jupyter lab --ip=0.0.0.0  --allow-root  --notebook-dir=/notebooks/
# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1