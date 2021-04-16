# nextcloud-as-oauth-provider-for-openproject

Make Nextcloud act as an OpenID Connect provider for OpenProject

## Patch your NextCloud

Backup your instance. From the NextCloud installation root directory run the following commands :

```
$ wget https://raw.githubusercontent.com/betaglop/nextcloud-as-oauth-provider-for-openproject/main/nextcloud-userinfo.patch
$ patch -p0 nextcloud-userinfo.patch
```

## Add a OAuth 2.0 Client in your NextCloud

* Log in with an admin user.
* Get into your "Settings"
* Administration > Security
* Add a OAuth 2.0 client with this callback URL: `https://<OP_URL>/auth/nextcloud/callback`

## Configure OpenProject

Add those lines in your `config/configuration.yml`:

```
default:
  openid_connect:
    nextcloud:
      display_name: "Connexion NextCloud"
      scheme: "https"
      host: "<my.nextcloud.tld>"
      port: 443
      identifier: "<ID>"
      secret: "<KEY>"
      token_endpoint: /index.php/apps/oauth2/api/v1/token
      userinfo_endpoint: /index.php/apps/oauth2/api/v1/userinfo
      authorization_endpoint: /index.php/apps/oauth2/authorize
      redirect_uri: https://<OP_URL>/auth/nextcloud/callback
  disable_password_login: true
  omniauth_direct_login_provider: nextcloud
```

TIP: Make sure OpenID Connect features are available in your instance.

## Tested on

* OpenProject 11.2.0
* NextCloud v21.0.1
