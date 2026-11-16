# Lens

[![Made with pollinations.ai](https://raw.githubusercontent.com/pollinations/pollinations/main/packages/ui/src/brand/badge-made-with.svg)](https://pollinations.ai/?ref=badge)

Type a science topic. Lens has a Pollinations text model write a short explanation in
three ordered stages, and a Pollinations image model paint an illustration for each one,
so the topic becomes something you step through rather than a wall of prose.

Live: **https://polli-lens.surge.sh**

## Why it exists

Explaining science is mostly sequencing: the hard part is the order, and where a reader
loses the thread. Lens forces that order. Each stage carries one idea, one illustration
and one question the reader can try before revealing the answer, so they can check their
understanding instead of nodding along.

## How it works

1. You paste your own Pollinations key. It is kept in `localStorage` on your device and
   sent only to `gen.pollinations.ai`.
2. **The plan**: one `POST /v1/chat/completions` to `openai/gpt-5.4-nano`, asked for a
   strict JSON object with a title, a summary and three stages. Each stage gives a name,
   an explanation, an image prompt, a probe question and its answer. The reply is parsed
   defensively: code fences and trailing commas are tolerated, every field is length
   capped, and a bad reply becomes a visible error rather than a broken screen.
3. **The illustrations**: `GET /image/{prompt}?model=flux&width=1024&height=1024` per stage,
   fetched with `Authorization: Bearer <your key>` and shown from a blob URL. They load
   lazily, only for the stage you are on, and only if auto-paint is on.
4. Navigation is a stepper plus arrow keys. Nothing is uploaded, nothing is stored on a
   server, and there is no analytics.

## Bring your own Pollen

Every request carries the visitor's own key, so the app costs its author nothing to run
and each reader pays for what they generate: roughly one text call plus one image per
stage they view. A free key is available at <https://enter.pollinations.ai/keys>.

## Run it yourself

No build step, no dependencies, one file:

```sh
git clone <this repo>
cd science-lens
python3 -m http.server 8080
# open http://127.0.0.1:8080
```

## Limits, stated honestly

- The text model is a small, fast model chosen for JSON discipline and cost, not depth.
  Explanations are short by design; a wrong or oversimplified sentence is possible, so
  this is a starting point for study, not a citation.
- Illustrations are generated images. They are not diagrams: structures, labels and
  scales in them are illustrative and should not be measured or quoted.
- Asking for "no text, no lettering" reduces stray lettering in the artwork but does not
  guarantee it.

## Licence

MIT. Generated text and images are subject to Pollinations' terms.
