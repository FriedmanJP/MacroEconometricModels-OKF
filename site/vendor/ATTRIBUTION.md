# Vendored OKF viewer — attribution

The `viewer/` and `bundle/document.py` files in this directory are vendored
from the [open-knowledge-format](https://github.com/GoogleCloudPlatform/open-knowledge-format)
reference agent (Copyright 2026 Google LLC, Apache License 2.0) at upstream
commit `ad30107`, instead of installing the full `reference-agent` package
(which would pull `google-adk` and `google-cloud-bigquery` just to render
the static viewer).

Upstream paths:

- `src/reference_agent/viewer/generator.py`
- `src/reference_agent/viewer/templates/viz.html` (unmodified)
- `src/reference_agent/viewer/static/viz.js` (unmodified)
- `src/reference_agent/viewer/static/viz.css` (unmodified)
- `src/reference_agent/bundle/document.py` (unmodified)

Local patches to `viewer/generator.py` only:

1. Import `bundle.document` instead of `reference_agent.bundle.document`
   (vendored layout).
2. `_extract_links` resolves bundle-root-relative (`/...`) links against
   the bundle root so cross-domain links become graph edges; upstream
   skips them (the viewer JS already navigates `/...` links in-concept).
3. `_TYPE_PALETTE` gains colors for this bundle's `Feature` and `Package`
   types.
