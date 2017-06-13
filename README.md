# Hello uShip

This repo is just a "hello world" example app using .NET Core. It also has [Habitat](https://habitat.sh) added for packaging the application.

## Quickstart

Make sure you have Habitat installed according to the [documentation](https://www.habitat.sh/docs/get-habitat/). Then, from the root of the directory, you should be able to build a Habitat package with:

    $ hab pkg build .

This will create a new Habitat studio, download the dependencies, and package everything into a .hart archive in the `results` directory. You can then run this on a machine that is running Habitat.

## Export to Docker

To export to Docker, from the root of the directory, first enter the Habitat studio and export the package:

    $ hab studio enter
    $ hab pkg build .
    $ hab pkg export docker emachnic/hello_uship

This will export to a Docker image and you should then be able to see it with the `docker images` command. To run it, you'll need to expose port 5000 to an unused port on your system:

    $ docker run -p 5000:5000 emachnic/hello_uship

You can also pass in [Toml](https://github.com/toml-lang/toml) configuration, like for updating the exposed port or the message that's displayed, to the `HAB_HELLO_USHIP` environment variable:

    $ docker run -p 5000:5000 -e HAB_HELLO_USHIP='important_message="Howdy y\'all!"' emachnic/hello_uship

## Apply Configuration

To apply configuration, like the port number or the message, to a running instance of hello_uship, you can do the following:

### In Habitat

On a system that is running Habitat directly, you can pipe stdin to `hab config apply`

    $ echo 'port = 5001' | hab config apply hello_uship.default 1

With this command, the configuration would be applied and your application instance(s) would be reloaded on the new port.

### In Docker

To apply config to a running Docker container, first exec into the container and then apply the configuration like you would in a normal Habitat environment:

    $ docker exec -it hello_uship /bin/sh
    $ echo 'important_message = "Howdy y\'all!"' | hab config apply hello_uship.default 1