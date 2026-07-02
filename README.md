# Healthily API Examples

Runnable examples for the [Healthily](https://www.livehealthily.com) Smart Symptom Checker API, packaged as a [Bruno](https://www.usebruno.com) collection.

The Healthily API is conversational and stateful — most calls only make sense in the context of the calls that came before them (login → consent → initial query → symptom Q&A → report). This repo captures complete, real conversation flows as **Scenarios** you can replay end to end, alongside **Individual Endpoints** for reference on each call in isolation.

## Getting started

1. **Install Bruno** — download it from [usebruno.com](https://www.usebruno.com/downloads).
2. **Clone this repo**
   ```sh
   git clone <this-repo-url>
   cd healthily-api-examples
   ```
3. **Open the collection** — in Bruno, choose *Open Collection* and select `collections/Healthily-V1`.
4. **Set your credentials** — pick an environment (`Healthily-prod`, `Healthily-staging`, or `Healthily-dev`) and fill in the three secret variables you received from Healthily:
   - `partner_id`
   - `partner_secret`
   - `api_key`

   These are marked as secrets, so Bruno stores them locally and never writes them back into the repo.
5. **Run a scenario** — open any folder under `Scenarios`, right-click it and choose *Run*. Requests execute in order; each one carries forward the state from the previous response (auth token, conversation id, chosen answers), ending in a report.

Requests under `Individual Endpoints` can be run one at a time, but remember the statefulness: most need a fresh token from `Login` and a conversation started by `Initial query`.

## What's inside

```
collections/Healthily-V1/
├── Individual Endpoints/   # one request per API endpoint
├── Scenarios/              # complete conversation flows, run in order
│   ├── Earache Jaw clicks convo/
│   ├── Clarification difficulty moving arm .../
│   ├── No outcome reports/          # edge cases: no cause found, assessment not possible, ...
│   └── ...
└── environments/           # dev / staging / prod targets
```

## Running from the CLI

The scenarios also run headless with the Bruno CLI, which is handy for CI or smoke tests:

```sh
npm install
cd collections/Healthily-V1
npx bru run Scenarios -r --env Healthily-prod \
  --env-var partner_id="$PARTNER_ID" \
  --env-var partner_secret="$PARTNER_SECRET" \
  --env-var api_key="$API_KEY"
```

This repo runs exactly that against staging and production on a nightly schedule (see `.circleci/config.yml`).
