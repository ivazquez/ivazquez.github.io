# Original photos

**Canonical originals live in Google Drive**, in each person's folder under the
lab Drive (e.g. `susana-martin-gonzalez/`, `pieter-geukema/`). They are not kept
in this repository: it is public, so anything committed here is browsable and
clonable by anyone, even when excluded from the built site.

This folder is a local scratch space for cropping. Everything in it is
gitignored except this README.

It is also kept out of the build twice over: Jekyll skips top-level directories
beginning with `_`, and `_originals/` is listed in `_config.yml`'s `exclude:`.
Originals must never go in `assets/img/`, which is the
`imagemagick.input_directories` root — anything in there gets WebP derivatives
generated at 480/800/1400px and published at full resolution.

## Convention

Name a working copy after the published file it produces, so the pair is
obvious:

```
_originals/headshots/Pieter_Geukema.jpg   ->   assets/img/Pieter_Geukema.jpg
```

Keep the original's own aspect ratio and resolution. Only the published copy is
cropped and compressed.

## Cropping a headshot

Published headshots are **square** — every profile card in `_projects/` uses
one, and the team grid crops unevenly if a photo is not square. Crop so the face
sits just above the middle, with a little headroom and some shoulder below; a
crop tight around the face looks wrong next to the others.

```python
from PIL import Image
im = Image.open('_originals/headshots/Name_Surname.jpg').convert('RGB')
side, x0, y0 = 1080, 20, 160        # square side, left offset, top offset
im = im.crop((x0, y0, x0 + side, y0 + side)).resize((800, 800), Image.LANCZOS)
im.save('assets/img/Name_Surname.jpg', quality=92, subsampling=0)
```

Aim for roughly 700–1000px square. Larger adds repo weight without improving
the ~250px card.
