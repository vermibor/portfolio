+++
title = 'Bob the (AI) Builder'
date = 2026-04-20
draft = false
badges = ["AI", "n8n", "Telegram", "Jira"]
featureimage = "featured.jpg"
heroStyle = "background"
+++

I'm currently building a summer house at the seaside. If you've ever tackled a full-scale construction project, you know it's not simply about picking out tile colors. It’s an exercise in extreme project management: chasing architects, chasing formal authority approvals, navigating local community consultations, wrangling mortgages, and negotiating with a half-dozen vendors simultaneously.

Very quickly, the scale of the project started growing out of control. With so many dependencies and parallel activities happening at once, I felt like I was losing my grip on the timeline. Given my background in project management, I knew exactly what was happening and what was needed: a proper system to manage the chaos.

Instead of just installing another app, I decided to build a custom AI Assistant to act as our personal project manager. 

## The Architecture: Tying the Stack Together

The goal was to build a system that had access to all our documentation, understood construction data, and tracked progress. Most importantly, the interface had to be dead simple to use while standing on a muddy plot of land holding a phone in one hand.

Here is how the stack works:

1. **The Frontend (Telegram):** I created a Telegram bot named 'AI Builder'. Telegram is ideal for this because it naturally handles text, voice memos, and file uploads directly from a phone.
2. **The Brain (n8n & Gemini):** Whenever a message is sent to the bot, it triggers an n8n workflow. If the input is voice, n8n passes it to Google Gemini to transcribe the audio into a clean text prompt.
3. **The Agent:** Once transcribed, the prompt is passed to an AI Agent within n8n. This agent is wired up to a local database, my Google Drive (where the heavy documentation lives), and Jira. Yes, I use Jira for a home construction project—and it works.
4. **The Output:** The agent executes the necessary operations and fires a confirmation response back to us in the Telegram chat.

### A Note on Security
Because Telegram bots are essentially public if someone finds the handle, Step One of the n8n logic is a strict security gate. The workflow immediately drops any traffic that doesn't originate from my or my wife's authorized Telegram IDs. 

## Real-World Commands 

Having this system means I don't have to open a laptop to manage the project. From the plot, I can send voice commands to the bot like:

* *"Review the house's technical documentation and add a comment to the roof cover task detailing exactly which types of materials we are approved to use."*
* *"Add a task for me called 'Review contract with electrician' and add a comment: 'make sure it covers the PV installation'."*
* *(Uploading a batch of photos)* *"Upload these pictures to the 'ground leveling' task."*
* *"I have a plumber coming next month. Which tasks do we need to complete before he arrives? Once he is done, what other tasks will we be able to start"*

The agent retrieves the right Jira ticket, pulls context from Drive if necessary, and handles the data entry.

## Multiplayer Mode: A Shared Brain

The biggest advantage is that the 'AI Builder' bot is a shared group chat between me and my wife. 

This creates a single, visible source of truth. We can both add new tasks, drop in comments, save documents, or log budget updates in one place. We can assign tasks to each other seamlessly. Because it all syncs back to Jira, nothing gets lost in a scrolling chat history. It completely removed the "Wait, I thought you called the contractor?" conversations.

## The Technical Roadblocks

Building this wasn't instantly smooth. A few specific headaches stood out:

**Wiring the Agents:** I chose to manually connect all the necessary tools to the n8n agents myself. Call me paranoid, but I still don’t entirely trust external 'agent-in-a-box' solutions. I prefer the granular control of building the workflow myself, even if it takes longer to set up.

**Fighting Atlassian’s API:** Fixing the bugs between n8n and Jira swallowed a lot of time. The Jira API makes things unnecessarily complicated because the calling schema changes depending on the specific type of update you are trying to push. It is something different to update a data attribute of an issue and a description of an issue ... why ? ...

**The PDF Problem:** The extraction tool I’m using for the RAG system gives decent results for standard text contracts and permits. However, it completely fails at reading and interpreting architectural technical drawings. That's a bottleneck I'm still trying to figure out. 

Building your own AI project manager to build a house might sound like overkill, but the time saved on data entry and the mental clarity of having a single point of tracking has a great chance to pay off. We'll see. I am only at the beggining of that journey.
