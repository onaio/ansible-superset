# Superset #

Use this role to install, configure, and manage Apache Superset.

## Role Variables ##

### Other Default variables are listed below ###

    # whether to restart Superset after making changes; default is True, for a cluster you may wish to disable
    superset_perform_restart: True

## Available Languages

This allows you control what languages are available and how they are labelled.

```yml
supeset_languages
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

## White Labelling ##

This role allows you to do some white-labelling of superset by allowed you to change the superset 'branding images', which are:

- The favicon: `s.png`
- The logo: `superset.png`
- The 2x logo: `superset-logo@2x.png`

The file sizes and dimensions of these images should be as close as possible to the images we are replacing, which can be found in the `static/images` directory of Superset.

Turn on white-labelling by setting this variable to True:

```yml
superset_white_label: True
```

### Use files to change the branding ###

You can use files by setting the following variables.

```yml
superset_white_label_use_filepaths: True
superset_favicon_path: "/path-to-/s.png"
superset_logo_path: "/path-to-/superset.png"
superset_2x_logo_path: "/path-to-/superset-logo@2x.png"
```

### Use base64 encoded images to change the branding ###

To avoid the hassle of dealing with files, you can use base64 encoded images by setting the following variables.

```yml
superset_white_label_use_base64: True
superset_favicon_base64: "base64 encoded favicon"
superset_logo_base64: "base64 encoded logo"
superset_2x_logo_base64: "base64 encoded 2x logo"
```

## License ##

Apache 2

## Authors ##

[Ona Engineering](https://ona.io)
