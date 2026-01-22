# CV Website for Kaldis Berzins

# To deploy on a server
1. `git clone https://github.com/kaldis-berzins/cv-website.git`
2. Get a cloudflare API token with Edit zone DNS permissions for domain
3. Put the following in `.env`: `CF_DNS_API_TOKEN = '<API TOKEN>'`
4. `docker compose up`
5. `docker compose scale server=3` for redundancy
6. To check logs use `docker compose logs -f` or `docker compose logs --tail`