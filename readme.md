# dokku-custom-domains

[Dokku](https://github.com/progrium/dokku) plugin to configure additional custom domains for your dokku app.

## Background

If you add a [custom domain to your heroko app](https://devcenter.heroku.com/articles/custom-domains), you can count on the fact that it will work in the same manner as the `appname.herokuapp.com` domain. The purpose of this plugin is to bring this possibility to dokku users.

## What differentiates this plugin from other dokku domain-related plugins?

Unlike other domain-related plugins, this plugin will update the existing app nginx.conf instead of rewriting it or coupling it with a similar but different vhost setup for the additional domains. Having to maintain two different set-ups of nginx vhost configuration would unnecessarily add to the maintenance costs, provides more sources for error, and go against the Unix philosophy: "Write programs that do one thing and do it well. Write programs to work together."

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

Create vhost with a second domain:

```bash
$ dokku domains:set <app> myawesomeapp.com            # Server side
$ ssh dokku@server domains:set <app> myawesomeapp.com # Client side
```

Create vhost with multiple additional domains:

```bash
$ dokku domains:set <app> myawesomeapp.com www.myawesomeapp.com            # Server side
$ ssh dokku@server domains:set <app> myawesomeapp.com www.myawesomeapp.com # Client side
```

Note: The original domain set by dokku nginx-vhosts plugin is kept active.

## License
MIT
