# Oracle API cost vs ChatGPT Plus

## Question

If I already pay for ChatGPT Plus, how much extra would I pay to use Oracle in API mode?

## Short answer

`ChatGPT Plus` and the OpenAI API are billed separately.

So if you use Oracle in:

- **browser mode** against ChatGPT, your extra OpenAI API cost is **$0** beyond the ChatGPT subscription itself
- **API mode** with `OPENAI_API_KEY`, you pay **additional pay-as-you-go API charges** on top of ChatGPT Plus

## Key billing facts

### ChatGPT Plus

Official help-center price: **$20/month**.

Notes:

- the live pricing page is localized by region/currency
- the currently returned localized pricing page showed **PLN 99.99/month**
- Plus gives access to ChatGPT product features, not bundled API credits

### API access is separate

OpenAI documents ChatGPT billing and API billing as separate platforms. In practice, this means:

- a ChatGPT subscription does **not** give you OpenAI API credits
- Oracle API runs are billed separately on the API platform

## Oracle-relevant OpenAI API prices

These are the most relevant prices for Oracle-style usage from the official OpenAI pricing pages.

### GPT-5.4

For prompts under the large-context threshold:

- input: **$2.50 / 1M tokens**
- cached input: **$0.25 / 1M tokens**
- output: **$15.00 / 1M tokens**

### GPT-5.4 Pro

For prompts under the large-context threshold:

- input: **$30.00 / 1M tokens**
- output: **$180.00 / 1M tokens**

Oracle defaults to a Pro model in its README examples/config, so this distinction matters a lot.

### GPT-5 mini

- input: **$0.25 / 1M tokens**
- cached input: **$0.025 / 1M tokens**
- output: **$2.00 / 1M tokens**

This is the cheap option if the goal is broad automation rather than premium long-form reasoning.

## Practical examples

These examples ignore search/file-search/container extras and are meant only as rough intuition.

### Example A — modest review run on GPT-5.4

Assume:

- 100k input tokens
- 10k output tokens

Approximate cost:

- input: $0.25
- output: $0.15
- total: **$0.40**

### Example B — the same run on GPT-5.4 Pro

Assume:

- 100k input tokens
- 10k output tokens

Approximate cost:

- input: $3.00
- output: $1.80
- total: **$4.80**

### Example C — the same run on GPT-5 mini

Assume:

- 100k input tokens
- 10k output tokens

Approximate cost:

- input: $0.025
- output: $0.02
- total: **$0.045**

## Extra charges that may matter

If Oracle/API runs use extra platform capabilities, additional costs can apply.

### Web search

- **$10 / 1K calls**
- plus search content tokens billed at model input rates

### File search

- **$2.50 / 1K tool calls**
- **$0.10 / GB / day** storage after the free first GB

### Containers / hosted shell / code interpreter

Current pricing page indicates:

- 1 GB: **$0.03**
- 4 GB: **$0.12**
- 16 GB: **$0.48**
- 64 GB: **$1.92**

OpenAI also notes a pricing transition toward per-session billing for these hosted execution resources.

## What this means for IEF

### Cheapest path

If you already have ChatGPT Plus and only need Oracle occasionally, the cheapest path is:

- prefer **browser mode** first
- fall back to API mode only when you need higher reliability, automation, or provider flexibility

### Sensible API strategy

If IEF adds Oracle support, a practical cost-aware strategy is:

1. default to **browser mode** for ChatGPT Plus users
2. use **API mode** for repeatable automation, follow-ups, or batch-style runs
3. prefer **`gpt-5.4`** over **`gpt-5.4-pro`** by default unless the task truly needs Pro-level reasoning
4. reserve **Pro/API** runs for explicitly hard research/review/escalation steps

## Plain-language conclusion

If you keep using Oracle through ChatGPT browser automation, your extra cost beyond ChatGPT Plus is effectively **none**.

If you use Oracle with an OpenAI API key, you start paying separate usage-based API charges immediately. For typical medium-sized runs, that can range from **a few cents** on `gpt-5-mini`, to **around tens of cents** on `gpt-5.4`, to **multiple dollars per run** on `gpt-5.4-pro`.

## Sources

- https://help.openai.com/en/articles/6950777-what-is-chatgpt-plus
- https://help.openai.com/en/articles/9039756-billing-settings-in-chatgpt-vs-platform
- https://help.openai.com/en/articles/8156019
- https://openai.com/api/pricing/
- https://developers.openai.com/api/docs/pricing
- https://chatgpt.com/pricing
- https://raw.githubusercontent.com/steipete/oracle/main/README.md
- https://raw.githubusercontent.com/steipete/oracle/main/docs/configuration.md
