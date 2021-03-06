# PHP container

PHP is used with Nginx via PHP-FPM. 

## WP CLI

PHP container has installed wp cli. When running wp cli make sure to open the shell as user 82 (www-data) to avoid access problems in the web server, which is running as user 82, too:
```bash
$ docker-compose exec --user 82 php wp cli version
```

## Composer

PHP container has installed composer. Example:
```bash
$ docker-compose exec --user 82 php composer update
```

## Xdebug

If you want to use Xdebug, uncomment this line to enable it in the compose file before starting containers:
```yml
PHP_XDEBUG: 1                 # Enable Xdebug extension
PHP_XDEBUG_DEFAULT_ENABLE: 1  # Comment out to disable (default).
```

If you would like to autostart xdebug, uncomment this line:
```yml
PHP_XDEBUG_REMOTE_AUTOSTART: 1     # Comment out to disable (default).
```

### Xdebug on macOS

There are two more things that need to be done on macOS in order to have Xdebug working (because there's no docker0 interface). Enable Xdebug as described in the previous section and uncomment the following two lines:

```yml
PHP_XDEBUG_REMOTE_CONNECT_BACK: 0         # Disabled for remote.host to work (enabled by default)
PHP_XDEBUG_REMOTE_HOST: "10.254.254.254"  # Setting the host (localhost by default)
```

You also need to have loopback alias with IP from above. You need this only once and that settings stays active until logout or restart.

```bash
sudo ifconfig lo0 alias 10.254.254.254
```

### Xdebug on Windows

You should do same things as for Mac OS. Enable Xdebug as described in the previous 2 sections and replace value of _PHP_XDEBUG_REMOTE_HOST_ to your DockerNAT ip assigned (by default it should be 10.0.75.1):

```yml
PHP_XDEBUG_REMOTE_HOST: "10.0.75.1"  # Setting the host (localhost by default)
```

You also need to check firewall not to block your connection. Disabling firewall should help.

## Configuration

Configuration is possible via environment variables. See the full list of variables on [GitHub](https://github.com/wodby/wordpress-php).
