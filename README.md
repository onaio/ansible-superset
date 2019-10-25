# Superset

Use this role to install, configure, and manage Apache Superset.

By default this installs Superset version 0.28.1. To install a different version, set the `superset_version` variable.

## Role Variables

`superset_python_executable`: Specifies which Python binary to use to install Superset. The role currently supports `python2.7` and `python3.6`.

`superset_recreate_virtualenv`: Set to `True` if you'd like the role to recreate the Python virtualenv Superset is installed.

This role creates a superset user and database. To connect to the database host, set:
- `superset_postgres_db_host`: The database host
- `superset_postgres_login_user`: The postgres user used to create the superset database and user
- `superset_postgres_login_password`: the password for the postgres user used to create the superset database and user
- `superset_postgres_db_name`: The superset database name (to be created). This is the database superset will use.
- `superset_postgres_db_user`: Superset postgres user name (to be created)
- `superset_postgres_db_pass`: Superset postgres user password

### Other Default variables are listed below

```yml
# whether to restart Superset after making changes; default is True, for a cluster you may wish to disable
superset_perform_restart: True
```

## Available Languages

This allows you control what languages are available and how they are labelled.

```yml
supeset_languages:
  - key: en
    flag: us
    name: English
  - key: ja
    flag: jp
    name: Japanese
```

The default is a subset of the total available languages, take a look at the `/translations` folder of the
current Superset release to see what other languages are available.

## Authentication Backends

This allows you to configure what authentication backend superset users. The default

```yml
superset_auth_type: AUTH_DB
```

uses Supersets on DB to manage users. Currenly configuration options are supported only for the `AUTH_OAUTH` type.

If you have selected that authentication type you can provide additional configuration details with

```yml
superset_oauth_providers:
  - name:
    icon:
    token_key:
    consumer_key:
    consumer_secret:
    base_url:
    request_token_params:
    request_token_url:
    access_token_url:
    authorize_url:
    access_token_method:
    custom_redirect_url:
```

where `request_token_params`, if provided is a JSON object. All values except `request_token_params`, `consumer_key`,
and `consumer_secret` are rendered in quotes. You would typically set `consumer_key` and `consumer_secret` to
retrieve env vars, e.g. `os.environ.get('OAUTH2_CLIENT_SECRET')`.

You can set your environment variables for the service using:

```yml
superset_service_env_vars:
  OAUTH2_CLIENT_SECRET: THE_SECRET
```

> Note that some oAuth providers such as `onadata` require that the `request_token_url`, `access_token_url`, and `authorize_url` all end with a `/` character.

## White Labelling

This role allows you to do some white-labelling of superset by allowing you to change the superset app name and 'branding images', which are:

- The favicon: `favicon.png`
- The logo: `superset.png`
- The 2x logo: `superset-logo@2x.png`

The file sizes and dimensions of these images should be as close as possible to the images we are replacing, which can be found in the `static/images` directory of Superset.

Turn on white-labelling by setting this variable to True:

```yml
superset_white_label: True
```

### Change App Name

```yml
superset_app_name: 'My App'
```

### Use files to change the branding

You can use files by setting the following variables.

```yml
superset_white_label_use_filepaths: True
superset_favicon_path: "/path-to-/favicon.png"
superset_logo_path: "/path-to-/superset.png"
superset_2x_logo_path: "/path-to-/superset-logo@2x.png"
```

### Use base64 encoded images to change the branding

To avoid the hassle of dealing with files, you can use base64 encoded images by setting the following variables.

```yml
superset_white_label_use_base64: True
superset_favicon_base64: "base64 encoded favicon"
superset_logo_base64: "base64 encoded logo"
superset_2x_logo_base64: "base64 encoded 2x logo"
```

### Asset Uploads

Superset, by default, will activate the following extensions for data uploads:

```yml
superset_allowed_extensions:
  - csv
```

Check the `ALLOWED_EXTENSIONS` configuration in Superset's [config.py](https://github.com/apache/incubator-superset/blob/master/superset/config.py) for the list of supported extensions.

The upload directories for images and data files (to be used by data import extensions) are set to:

```yml
superset_img_upload_dir: "{{ superset_home }}/images/"
superset_upload_dir: "{{ superset_home }}/uploads/"
```

### Change JSON Limit

Modify this value to increase the Superset dashboard position JSON data limit. By default this is set to 2^16, in the below example we increase it to 2^24.

```yml
superset_dashboard_position_data_limit: 16777216
```

### Superset-patchup (ketchup)

This role can be set to optionally include Superset-patchup, by doing this:

```yml
superset_use_ketchup: True
superset_ketchup_version: "v0.1.0"
```  

Superset-patchup enhances Superset by adding more features.  You can read more about this project here: [Superset-patchup documentation](https://github.com/onaio/superset-patchup)

> Note that the versions of ansible-superset bindings including this warning are only compatible with superset-patchup v0.1.6 and above (due to changed initialization logic).

###  Superset caching
To enable [caching](https://superset.incubator.apache.org/installation.html#caching) in superset, provide `CACHE_CONFIG` that complies with the Flask-Cache specifications.

```yml
superset_enable_cache: True
superset_cache_config: |
  {
    'CACHE_TYPE': 'redis',
    'CACHE_DEFAULT_TIMEOUT': 60 * 60 * 24, # 1 day
    'CACHE_KEY_PREFIX': 'superset_results',
    'CACHE_REDIS_URL': 'redis://localhost:6379/0'
  }
```

To allow periodical warmup of the cache, configure Superset's celery task with the preferred warmup strategy. Enable celerybeat and configure it's dictionary like so:

```yml
superset_enable_celerybeat: True
superset_celerybeat_schedule: |
  {
    'cache-warmup-hourly': {
      'task': 'cache-warmup',
      'schedule': crontab(minute=0, hour='*'),  # hourly
      'kwargs': {
          'strategy_name': 'top_n_dashboards',
          'top_n': 5,
          'since': '7 days ago',
      },
    },
  }
```

## Testing

This project comes with a Vagrantfile, this is a fast and easy way to test changes to the role, fire it up with `vagrant up`.

See [vagrant docs](https://docs.vagrantup.com/v2/) for getting setup with vagrant

## License

Apache 2

## Authors

[Ona Engineering](https://ona.io)
