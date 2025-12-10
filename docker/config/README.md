# Environment Configuration Files

This directory contains environment configuration files for the Ichnaea application.

## Files

### `my.env.dist`
Template file for personal environment settings. Copy this to `my.env` to create your personal configuration file:

```bash
make my.env
```

or manually:

```bash
cp docker/config/my.env.dist my.env
```

**Note:** `my.env` is gitignored and should never be committed to version control.

### `local_dev.env`
Default configuration for local development environment. This file contains:
- Database connection strings (MySQL/MariaDB)
- Redis connection settings
- Celery worker configuration
- Default values for running the app locally

These values are automatically loaded by docker-compose.

### `test.env`
Configuration specific to running tests. Uses separate Redis DB and test database to avoid conflicts with development data.

## Quick Start

To run the app with default configuration:

1. Create your personal environment file:
   ```bash
   make my.env
   ```

2. (Optional) Edit `my.env` to customize:
   - **Linux users**: Set `ICHNAEA_UID` and `ICHNAEA_GID` to match your user (run `id` to see values)
   - **Apple Silicon users**: Set `ICHNAEA_DOCKER_DB_ENGINE=mariadb_10_5`
   - Add `MAPBOX_TOKEN` if you want to see maps on the website

3. Build and run:
   ```bash
   make build
   make runservices
   make setup
   make run
   ```

## Configuration Precedence

Configuration values are loaded in this order (later sources override earlier ones):

1. Code defaults (in `ichnaea/conf.py`)
2. `local_dev.env` (or `test.env` for tests)
3. `my.env` (your personal overrides)

This means you can override any value from `local_dev.env` by setting it in your `my.env` file.

## Common Configuration Options

### Platform-Specific

**Linux**: You may need to set UID/GID to avoid file permission issues:
```bash
# In my.env
ICHNAEA_UID=1000  # Your user ID (run 'id -u')
ICHNAEA_GID=1000  # Your group ID (run 'id -g')
```

**Apple Silicon (M1/M2/M3 Macs)**: Use MariaDB instead of MySQL:
```bash
# In my.env
ICHNAEA_DOCKER_DB_ENGINE=mariadb_10_5
```

### Optional Features

**Mapbox Maps**: Enable map display on the website:
```bash
# In my.env
MAPBOX_TOKEN=pk.your_token_here
```
Get a free token at: https://account.mapbox.com

**Debug Mode**: Enable detailed debugging:
```bash
# In my.env
PYRAMID_DEBUG_ALL=True
```

## Default Values

The application runs with these defaults (from `local_dev.env`):

- **Database**: `mysql+pymysql://root:location@db:3306/location`
- **Redis**: `redis://redis:6379/0`
- **Logging**: `INFO` level
- **Celery Workers**: 1 concurrent worker
- **Environment**: Local development mode enabled

All these can be overridden in `my.env` if needed.

## More Information

For detailed configuration documentation, see:
- [Local Development Documentation](../../docs/local_dev.rst)
- [Configuration Reference](../../docs/configure.rst)
