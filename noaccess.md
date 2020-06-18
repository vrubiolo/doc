# Get back access to one's YunoHost

Many reasons can prevent a YunoHost server admin from accessing their server,
either partially or totally. In many cases, one of the accesses is locked but
others remain operational.

This page will help you diagnose the problem, get back access and fix the issue
if needed. Issues are sorted from most to least common, top to bottom. You just
need to go over them and test each in turn.

## Having access to the server via its IP address, but not via the domain name?

#### If you are self-hosted at home : you need to configure port redirections

Check you are able to access the server using its global IP (that you are can get via https://ip.yunohost.org). If that does not work:
   - Check you have configured [port redirections](/isp_box_config)
   - Some ISP boxes do not support hairpinning and you cannot access your server from inside your local network (unless you are using the local IP address). To work around the issue, you can try to access your server using a proxy like proxfree.com

#### You need to configure your DNS records

(Note: this is not necessary if you are using a domain like `nohost.me`, `noho.st` or `ynh.fr`)

You need to configure your DNS records as explained on [this page](/dns_config) (at the very least the A record, and the AAAA one if you are using IPv6).

You can validate your DNS records by checking what is returned by https://www.whatsmydns.net/ with your server global IP (if you are self-hosted at home, you can get this IP on https://ip.yunohost.org)

#### Other possible causes

- Your `noho.st`, `nohost.me` or `ynh.fr` domain name is unreachable due to a Yunohost infrastructure outage. Check on the forum if others are reporting a similar issue.
- Your domain name might be expirted. You can check whether that's the case by connecting over to your registrar management interface or by checking the WHOIS records, for instance by using the `whois YOUR_DOMAIN_NAME` command.
- You have a dynamic IP address. In this case, you need to setup a script that periodically updates your IP (or switch to using a `nohost.me`, `noho.st` or `yhn.fr` domain name, which includes such a functionality).

## You are experiencing a certificate error which prevents you from accessing the webadmin interface

If you have just installed your server or added a new domain, your server is currently using a self-signed certificate. In this case, it should be possible and legit to *exceptionally* add a security exception until you have [installed a Let's Encrypt certificat](/certificate), provided you are using a safe Internet connection (i.e. not via the Tor Browser for instance).

A certificate error can also be displayed in certain cases if you made a typo in your browser address/URL bar.

## You have SSH access but not any Web admin one or vice-versa

#### You are trying to connect via SSH as `root` instead of being `admin`

By default, an SSH connection to your server must be made as the `admin` user. It is possible to connect as the `root` user *only from the local network* where the server is (or via the web console/VNC session for VPSes)

When you are running `yunohost` commands as the `admin` user, you need to prefix them with `sudo` (for instance, `sudo yunohost user list`). You can also become `root` by running `sudo su`.

#### You have been temporarily banned

Your YunoHost server includes a mechanism (fail2ban) which automatically bans IPs that fail multiple successive authentications attempts. In certain cases, it might be a program (like a Nextcloud client) which is configured with an old/obsolete password or a user which is assigned the same IP as you.

If you have been banned while attempting to access a web page, only web pages will be unreachable, you should still be able to access the server over SSH. In the same vein, if you have been banned while using SSH, you should be able to access the webadmin interface.

If you have been banned both over SSH and for the webadmin, you can try accessing your server using a separate, different IP, for instance over a smartphone 4G network or by using the Tor browser.

See also: [unbanning an IP with Fail2ban](/fail2ban)

Note: a ban usually lasts 10 to 12 minites. IP banning is only available over IPv4.

#### The nginx web server is broken

Maybe the nginx server is out of order. You can check this [over ssh](/ssh) with `yunohost service status ssh`. If nginx is out of order, check its configuration is sane via `nginx -t`. If the configuration itself is broken, this might be due to having installed or uninstalled a bad quality app... If you are lost [ask for help](/help).

It might also be possible that the web server (nginx) or the SSH server have been killed because the machine is out of disk space or RAM/swap.
- Try to restart the service using `systemctl restart nginx`.
- You can check the available disk space via `df -h`. If one of your partitions is full, you'll need to identify what is using that space on your system and perform some cleanup. One can install the `ncdu` software via the `apt install ncdu` command and then call `ncdu /` to analyze folder size of the entire filesystem.
- You can check on RAM/swap usage via the `free -h` command. Depending on the results, it might be necessary to perform some memory optimizations on your machine so that less RAM is used (remove bloated and useless apps, ...) or to add more RAM or a swap file.

#### Your server is reachable over IPv6 but not over IPv4 or vice-versa

You can check this by attempting to ping your server over IPv4 and IPv6.

In such a case, you may be able to reach the webadmin over IPv6 but not via SSH, which normally defaults to IPv4...

In this case, you should tackle your connectivity issue.

In certain cases, a box/router firmware upgrade did enable IPv6, leading to configuration problems with your domain name.

## The webadmin interface is working but certain web applications return a 502 error code.

It's very likely that the service backing those applications is out of service (typically for PHP applications, that would be `php7.0-fpm` or `php7.3-fpm`). You could thus try to relaunch the service and if that does not work, inspect the associated service log and/or [ask for help](/help).


## Vous avez perdu votre mot de passe administrateur ? (ou bien le mot de passe est refusé)

Si vous arrivez à afficher la page web d'administration (forcez le rafraîchissement avec CTRL + F5 pour être sur) et que vous n'arrivez pas à vous connectez, vous avez probablement un mot de passe erroné.

Si vous êtes certain du mot de passe, il est possible que le service SLAPD qui gère l'authentification soit en panne. Si c'est le cas, il vous faut vous connecter en `root`.
- Si votre serveur est chez vous, vous avez sans doute accès au réseau local du serveur. Depuis ce réseau, vous pouvez vous connecter [en SSH](/ssh) avec l'utilisateur `root`.
- Si vous êtes sur un VPS, votre hébergeur vous fournit peut-être la possibilité d'avoir une console sur votre serveur depuis le navigateur web. 
Une fois connecté, il vous faut regarder l'état du service avec la commande `yunohost service status slapd` et/ou tenter de réinitialiser votre mot de passe avec la commande `yunohost tools adminpw`.

Si vous ne pouvez pas ou ne réussissez pas non plus à vous connecter en `root`, vous allez devoir opérer en mode rescue.

TODO: à compléter


## Votre VPN a expiré ou ne se monte plus

Si vous utilisez un VPN a IP fixe, peut être que celui-ci est arrivé à expiration ou que l'infrastructure de votre fournisseur est en difficulté.

Dans ce cas, vous pouvez peut être accéder à votre serveur avec son IP locale s'agissant probablement d'un serveur auto-hébergé chez-vous.

Pour connaître votre IP locale, certaines BOX proposent une cartographie du réseau en cours avec les équipements connectés. Sinon, en ligne de commande avec linux:
```bash
sudo arp-scan --local
```

Vous pouvez aussi essayer avec le domaine `yunohost.local` s'il n'y a qu'un seul YunoHost sur votre réseau.

Il faut voir avec votre fournisseur de VPN pour renouveler le VPN et mettre à jour les paramètre de l'app VPN Client.

TODO: à compléter

## Votre serveur est coincé au démarrage

Dans certains cas, votre serveur peut rester coincé au démarrage. Il peut s'agir d'un problème suite à l'installation d'un nouveau kernel. Essayez de choisir un autre kernel avec VNC ou avec l'écran lors du boot.

Si vous êtes en mode `rescue` avec `grub`, dans ce cas il peut s'agir d'un problème de configuration de `grub` ou d'un disque corrompu.

Dans ce cas il faut accéder au disque avec un autre système (mode `rescue` du fournisseur, live usb, lire la carte SD ou le disque dur avec un autre ordinateur) et essayer de vérifier l'intégrité des partitions avec `smartctl`, `fsck` et `mount`.

Si les disques sont corrompus et difficiles à monter, il faut sauvegarder les données et potentiellement refaire un formatage/réinstaller et/ou changer le disque. Si on arrive à monter le disque, il est possible d'utiliser `systemd-nspawn` pour entrer dans la base de données.

Sinon, relancer `grub-update` et `grub-install` en `chroot` ou avec `systemd-nspawn`.





## L'accès en VNC ou via écran ne fonctionne pas

Dans ce cas il peut s'agir d'un problème matériel sur votre serveur physique ou d'un problème d'hyperviseur si c'est un VPS.

Si c'est une machine louée contactez le support de votre fournisseur. Sinon, essayez de dépanner votre machine en retirant les composants qui peuvent être en panne.
