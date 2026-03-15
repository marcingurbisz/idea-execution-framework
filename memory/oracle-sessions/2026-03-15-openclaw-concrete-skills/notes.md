# Notes

The Oracle workflow was still useful here even though the environment blocked a full run:

- it forced a precise research question
- it produced a durable repo-local session folder
- it surfaced environment gaps that matter for future Oracle automation (`OPENAI_API_KEY`, browser cookies, `xsel`)

For now, the best IEF-compatible fallback is:
1. prepare the Oracle request and record the blocker explicitly
2. continue with local repo/web research
3. save the synthesized result in `memory/`
4. revisit true Oracle execution later from an environment with browser auth or API access
