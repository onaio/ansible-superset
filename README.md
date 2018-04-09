# Superset #

Use this role to install, configure, and manage Apache Superset.

## Role Variables ##

### Other Default variables are listed below ###

    # whether to restart Superset after making changes; default is True, for a cluster you may wish to disable
    superset_perform_restart: True

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
