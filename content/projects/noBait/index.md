+++
title = 'The noBait Experiment'
date = 2026-01-06
draft = false
badges = ["AI", "Agents", "API", "weekend project"]
featureimage = "featured.jpg"
heroStyle = "background"
+++

I’ve spent the last week seeing exactly how much heavy lifting I could offload to an AI. 

The goal was simple: Build a Chrome extension called **noBait** to nuke clickbait and distracting thumbnails from my browser. But the *real* goal was to stress-test the new **GLM 4.7** model using **OpenCode** to see if it could actually handle a project end-to-end without me having to babysit every line of syntax.

## Tooling & Setup

I didn’t want to just copy-paste snippets from a chat window. I wanted an "agentic" workflow. 

Early on, I bounced around between a few different AI CLI tools, trying to find the right "vibe." After a few failed starts, I decided to stop window shopping and just master one tool: **OpenCode**. It became my primary interface for everything. 

The split between 'Plan' and 'Build' agents seem to work perfectly well. Usage of Agent.md acted as the project's "brain"—defining the architecture and coding standards. By feeding this into OpenCode, the model (GLM 4.7) actually had a map of the territory. It wasn't just guessing; it was working within the constraints of my existing file structure.

## Testing GLM 4.7 Limits

Most people use these models for simple scripts. I have a feeling this try was a little bit more demanding, where this new model had a chance to shine.

**What worked surprisingly well:**
* **Context Retention:** Because of the `Agent.md` setup, I didn't have to keep reminding the model about my specific selectors. It "remembered" the project's soul. And it general it managed this really well, even when comparing to other models.
* **Logic over Syntax:** GLM 4.7 is sharp. When I asked it to handle various websites loading mechanisms, it correctly suggested a MutationObserver instead of a lazy `setTimeout` loop.

That said, it wasn't a perfect solo run. There were a few moments where GLM got stuck in a loop and I had to pivot. In those cases, I brought in **Grok** and **Gemini** to provide a second opinion or break a logic stalemate. Having that "council of models" was key to getting past the tricky parts.

## Why I’m Calling it "Done"

The project is about 99% there. If you [check the repo](https://github.com/vermibor/noBait/tree/main), the extension works. It cleans up the UI, the popup is functional, and the core logic is solid. 

It still needs some polish for cross-browser usage and better multi-language support, but I’m marking it as complete for a very specific reason. During my final push, I discovered the **'Oh my OpenCode'** orchestration add-on.

Seeing what that orchestrator can do made me realize that **noBait** has served its purpose as a learning ground. Between the free tier limits on my OpenRouter API and this new discovery, I’ve lost interest in tweaking secondary functions for cross-browser support. I see a much more challenging and interesting project on the horizon that actually demands that kind of advanced orchestration.

**noBait** proved that with a well-structured `Agent.md` and the right focus on your tooling, you can move incredibly fast. Now, it's time to take what I learned and apply it to something even bigger.

***

**Repository:** [vermibor/noBait](https://github.com/vermibor/noBait/tree/main)  
**Tooling:** OpenCode + GLM 4.7 (with assists from Grok & Gemini)
