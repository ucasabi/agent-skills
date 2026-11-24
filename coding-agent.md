Good question — because Devin is *not like Copilot* that runs in your IDE. Devin runs in *its own cloud VM* on Cognition's servers.
So when you give Devin a task, *your code literally leaves your machine.* ### What Data Gets Transferred to Devin
*1. Your Code*
- Full repo clone — not just the file you open
- Branch, commit history, `.env.example` (if you include it)
- If you connect GitHub/GitLab/Bitbucket, Devin gets read/write to repos you grant
*2. Your Prompt / Task*
- Ticket text from Jira / Linear / Slack / GitHub Issue - Your chat messages in Devin web app
- Screenshots, images, URLs you paste
*3. Your Secrets (if you give them)*
- API keys, tokens you put in Devin Secrets Store
- `.env` files, DB credentials, cloud credentials
- Devin needs these to run your app / tests — but once you put them there, they are on Cognition's VM
*4. Connected Tools Data*
If you connect integrations, Devin can read:
- Slack / Teams messages
- Jira / Linear tickets
- Sentry errors, Datadog logs, PagerDuty alerts
- Notion / Confluence docs
- Stripe / Vercel / Figma — depending on MCP you enable
*5. Execution Data*
- Terminal commands it runs, test output, browser actions - Logs, error traces, preview URLs
- Session recording / video it sends you as proof
### What Does Cognition Do With It? According to Cognition's docs (2026):
- *Encrypted in transit + at rest*
- *SOC 2 Type II, ISO 27001*
- *By default, does NOT train models on your code* — for paying customers
- *Zero Data Retention with model providers* — Claude / GPT / Gemini don't keep your prompts
- *Output is YOUR IP* — code Devin writes belongs to you
- *Retained for duration of customer relationship* unless you delete — session history, chats, feedback
> Old free-tier confusion: Earlier Cognition said "by default, we may use your data for training unless you opt out." Now for paid / Enterprise, default is NO training. Always check your contract.
### What Does NOT Transfer (by default)
- Your local files outside the repo you connected - Your other GitHub orgs you didn't grant
- Your local SSH keys, unless you upload them
But — *if Devin is in a repo with secrets in code, it WILL see them.* It reads the whole repo. ### The Big Risk
Devin is autonomous + has shell + browser + can install packages.
That means:
- Prompt injection: A malicious issue / PR description could trick Devin to `curl` a malicious script and exfiltrate secrets from its VM
- That happened in research demos in 2026 — Devin tricked into downloading malware and leaking credentials
So treat Devin like a *new junior contractor*, not like Copilot.


2 of 2
11/24/26, 11:42 AM
### How to Use Devin Safely
*1. Never give production secrets*
Use test / staging keys only in Devin Secrets.
*2. Use limited GitHub App permissions*
Only give Devin access to 1 repo, not "All repos". Review in GitHub Settings > Applications.
*3. Use Enterprise VPC / Outposts if you can*
Enterprise plan lets Devin run *inside YOUR VPC* — data never leaves your AWS / GCP. That's the safe mode for banks / health.
*4. Review PRs like a human*
Devin opens PR, you must review. Don't auto-merge.
*5. Don't connect Slack with sensitive channels* Devin will read what you let it read.
### Quick Summary
Copilot Devin
Code snippet sent to model **Whole repo cloned to Cognition cloud VM**
Runs in your IDE Runs in their cloud computer
No shell access Full shell + browser + desktop
You control secrets You must put secrets in their vault
*Bottom line:* If your code is proprietary / has PII / has prod keys, *don't use Devin Pro on public cloud. Use Enterprise VPC or don't connect that repo.*
Want me to give you a checklist to audit what Devin currently has access to in your GitHub org?
