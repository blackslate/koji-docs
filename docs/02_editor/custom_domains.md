# Custom domains

## Understanding services

A Koji project contains one or more services. Each of these services is a
separate NodeJS project, and typical projects have a frontend service, a 
backend service, or both. This is what makes Koji projects "full stack.

Each service starts its own process in development, and when a project is 
deployed, each service is deployed to its own domain name. Services are 
assigned an autogenerated domain name that typically looks like 
`SERVICENAME-project-id.withkoji.com`.

From the "Custom Domains" section of the editor, you can configure additional 
domains to serve your app.

## Custom subdomains

Koji controls a few domain names that are made available for you to use at no 
cost and with no additional configuration. These include:
- withkoji.com
- koji.sh
- koji.game

You can configure one custom Koji subdomain for each service in your project. 
For example, if you made a Gem Match game, you could choose `gems.withkoji.com` 
to point to your app's frontend, so that users accessing it directly don't need 
to type the giant autogenerated URL. These subdomains are available on a 
first-come, first-serve basis and are assigned to projects, not users. This 
means that if you assign a domain name and later choose to remove it, someone 
else can select it for their app.

## Managed domain names

If you buy your own domain name from somewhere like Google Domains or 
Namecheap, you can easily configure that domain to serve your app. Simply 
register your domain in your project, and then use your domain provider's 
management tools to configure the appropriate DNS records.

If you want to host your app on a subdomain of your custom domain name 
(e.g., `www.mycustomdomain.com` or `game.mycustomdomain.com`), you will need to 
use your domain provider's tools to add a `CNAME` record for that subdomain 
(`www` or `game`) that points to `cnames.withkoji.com`.

If you want your app to be served from the root domain name 
(`mycustomdomain.com`), you need to add an `A` record for the domain's root 
that maps it to Koji's IP block:
```
151.101.1.78
151.101.65.78
151.101.129.78
151.101.193.78
```

If you are configuring a root domain via `A` records, it is also recommended to 
configure `AAAA` records to support IPv6. Koji's IPv6 block is:
```
2a04:4e42::334
2a04:4e42:200::334
2a04:4e42:400::334
2a04:4e42:600::334
2a04:4e42:800::334
2a04:4e42:a00::334
2a04:4e42:c00::334
2a04:4e42:e00::334
```

Different domain providers have different tools for managing DNS records, and 
some of those tools are more complex than others. If you have questions about 
how to configure your domain's DNS records, please connect with us on Discord 
and someone will be happy to help.

Managed subdomains (withkoji.com, koji.sh, and koji.game) automatically manage 
HTTPS termination. This means that your app's traffic is secured with HTTPS, 
but your app itself does not require any special configuration (like listening 
on both ports 80 and 443, serving certificates and managing certificate 
lifecycle and renewal, and dealing with mixed-security content).

## HTTPS and Security

KojiCDN also automatically manages HTTPS certificates, renewal, and termination 
for custom domains via LetsEncrypt and our CDN partner Fastly. Once you've 
configured a custom domain for use with your app, it will take up to an hour 
for Koji to verify configuration, procure certificates, and begin serving 
traffic. You'll receive an email once this process is complete and your domain 
is ready to go.

As a bonus, once you've configured a custom domain for use with a Koji app, you 
can swap that domain to a new app without repeating the configuration process. 
Your new app will immediately begin being served on the domain.

## .well-known/koji

The `/.well-known/koji` directory is available on all static frontend services 
and provides a few resources:
- `/.well-known/koji` returns a JSON document detailing the domain's current project ID and release ID
- `/.well-known/archive.zip` returns a ZIP archive of the project's compiled frontend