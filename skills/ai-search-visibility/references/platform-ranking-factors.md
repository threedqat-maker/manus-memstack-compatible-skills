# Platform Ranking Factors — Evidence-Graded Reference

This reference summarizes how major AI answer engines and traditional search systems discover, select, and cite content as of **May 2026**. Treat it as a strategic working document, not a fixed ranking formula. AI search systems change quickly, and public documentation rarely exposes full ranking logic.

## Evidence grading

Use the following confidence labels when applying this reference.

| Grade | Meaning | How to use it |
|---|---|---|
| **Official** | Directly supported by platform documentation | Safe to use as baseline technical guidance. |
| **Commercial study** | Based on published studies by SEO or AI-visibility vendors | Useful directionally, but not proof of platform algorithms. |
| **Observed / inferred** | Based on field observation, log analysis, or third-party interpretation | Treat as a hypothesis to test on the specific site. |
| **Speculative** | Unsupported or rapidly changing claim | Do not build critical strategy around it without fresh verification. |

> **Critical distinction:** Search/indexing crawlers, user-triggered fetchers, and model-training crawlers are not equivalent. A site can allow AI answer-engine visibility while still blocking some training crawlers.

---

## Current crawler access matrix

| Platform / company | User agent | Type | Primary use | Recommended default for visibility |
|---|---|---|---|---|
| OpenAI | `OAI-SearchBot` | Search/indexing crawler | Surfaces websites in ChatGPT Search | **Allow** when ChatGPT Search visibility is desired. |
| OpenAI | `GPTBot` | Training crawler | Crawls content that may be used for OpenAI foundation-model training | Optional; allow only if training use is acceptable. |
| OpenAI | `ChatGPT-User` | User-triggered fetcher | Fetches pages in response to user actions in ChatGPT or Custom GPTs | Usually allow for answer access; note OpenAI says robots.txt may not apply the same way to user-triggered actions.[1] |
| Perplexity | `PerplexityBot` | Search/indexing crawler | Surfaces and links websites in Perplexity search results | **Allow** when Perplexity visibility is desired.[2] |
| Perplexity | `Perplexity-User` | User-triggered fetcher | Fetches pages in response to user requests | Usually allow for answer access; Perplexity says this generally ignores robots.txt because it is user-requested.[2] |
| Anthropic | `Claude-SearchBot` | Search/indexing crawler | Indexes content to improve Claude search results | **Allow** when Claude search visibility is desired.[3] |
| Anthropic | `Claude-User` | User-triggered fetcher | Fetches websites in response to Claude users | Usually allow for answer access.[3] |
| Anthropic | `ClaudeBot` | Training crawler | Crawls public web content that may contribute to model training | Optional; allow only if training use is acceptable.[3] |
| Google | `Googlebot` | Search crawler | Crawls for Google Search, including eligibility for AI Overviews and AI Mode | **Allow** for Google Search and AI feature eligibility.[4] |
| Google | `Google-Extended` | AI training / product-use control | Controls use of site content for Gemini and Vertex AI improvement, not ordinary Google Search crawling | Optional; do not confuse with Google Search indexing.[4] |
| Microsoft | `Bingbot` | Search crawler | Crawls for Bing Search, Copilot, and grounding experiences | **Allow** for Bing/Copilot visibility.[5] |
| Common Crawl | `CCBot` | Dataset crawler | Builds open web datasets used by many organizations | Optional; allow only if broad dataset inclusion is acceptable. |
| ByteDance | `Bytespider` | Crawler | ByteDance/TikTok-related crawling and AI uses | Optional; verify current policy before advising clients. |
| Cohere | `cohere-ai` | Crawler | Cohere-related model/data crawler where observed | Optional; verify current policy before advising clients. |

### Recommended robots.txt patterns

For most commercial sites that want AI search visibility but want to make a deliberate choice about model training, start with this pattern and adjust after legal/business review.

```txt
# Search/indexing crawlers for AI answer visibility
User-agent: OAI-SearchBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Claude-SearchBot
Allow: /

User-agent: Googlebot
Allow: /

User-agent: Bingbot
Allow: /

# User-triggered fetchers for answer access and citation opportunities
User-agent: ChatGPT-User
Allow: /

User-agent: Perplexity-User
Allow: /

User-agent: Claude-User
Allow: /

# Optional: restrict model-training crawlers while preserving search visibility
User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

# Block private or low-value paths for all crawlers
User-agent: *
Disallow: /admin
Disallow: /api
Disallow: /dashboard
```

Robots.txt controls crawling, not necessarily indexing or snippet display. For Google and Bing, use platform-specific preview controls, `noindex`, snippet controls, or cache/archive directives when the goal is controlling display or grounding use rather than raw crawl access.[4] [5]

---

## Platform summary table

| Platform | Primary discovery / grounding basis | Technical priority | Content priority | Evidence confidence |
|---|---|---|---|---|
| **ChatGPT Search** | OpenAI search crawler and user-triggered fetches; possible dependence on broader web indexes varies by implementation | Allow `OAI-SearchBot`; decide separately on `GPTBot`; ensure pages are crawlable and internally linked | Clear answer-first pages, strong authority signals, recent updates, community/review mentions | Official for crawler roles; commercial study for citation correlations |
| **Perplexity** | Perplexity crawler plus user-triggered fetches; Perplexity documents IP ranges and WAF guidance | Allow `PerplexityBot`; consider WAF allow rules using official IP endpoints | Atomic paragraphs, direct answers, source-rich content, strong entity clarity | Official for crawler roles; inferred for ranking factors |
| **Google AI Overviews / AI Mode** | Google Search index and core Search ranking/quality systems | Meet normal Google Search technical requirements; allow Googlebot; no special AI file or schema required | Helpful, reliable, people-first content; visible text; strong page experience; structured data that matches visible content | Official |
| **Microsoft Copilot / Bing grounding** | Bing index and Bing grounding systems | Allow Bingbot; use IndexNow, XML sitemaps, stable URLs, canonicalization, and appropriate meta directives | Clear, focused, independently verifiable content surfaced early on the page | Official |
| **Claude Search** | Anthropic web search and Claude crawler/fetcher ecosystem; search backend details are not fully public in official docs | Allow `Claude-SearchBot` and `Claude-User` for visibility; decide separately on `ClaudeBot` | Factual density, extractable structure, citations, clear definitions | Official for bots; uncertain for backend/ranking mechanics |
| **Google traditional** | Google Search index | HTTPS, mobile usability, crawlability, Core Web Vitals, valid canonical/indexing controls | E-E-A-T, original helpful content, topical depth, internal links | Official / long-established SEO evidence |

---

## 1. ChatGPT Search

### What is currently reliable

OpenAI documents three separate relevant agents. `OAI-SearchBot` is used to surface websites in ChatGPT Search features. `GPTBot` is for crawling content that may be used in model training. `ChatGPT-User` supports user-triggered actions in ChatGPT and Custom GPTs, and OpenAI notes that robots.txt rules may not apply to these user-triggered actions in the same way.[1]

Do **not** advise users that `GPTBot` is required for ChatGPT Search visibility. The safer policy is to allow `OAI-SearchBot` for search visibility and make a separate business decision on `GPTBot`.

### Directional citation factors

SE Ranking’s 2025 study of 129,000 domains and 216,524 pages found correlations between ChatGPT citations and referring domains, domain trust, traffic, content depth, freshness, community mentions, review-platform presence, and performance metrics.[8] These are **correlations**, not confirmed algorithmic weights.

| Signal | Current interpretation |
|---|---|
| Referring domains / domain trust | Strong directional authority signal in commercial studies. |
| Content freshness | Quarterly updates may improve citation likelihood for time-sensitive or competitive topics. |
| Community and review mentions | Reddit, Quora, Trustpilot, G2, Capterra, and similar mentions may influence answer confidence or retrieval prominence. |
| Content depth and structure | Comprehensive, clearly sectioned content appears to correlate with citations. |
| LLMs.txt | SE Ranking found negligible impact for ChatGPT citations; treat as optional for AI systems generally, not as a ChatGPT ranking lever.[8] |

### Practical optimization priorities

Focus on crawlability, clear topical pages, authority-building, brand/entity consistency, community proof, fresh statistics, and direct answer sections. Avoid keyword stuffing and thin AI-generated pages.

---

## 2. Perplexity

Perplexity officially documents `PerplexityBot` for surfacing and linking websites in Perplexity results and recommends allowing it in robots.txt when visibility is desired. It also documents `Perplexity-User` for user-triggered requests and provides official IP endpoints for WAF allowlisting.[2]

The old reference’s detailed “3-layer reranker” should be treated as **observed/inferred**, not official. Perplexity almost certainly uses multiple retrieval and ranking stages, but the exact internal scoring model is not public.

| Priority | What to do |
|---|---|
| Crawler access | Allow `PerplexityBot`; configure WAF rules using both User-Agent and official IP ranges where possible.[2] |
| Content structure | Use concise paragraphs, definition blocks, comparison tables, and source-rich answers. |
| Freshness | Update pages with current facts, dates, and source links. |
| PDFs and docs | Public PDFs and documentation can be useful, but do not assume automatic priority without testing. |

---

## 3. Google AI Overviews and AI Mode

Google’s official guidance is clear: there are **no additional technical requirements** to appear in AI Overviews or AI Mode beyond being indexed and eligible to appear in Google Search with a snippet. Google also says there is no special schema.org structured data, AI text file, or machine-readable file required for these features.[4] [6]

This means the previous reference’s precise claims about AI Overview trigger rates, ranking weights, and large visibility lifts should be removed or treated as external study claims only. Do not present them as Google’s ranking formula.

| Google recommendation | Practical implication |
|---|---|
| Follow foundational SEO best practices | Technical SEO, crawlability, internal links, helpful content, and page experience still matter. |
| Make important content available in textual form | Do not hide core claims only in images, scripts, or gated experiences. |
| Structured data must match visible content | Schema helps understanding/eligibility for rich features, but it is not a special AI Overview requirement. |
| Use Search Console for measurement | AI feature traffic is reported within Search Console’s normal Web search type.[4] |
| Use preview controls for snippets | `nosnippet`, `data-nosnippet`, `max-snippet`, and `noindex` control how content may appear.[4] |

---

## 4. Microsoft Copilot and Bing grounding

Bing’s webmaster guidelines explicitly state that they cover Bing search experiences, Copilot, and grounding API results. Bing says the same fundamentals that support discovery, indexing accuracy, content clarity, authority, and trust also support grounding and AI-powered search experiences.[5]

IndexNow remains important because Bing recommends it for notifying Bing/Copilot quickly when URLs are added, updated, or removed.[5] [7]

| Priority | What to do |
|---|---|
| Discovery | Use IndexNow, XML sitemaps, crawlable internal links, and external links. |
| Crawl/index controls | Understand that robots.txt controls crawl access, while `noindex` controls whether a URL should appear in Bing search, Copilot, or grounding API results.[5] |
| Snippet/grounding controls | Avoid `NOCACHE` and `NOARCHIVE` on pages intended for rich Copilot citations unless there is a deliberate policy reason.[5] |
| Content clarity | Put key information early, define entities clearly, keep one main topic per URL, and maintain accurate content. |

Page speed remains a quality and usability recommendation, but avoid presenting “under two seconds” as a confirmed Copilot ranking threshold unless a current source is available.

---

## 5. Claude Search

Anthropic’s current crawler documentation identifies three primary bots: `ClaudeBot`, `Claude-User`, and `Claude-SearchBot`. `ClaudeBot` is associated with model-training crawl use, `Claude-User` with user-directed access, and `Claude-SearchBot` with search indexing and result quality.[3]

The previous reference’s `anthropic-ai` user agent and hard claim that Claude Search is backed by Brave Search should not be treated as official without fresh supporting evidence. Anthropic’s official web search tool documentation confirms that Claude can use a web search tool and return cited sources, but it does not expose a public ranking formula or a complete backend-provider statement in the extracted documentation.[9]

| Priority | What to do |
|---|---|
| Search visibility | Allow `Claude-SearchBot` and `Claude-User` when Claude answer visibility is desired. |
| Training policy | Decide separately whether to allow or block `ClaudeBot`. |
| Content format | Use factual density, extractable structure, citations, tables, definitions, and clear source attribution. |
| Verification | Check server logs for `Claude-SearchBot`, `Claude-User`, and `ClaudeBot`; verify with current Anthropic docs before client implementation. |

---

## 6. Traditional Google Search baseline

Google Search remains the foundation for Google AI features. Optimize for helpful, reliable, people-first content, crawlability, page experience, mobile usability, internal linking, structured data that matches visible content, and clear ownership/author information.[4] [6]

| Area | Baseline guidance |
|---|---|
| Crawlability | Allow Googlebot where pages should be discoverable. |
| Indexability | Use canonical tags, noindex rules, and sitemap hygiene correctly. |
| Page experience | Maintain good Core Web Vitals and mobile usability. |
| Content quality | Prioritize unique, non-commodity content based on first-hand experience or expertise. |
| Structured data | Use schema where it accurately describes visible content; do not add fake or hidden claims. |

---

## Cross-platform takeaways

### Universal best practices

| Practice | Why it matters |
|---|---|
| Keep important content crawlable and indexable | AI answer engines depend on accessible source material. |
| Separate search visibility from training access | Allows nuanced policy decisions for `OAI-SearchBot` vs `GPTBot`, and `Claude-SearchBot` vs `ClaudeBot`. |
| Put the answer early | Bing and Google both emphasize clear, accessible content; AI systems extract better from direct answers. |
| Use clear headings, tables, and lists | Improves extraction, readability, and citation fit. |
| Cite authoritative sources | Supports trust and aligns with GEO research and general quality guidance. |
| Maintain freshness where facts change | Outdated facts reduce trust and may hurt eligibility in time-sensitive topics. |
| Build real authority signals | Links, mentions, reviews, expert authorship, and community proof all help retrieval and trust. |
| Avoid manipulative AI-only tactics | Google explicitly says special AI files and markup are not required for Google AI features.[4] [6] |
| Verify with logs and platform tools | Use Search Console, Bing Webmaster Tools, server logs, and manual AI-query checks. |

### Recommended monitoring checklist

| Check | Frequency | Method |
|---|---|---|
| `robots.txt` crawler policy | Quarterly or after policy changes | Review crawler blocks/allow rules against current official docs. |
| Search Console performance | Monthly | Inspect Web search traffic and page eligibility. |
| Bing Webmaster Tools / IndexNow status | Monthly | Confirm important URLs are indexed and refreshed. |
| AI crawler logs | Monthly | Search logs for `OAI-SearchBot`, `PerplexityBot`, `Claude-SearchBot`, `Bingbot`, and `Googlebot`. |
| Brand/entity answers | Monthly | Ask ChatGPT, Perplexity, Claude, Copilot, and Google about the brand/product. |
| Citation accuracy | Monthly | Capture where AI systems cite the site and whether answers are correct. |
| Content freshness | Quarterly | Refresh statistics, screenshots, pricing, product details, and citations. |

---

## Sources and notes

Commercial AI-visibility platforms such as SE Ranking, Profound, Semrush, Otterly.ai, Peec AI, and Scrunch AI publish useful research, but their studies should be treated as **directional**. They often measure correlations in specific query sets, date ranges, and tracked datasets. Use them to prioritize experiments, not as proof of exact ranking formulas.

## References

[1]: https://developers.openai.com/api/docs/bots "OpenAI Crawlers Documentation"  
[2]: https://docs.perplexity.ai/docs/resources/perplexity-crawlers "Perplexity Crawlers Documentation"  
[3]: https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler "Anthropic crawler documentation"  
[4]: https://developers.google.com/search/docs/appearance/ai-features "Google Search Central: AI features and your website"  
[5]: https://www.bing.com/webmasters/help/webmaster-guidelines-30fba23a "Bing Webmaster Guidelines"  
[6]: https://developers.google.com/search/docs/fundamentals/ai-optimization-guide "Google Search Central: Optimizing your website for generative AI features on Google Search"  
[7]: https://www.bing.com/indexnow "Bing IndexNow Documentation"  
[8]: https://seranking.com/blog/how-to-optimize-for-chatgpt/ "SE Ranking: How to optimize for ChatGPT"  
[9]: https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool "Anthropic Claude web search tool documentation"  
