# Note: this Dockerfile is executed with the root of the staticdeploy monorepo
# as context
FROM node:8

WORKDIR /opt/staticdeploy

# Copy files
COPY package.json lerna.json yarn.lock tsconfig.json ./
COPY common-types/package.json common-types/tsconfig.json ./common-types/
COPY common-types/src ./common-types/src/
COPY sdk/package.json sdk/tsconfig.json ./sdk/
COPY sdk/src ./sdk/src/
COPY cli/package.json cli/tsconfig.json ./cli/
COPY cli/bin ./cli/bin/
COPY cli/src ./cli/src/

# Install dependencies. Don't use --frozen-lockfile since some subprojets were
# excluded, causing changes to yarn.lock on install
RUN yarn install && \
    # Compile code
    yarn compile && \
    # Remove dev dependencies
    yarn install --production && \
    # Remove unecessary files
    rm yarn.lock tsconfig.json && \
    rm -r common-types/tsconfig.json common-types/src && \
    rm -r sdk/tsconfig.json sdk/src && \
    rm -r cli/tsconfig.json cli/src && \
    # Install staticdeploy executable
    ln -s /opt/staticdeploy/cli/bin/staticdeploy.js /usr/local/bin/staticdeploy

# Reset workdir
WORKDIR /