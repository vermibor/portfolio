+++
title = 'AI Concierge'
date = 2025-12-07
draft = false
badges = ["n8n", "AI", "weekend-project", "automation"]
featureimage = "featured.jpg"
heroStyle = "background"
+++

So here's a problem I never thought I'd automate away: figuring out what to do on weekends.

Living in a small village has its perks – quiet streets, friendly neighbors, that whole peaceful countryside vibe. But when Friday evening rolls around and you realize you have zero plans? Gardening is always an option but maybe there is something more interesting. My wife and I would end up in this Thursday/Friday night ritual where I'd spend an hour or two hunting through Facebook events, local forums, and random websites trying to find *something* interesting happening within reasonable driving distance.

The local social media groups? They're mostly people selling furniture and complaining about potholes. Not exactly a goldmine for weekend inspiration.

## Enter: The Weekend Concierge

Last weekend I finally had enough time and decided to build something about it. I fired up n8n (my favorite automation tool these days) and spent a few hours putting together what I'm calling my "AI concierge" agent.

Here's what it does: every week, it automatically searches for events, activities, and interesting stuff happening in our area. Then it uses AI to filter through everything based on what my wife and I actually like – no random concerts, networking events or kids' birthday party venues, thank you very much. Finally, it packages everything into a neat little newsletter that lands in our inbox. And it learns to be better each time.

## How It Works

The workflow isn't rocket science, (it is a simple recommendation engine after all using RAG architecture) but it feels like magic when it works:

1. **Data gathering** – It pulls events from various sources (local websites, event aggregators, even some scraped content from regional tourism pages)
2. **AI filtering** – This is what makes the difference. I feed all the raw event data into an AI model with a prompt about our preferences: outdoor activities, live music, food markets, anything cultural but not too fancy, that kind of thing. And it is also linked with a database of past events we liked the most.
3. **Newsletter generation** – The filtered results get formatted into a readable email with dates, locations, and why the AI thinks we might like each thing
4. **Weekly delivery** – Every Wednesday morning, there it is in our inbox

## The Best Part

I don't think about it anymore. That's the whole point. No more "I guess we'll just stay home again" weekends because I forgot to plan ahead.

And honestly? The AI does a better job than I did manually. It catches stuff I would've missed, and it's way better at connecting the dots between what we liked before and what might interest us next.

## Worth the Time?

Took me maybe 3-4 hours to set up including all the testing and tweaking. I've already saved more time than that just in the past month, and we've actually *done* more interesting things because the planning friction is gone.

But hold on .... can I make it as a personal, super focused event recommendation service to wider public that will look at individual preferences? I may spend more time on enhancing this simple flow. In the meantime: <a href="n8n/newsletter_simple.json" download>download this n8n workflow</a> and set this up for yourself. Just add your credentials and change preferences in 'parameters' step. Enjoy.
