# ---------------------------------------------------------------
# Netlify configuration for Arete Fitness
# ---------------------------------------------------------------
# Netlify's post-processing ("Asset optimization" / "Pretty URLs")
# rewrites HTML after upload — it strips .html from links and remaps
# paths. On the previous deploy that remapping broke: every URL
# started serving the wrong file (the homepage served the contact
# page, /about served a JPEG, /bundles served the booking form).
#
# skip_processing turns all of that off, so every URL resolves to
# exactly the file it names — which is what the links in these pages
# already expect (they point at real .html filenames).
#
# Leave this file in place. Do not re-enable asset optimization in
# the Netlify dashboard.
# ---------------------------------------------------------------

[build.processing]
  skip_processing = true
