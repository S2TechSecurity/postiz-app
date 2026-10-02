# S2 Postiz integration

## Repository policy

- `origin`: `https://github.com/S2TechSecurity/postiz-app.git`
- `upstream`: `https://github.com/gitroomhq/postiz-app.git`
- All S2 customisation is developed against the S2 fork.
- Upstream changes are fetched from `upstream` into review branches before they
  are merged into our main branch.

## S2 AI Gateway

Postiz text/copy/agent workloads can use the S2 AI Gateway instead of a paid
OpenAI account.

Configure:

```env
OPENAI_API_KEY=<S2 AI Gateway bearer token>
OPENAI_BASE_URL=http://host.docker.internal:8787/v1
OPENAI_MODEL=creative
```

`OPENAI_API_KEY` is retained as the environment name for compatibility with
Postiz and its SDKs. In the S2 deployment the value is the S2 Gateway bearer
credential, not an OpenAI-issued key.

The S2 fork adds configurable base URL and model support across the direct
OpenAI SDK, CopilotKit adapter, LangChain agent flow, and autopost flow.

## Image generation

Do not point image calls at the S2 chat-completions gateway. The S2 fork uses
separate image variables:

```env
OPENAI_IMAGE_API_KEY=
OPENAI_IMAGE_BASE_URL=
OPENAI_IMAGE_MODEL=chatgpt-image-latest
```

If `OPENAI_BASE_URL` is configured and no image key is supplied, the text AI
can still use S2 Gateway while image generation remains unconfigured.

For the initial S2 Content Factory, MoneyPrinterTurbo remains responsible for
media generation and Postiz is responsible for scheduling, publishing,
analytics, collaboration and social-account OAuth.

## Deployment

Do not deploy the stock Compose file with its example passwords. A production
S2 deployment must use generated secrets, persistent Postgres/Redis/Temporal
storage, backups, and platform OAuth credentials. Build the Postiz application
from the S2 fork so the gateway compatibility code is included.
