# API Anything operations

This directory contains checked API Anything specifications for public interfaces. On `ssau.ru`, `getNewsById` and `searchNews` were learned using public values, passed a holdout call, and were independently called without cookies. On `priemsamara.ru`, `getProgramTitle` was learned with an HTML extraction recipe that keeps only the program title, then called with a holdout ID. Checks ran on 2026-10-27.

The learner labeled the public `ssau.ru` requests as logged-in. Direct requests to the same routes succeeded without cookies, so `learnedLoggedIn` is set to `false` in the checked-in spec. This is recorded here to make the manual correction explicit.

To explore locally without sharing captures or publishing learned operations:

```bash
export API_ANYTHING_HOME="$PWD/.api-anything-local"
npx -y github:goodnight000/api-anything capture 'https://ssau.ru/rasp' --outline
npx -y github:goodnight000/api-anything learn \
  'https://ssau.ru/api/news/?id={id}' \
  --args id=26231 --args2 id=26230 --args3 id=26229 \
  --site ssau --op getNewsById
```

Keep `.api-anything-local` out of version control: captures can contain cookies. Export only reviewed public operations into this directory. Do not run `publish` for this project unless publication to the shared API Anything catalog is explicitly intended.

`priem-samara.json` extracts only the program title from the HTML fragment. The unfiltered response can contain contact information; do not export a raw HTML response.
