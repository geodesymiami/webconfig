# Webconfig (volcano list for VolcDef_web)

This directory holds the source Excel config and (optionally) generated `volcanoes_volcdef.json` when working from the minsar repo.

## Production: shared webconfig directory

For the VolcDef_web site, use a single shared config directory (e.g. `/var/www/webconfig`) so the app and the make script both use the same file. No manual copy is needed.

1. **Create the webconfig directory** (on the server):
   ```bash
   sudo mkdir -p /var/www/webconfig
   sudo chown www-data:www-data /var/www/webconfig   # or your Apache user
   ```

2. **Generate the volcano list** into that directory (from minsar repo):
   ```bash
   make_volcdef_volcanoes_json.py webconfig/Holocene_Volcanoes_volcdef_cfg.xlsx --outdir /var/www/webconfig
   ```
   Or, if the xlsx lives elsewhere: `make_volcdef_volcanoes_json.py /path/to/Holocene_Volcanoes_volcdef_cfg.xlsx --outdir /var/www/webconfig`

3. **Configure VolcDef_web** to read from it by setting `WEBCONFIG_DIR=/var/www/webconfig` (the bundled `volcdef_web.conf` already sets this via Apache `SetEnv`).

After that, updating the list is just: edit the xlsx, run the script with `--outdir /var/www/webconfig`, and reload the site if needed.
