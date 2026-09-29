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
3. The existing **Upload image** control automatically opens the Cloudinary Upload Widget.

This static prototype intentionally does not expose an API secret. The later Worker integration will retrieve structured metadata, moderation outcomes, workflow status, and generated derivatives safely.
