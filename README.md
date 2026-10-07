# Connected Visual Media Lifecycle

A dependency-free, GitHub Pages-ready prototype for a Cloudinary platform demo.

## Run locally

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Enable the real Cloudinary Upload Widget

1. Create a restricted unsigned upload preset in the `demohost` cloud.
2. Set its name in `window.LIFECYCLE_DEMO_CONFIG.uploadPreset` in `index.html`.
3. The existing **Upload image** control will open the Cloudinary Upload Widget with **Local** and **URL** sources.

For this demo preset, set the destination to a dedicated demo asset folder, assign your starter tags and structured metadata, and configure any AI tagging, moderation, eager transformations, and notification URL there. Restrict allowed file formats, disallow caller-provided public IDs, and avoid using a production preset.

## Read tags and structured metadata

The `worker/` folder contains a small Cloudflare Worker that reads the uploaded asset by its Cloudinary `asset_id` and returns only the data the page needs: tags, contextual metadata, structured metadata, and moderation results. The browser polls it every 2.5 seconds for up to about 38 seconds after a successful upload.

Deploy the Worker and set these Worker secrets:

```sh
cd worker
wrangler secret put CLOUDINARY_CLOUD_NAME
wrangler secret put CLOUDINARY_API_KEY
wrangler secret put CLOUDINARY_API_SECRET
wrangler secret put ALLOWED_ORIGIN
wrangler secret put DEMO_ASSET_FOLDER
wrangler deploy
```

Set `ALLOWED_ORIGIN` to the exact GitHub Pages origin (for example, `https://lucasainsworth-cld.github.io`). For both production and local testing, use `ALLOWED_ORIGINS` instead, with a comma-separated allowlist such as `https://lucasainsworth-cld.github.io,http://localhost:8000`. The Worker only accepts requests made from an allowed origin and only resolves image assets by asset ID. Set `DEMO_ASSET_FOLDER` to the dedicated demo folder after the seed assets have been moved there. Then paste the deployed URL plus `/asset` into `window.LIFECYCLE_DEMO_CONFIG.assetEndpoint` in `index.html`.

The Cloudinary API secret stays exclusively in the Worker. Never add it to `index.html`, the repository, or GitHub Pages settings.
