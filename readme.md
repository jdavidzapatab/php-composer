## Docker Container for PHP and Composer

This repository provides Docker images for various PHP versions with Composer installed. It is designed to be a slim and secure base for PHP projects.

Available PHP versions:
- **8.5** (Latest)
- **8.4**
- **8.2**
- **5.6** (Legacy, includes `mcrypt`)

## Build

To build the images, use the following commands from the root of the repository:

### PHP 8.5 (Latest)
```bash
docker buildx build --platform linux/amd64,linux/arm64 -t davidzapata/php-composer:8.5 -t davidzapata/php-composer:latest --push 8.5/
```

### PHP 8.4
```bash
docker buildx build --platform linux/amd64,linux/arm64 -t davidzapata/php-composer:8.4 --push 8.4/
```

### PHP 8.2
```bash
docker buildx build --platform linux/amd64,linux/arm64 -t davidzapata/php-composer:8.2 --push 8.2/
```

### PHP 5.6
```bash
docker buildx build --platform linux/amd64,linux/arm64 -t davidzapata/php-composer:5.6 --push 5.6/
```

## Pull from Docker Hub

You can pull the pre-built images:

```bash
docker pull davidzapata/php-composer:8.5
docker pull davidzapata/php-composer:8.4
docker pull davidzapata/php-composer:8.2
docker pull davidzapata/php-composer:5.6
docker pull davidzapata/php-composer:latest
```

## Usage

Go into any project that has a `composer.json` and run the following commands.

### Running Composer
```bash
# Using 8.5
docker run --rm -v $(pwd):/var/www davidzapata/php-composer:8.5 composer install

# Using 5.6
docker run --rm -v $(pwd):/var/www davidzapata/php-composer:5.6 composer install
```

### Creating a Laravel Project
```bash
docker run --rm -v $(pwd):/var/www davidzapata/php-composer:8.5 composer create-project --prefer-dist laravel/laravel blog
```

### Running PHP Built-in Server
```bash
docker run --rm -p 80:80 -v $(pwd):/var/www davidzapata/php-composer:8.5 php -S 0.0.0.0:80 -t public
```

## Security & CVE Scanning

All images are tested for CVEs using [Docker Scout](https://docs.docker.com/scout/).

To scan an image:
```bash
docker scout cves davidzapata/php-composer:8.5
```

The Dockerfiles follow best practices to remain slim and secure:
- Use official PHP CLI images as base.
- Multi-stage or single-layer builds to minimize footprint.
- Targeted security updates for known vulnerabilities.
- Removal of build-time dependencies (GCC, etc.) after extension installation.

## As a base image

```dockerfile
FROM davidzapata/php-composer:8.5

# your custom logic here
```