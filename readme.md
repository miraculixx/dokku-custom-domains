# dokku-custom-domains

[Dokku](https://github.com/progrium/dokku) plugin to configure additional custom domains for your dokku app.

## Background

If you add a [custom domain to your heroku app](https://devcenter.heroku.com/articles/custom-domains), you can count on the fact that it will work in the same manner as the `appname.herokuapp.com` domain. The purpose of this plugin is to bring this possibility to dokku users.

## What differentiates this plugin from other dokku domain-related plugins?

Unlike other domain-related plugins, this plugin will update the existing app nginx.conf instead of rewriting it or coupling it with a similar but different vhost setup for the additional domains. Having to maintain two different set-ups of nginx vhost configuration would unnecessarily add to the maintenance costs, provides more sources for error, and go against the Unix philosophy: "Write programs that do one thing and do it well. Write programs to work together."

## How does it work?

The plugin modifies the nginx "server_name" directive to include all custom domains set by `domains:set`.

## Installation

```bash
git clone https://github.com/neam/dokku-custom-domains.git /var/lib/dokku/plugins/custom-domains
```

## Commands

```bash
$ dokku help
    domains <app>                                   display the domains for an app
    domains:set <app> DOMAIN1 [DOMAIN2 ...]         set one or more domains
```

## Simple usage

### Add domains

Create vhost with a custom domain:

```bash
$ dokku domains:set <app> myawesomeapp.com www.myawesomeapp.com            # Server side
$ ssh dokku@server domains:set <app> myawesomeapp.com www.myawesomeapp.com # Client side
```

Create vhost with a second sub-domain:

```bash
$ dokku domains:set <app> subdomain.myawesomeapp.com            # Server side
$ ssh dokku@server domains:set <app> subdomain.myawesomeapp.com # Client side
```

Create vhost with a wildcard domain:

```bash
$ dokku domains:set <app> *.myawesomeapp.com            # Server side
$ ssh dokku@server domains:set <app> *.myawesomeapp.com # Client side
```

Create vhost with multiple additional domains:

```bash
$ dokku domains:set <app> myawesomeapp.com www.myawesomeapp.com anotherawesomedomain.com www.anotherawesomedomain.com            # Server side
$ ssh dokku@server domains:set <app> myawesomeapp.com www.myawesomeapp.com anotherawesomedomain.com www.anotherawesomedomain.com # Client side
```

### Remove domains

Unlike heroku that uses `domains:add` and `domains:remove`, this plugin has only `domains:set`. To remove a domain, omit them from the arguments. So, if the domains are `myawesomeapp.com www.myawesomeapp.com anotherawesomedomain.com www.anotherawesomedomain.com` and you want to remove `anotherawesomedomain.com www.anotherawesomedomain.com`, run:

```bash
$ dokku domains:set <app> myawesomeapp.com www.myawesomeapp.com            # Server side
$ ssh dokku@server domains:set <app> myawesomeapp.com www.myawesomeapp.com # Client side
```

## Notes

The original domain set by dokku (through the standard nginx-vhosts plugin) is kept active.

If you destroy the app, any custom domains assigned to it will be freed. You can subsequently assign them to other apps.

## Unit tests

This plugin uses [assert.sh 1.0 - bash unit testing framework](http://github.com/lehmannro/assert.sh) and ships with relevant tests thanks to [wmluke](https://github.com/wmluke).

To run the tests:

    make test

## License
MIT
