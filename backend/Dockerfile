# Dockerfile for Elixir Phoenix API
FROM elixir:1.14.4-alpine AS build

# Install dependencies
RUN apk add --no-cache bash git openssh-client build-base

# Install hex + rebar
RUN mix local.hex --force && mix local.rebar --force

# Set up working directory
WORKDIR /app

# Copy mix.exs and mix.lock
COPY mix.exs mix.lock ./

# Install Elixir dependencies
RUN mix deps.get --only prod

# Copy the application source code
COPY . .

# Build the application
RUN MIX_ENV=prod mix release

# Start a new container for the production environment
FROM alpine:latest AS app
RUN apk add --no-cache bash openssl

# Copy release from the build stage
WORKDIR /app
COPY --from=build /app/_build/prod/rel/backend ./

# Expose the port the app runs on
EXPOSE 4000

# Set up entrypoint
CMD ["./bin/backend", "start"]
