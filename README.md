# Maia Platform Website

Static landing page for [maiaplatform.org](https://maiaplatform.org), built with pure HTML, CSS and JavaScript.

## Install dependencies
```bash
sudo apt update
sudo apt install nginx certbot python3-certbot-nginx rsync -y
```

## Local preview

```bash
python3 -m http.server 8080
```

Open `http://localhost:8080`.

## Deploy to NGINX

```bash
sudo mkdir -p /var/www/maiaplatform.org
sudo rsync -av --delete ./ /var/www/maiaplatform.org/ --exclude nginx --exclude README.md
sudo cp nginx/maiaplatform.org.conf /etc/nginx/sites-available/maiaplatform.org
sudo ln -s /etc/nginx/sites-available/maiaplatform.org /etc/nginx/sites-enabled/maiaplatform.org
sudo nginx -t
sudo systemctl reload nginx
```

After the DNS records for `maiaplatform.org` and `www.maiaplatform.org` point to the VPS:

```bash
sudo certbot --nginx \
  -d maiaplatform.org \
  -d www.maiaplatform.org \
  --redirect

sudo certbot renew --dry-run
```

## Files

- `index.html`: complete website, including styles and interactions.
- `assets/images/maia-platform-hero.png`: hero artwork.
- `assets/images/favicon.svg`: browser icon.
- `nginx/maiaplatform.org.conf`: initial HTTP NGINX virtual host. Certbot adds HTTPS.

## Update the catalog on an existing installation

The Maia Meet card appears in Apps and Media and opens
`https://meet.maiaplatform.org/`, with a separate repository link.
To publish a changed `index.html` on the host serving Maia Platform, copy the
updated checkout/file there and run from that checkout:

```bash
sudo cp -a /var/www/maiaplatform.org/index.html /var/www/maiaplatform.org/index.html.bak
sudo install -m 0644 index.html /var/www/maiaplatform.org/index.html
```

This updates the page without replacing Nginx/TLS configuration or other assets.
No Nginx reload is needed for a static HTML update. Adjust the destination if the
site uses a different document root.
