# Superset

Use this role to install, configure, and manage Apache Superset.

By default this installs Superset version 0.24. To install a different version, set the `superset_version` variable.

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
```

where `request_token_params`, if provided is a JSON object. All values except `request_token_params`, `consumer_key`,
and `consumer_secret` are rendered in quotes. You would typically set `consumer_key` and `consumer_secret` to
retrieve env vars, e.g. `os.environ.get('OAUTH2_CLIENT_SECRET')`.

You can set your environment variables for the service using:

```yml
superset_service_env_vars:
  OAUTH2_CLIENT_SECRET: THE_SECRET
```

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

## Testing

This project comes with a Vagrantfile, this is a fast and easy way to test changes to the role, fire it up with `vagrant up`.

See [vagrant docs](https://docs.vagrantup.com/v2/) for getting setup with vagrant

## License

Apache 2

## Authors

[Ona Engineering](https://ona.io)
