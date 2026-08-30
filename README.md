# Maia Platform Website

Static landing page for [maiaplatform.org](https://maiaplatform.org), built with pure HTML, CSS and JavaScript.

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
