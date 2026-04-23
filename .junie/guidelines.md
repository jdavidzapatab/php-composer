# PHP Composer Image Guidelines

This repository maintains multiple Dockerfiles for different PHP versions to build the `davidzapata/php-composer` image.

## PHP Versions
The following PHP versions must be maintained:
- `5.6`: Legacy support, must include the `mcrypt` extension and use Composer 2.2.
- `8.2`: Stable version.
- `8.4`: Previous version.
- `8.5`: Latest version (also tagged as `latest`).

## Dockerfile Requirements
- **Slimness**: Images must be as slim as possible. Use multi-stage builds or clean up apt caches and temporary files in the same layer as installation.
- **Extensions**: All images should include common extensions: `xdebug`, `gd`, `zip`, `bcmath`, `pdo_mysql`, `pdo_pgsql`, `soap`, `redis`, `pcntl`, `mongodb`, `sockets`.
- **Composer**: Include the appropriate Composer version (latest for PHP 7+, 2.2 for PHP 5.6).
- **Security**: 
    - Always run `apt-get update` and `apt-get upgrade` to get latest security patches.
    - Test images for CVEs using `docker scout cves <image-tag>`.
    - Fix known critical/high CVEs if possible (e.g., upgrading specific libraries).

## Tagging & Publishing
- Images should be tagged using the pattern `davidzapata/php-composer:<major>.<minor>`.
- Example: `davidzapata/php-composer:8.5`
- **Multi-platform support**: Use `docker buildx` to build for `linux/amd64` and `linux/arm64`.
- **Pushing**: Use the `--push` flag to push images to the registry.

## Maintenance
When updating extensions or base images, ensure all supported PHP version Dockerfiles are updated accordingly and re-published using:
`docker buildx build --platform linux/amd64,linux/arm64 -t davidzapata/php-composer:<version> --push <the dockerfile folder>`
