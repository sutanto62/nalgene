# Community sentiment

## What this angle covers

- Developer forums (Reddit, Hacker News, dev.to, Discourse communities)
- Conference talks and podcasts discussing the technology
- Practitioner blog posts (someone describing their own experience, not a vendor's marketing post)
- Structured surveys (Stack Overflow Developer Survey, State of JS/CSS/etc., ThoughtWorks Technology Radar)
- Social media threads with substantive technical discussion, not just reactions

Everything found here is **sentiment**, never fact. Report what people are saying and how widespread it seems, not whether they're right. Actively look for both enthusiasm and backlash — a search biased toward one phrasing ("why X is great") will only surface one side; search the opposite phrasing too ("why X is overrated", "X problems").

## Watch for astroturfing

Content that looks grassroots but is vendor-sponsored, a paid post, or from an account with an undisclosed financial interest (a consultancy that sells migrations to the technology in question, for instance) is marketing, not sentiment. Flag it as such when detectable, and don't let it inflate the apparent size of genuine community enthusiasm.

## How to search it

```text
"<technology>" reddit OR "hacker news"
"<technology>" problems OR overrated OR "why we moved away"
"<technology>" "state of js" OR "developer survey" <year>
"<technology>" conference talk <year>
```

Note roughly how many independent voices you're seeing, not just whether you found supportive quotes — ten mentions across the same three threads is different from the same sentiment echoed independently across unrelated communities.
