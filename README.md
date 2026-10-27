# OpenMage Cloudflare Purge (DM_Cloudflare)

Purges the whole Cloudflare cache for your site from the Magento 1 / OpenMage
admin, so visitors see changes straight away after you flush Magento's own cache.

- **Automatic:** clicking **Flush Magento Cache** or **Flush Cache Storage** in
  System > Cache Management also purges Cloudflare.
- **Button:** a **Purge CloudFlare Cache** button is added to System > Cache
  Management for purging Cloudflare on its own.
- Uses the Cloudflare API v4 with an **API token** (Bearer auth), not the older
  global API key.
- Shows Cloudflare's own error message in the admin when a purge fails (with the
  first and last four characters of the token and the zone ID, to help spot a
  wrong one).

Originally written by Agli Panci; maintained by YSR Tech.

Compatible with Magento 1.9 and OpenMage 19/20 on PHP 7.4 and later. Needs the
PHP `curl` and `dom` extensions.

## Install

Copy `app/` into the Magento root, then flush the cache. There is no database setup.

```
app/etc/modules/DM_Cloudflare.xml
app/code/community/DM/Cloudflare/...
```

## Settings

System > Configuration > General > **CloudFlare**
(`general/cf_group/*`):

| Setting | |
|---|---|
| Cloudflare API Token | Create one at dash.cloudflare.com > My Profile > API Tokens with the **Zone > Cache Purge > Purge** permission for your zone. |
| Cloudflare Zone ID | On the Overview page of your domain in the Cloudflare dashboard. |

If either is empty, a purge shows "API Token or Zone ID is not configured" and
does nothing.

## Notes

- It purges **everything** in the zone. It does not purge single URLs, and it
  does not purge when a product or CMS page is saved; only on the cache flushes
  above or the button.
- The token is stored as plain text in `core_config_data`, like any text setting.
  Give it only the Cache Purge permission.
- PHP 8.5 deprecates `curl_close()`, which this module calls after each request.
  It is harmless (the call has done nothing since PHP 8.0) and is kept so PHP 7.x
  releases the handle.

## Licence

[OSL 3.0](https://opensource.org/licenses/osl-3.0.php)
