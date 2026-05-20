# Vibe coding through high school

I'm 16, from Helsinki, and over the past few months I've built three Android apps I now use every day. My actual coding ability when I started was close to zero. I knew what a function was, vaguely. Everything I've shipped runs on a different kind of skill — prompting, taste, and being stubborn about which features get to stay.

The three apps are one suite. I call it Limecore Nexus OS.

StudyDesk is a mobile-first study planner on React 19 and Vite 7\. The question it's trying to answer isn't "what do I have to do?" but "what do I do right now?" Limelog is a workout tracker for periodized strength programs, React 18 with Zustand. Nexus Command Center is the hub — a dashboard on Dexie and IndexedDB locally, pulling finance, studies, fitness and tasks into one place.

All three are independent apps, but they write to the same Supabase Postgres backend with Row-Level Security keeping each user's data separate. Anything I log in Limelog shows up in Nexus within about 1.5 seconds via Postgres Realtime. Same with a Pomodoro session in StudyDesk. That part — three apps sharing one live data graph — is the thing I can't find anywhere else on the consumer side, and it's the reason any of this is worth writing about.

## What Claude was great at

I didn't expect Claude to be better at architecture than at code. I came in assuming it was a fancy autocomplete and I'd be making the decisions. What actually happened is that it pushed back on bad ideas before I'd written a line.

The clearest example was sync. When I first wanted Limelog data to show up in Nexus, I assumed I'd have to build an API on the Limelog side, scrape it from Nexus, and stitch the two copies together somehow. It looked at my setup and basically said: you have two apps and one Supabase project, so have both apps write to the same tables and let Postgres Realtime push updates to whoever's subscribed. Use RLS to keep per-user data separated. That was it.

That single redirect saved me a week and a much worse architecture. I would have built scrapers. I have no idea why, but I would have.

Pure math it gets right on the first try. The GPA calculator in StudyDesk toggles between the US 4.0 scale and the IB 1-7 scale — I described the conversion rule and got working code back, no bugs, no second pass. Same with the financial data in Nexus. Finnhub for stocks, Yahoo as a fallback, CoinGecko for crypto, all with retry logic if a provider goes down. I knew only that Finnhub existed before that conversation. Claude found them, wired them up, and handled the failure cases. That would have taken me days even if I'd had real coding skill.

The exercise library is another good one. Limelog ships with 140+ built-in exercises tagged by movement pattern, equipment, and muscle group. I described what I wanted and got the structured library back. If I'd had to type all 140 by hand I would have, but I'm glad I didn't.

Looking back, the pattern is pretty clear. Claude was strongest exactly where I had a clear, finite problem. "Here is the rule, write the code." "Here are the requirements, find me the tools." When the question had a right answer, it found the right answer faster than I could have looked it up.

## What Claude was terrible at

Anything that needs taste or eyes is still not there.

UI was the first wall I hit. Claude's default visual instincts are bad. Not technically broken — the components render, the spacing is sort of fine — but the result always looks like every other AI-built app you've seen. I ended up designing in a separate tool, Kombai, which is purpose-built for UI. I'd get it to generate theme variations and wireframes, pick one, then feed that into Claude for the implementation. Even then I had to push back constantly to keep it from drifting back toward the safe default look.

The clearest failure was an opening animation I wanted for Nexus. I had this whole idea of a futuristic vault-opening sequence when the app launches. I described it carefully. Claude Code told me it had implemented it. It hadn't. The animation file existed, the function was there, but the actual animation step in the middle was empty. Nothing happened on screen. It told me it was done when it wasn't, and I caught it after updating it to my phone. I tried again in Claude Design and got further, but the result was choppy and never quite felt right. I shelved the animation. I don't think it's something you can vibe-code yet — it'll probably need another model generation.

Context drift was the other constant problem, especially when I was building StudyDesk on the free tier with Sonnet. I'd ask Claude to fix one component and a completely unrelated view would silently break. The thing it changed wasn't related to the thing that broke, and what it told me it had done didn't always match what it had actually done. So I'd spend an hour fixing the original bug, find a screen I hadn't touched in a week now blank, and start the cycle over.

Honestly Claude is great when there's one right answer and bad when there's a feel involved. Whenever I had to get across "this should look like a vault, not a fade" or "the workouts page should feel heavy, not airy," I was working against the model's defaults the whole time.

## The things only I could do

The first version of StudyDesk had way too many features. I just like adding features, I think. There was a habit tracker, a streak counter, an analytics page nobody was ever going to look at. None of it was being used, by me or anyone else. I got some feedback on it and cut everything except the exam and assignment calendar, the Pomodoro timer and the next up page, because those were the only three things I was actually opening the app for. That wasn't an engineering call. It was a "what did I open today" call, and only I could make it.

Limelog exists because I spent most of last basketball season on the sidelines with back stress injuries. I wanted to get back to working out, even if it was something lighter in the weight room and asked my AI if there was an app that would work in my situation or if it were easier just to build one from scratch. The injury-restriction flags and the phase awareness in Limelog aren't things I picked up from competitor research. They're features I needed because my own training was broken and nothing else handled it.

The design direction too. Limelog looks brutalist because heavy lifting feels brutalist. You don't want a soft-rounded button with a pastel gradient when you're about to fail a rep. StudyDesk looks more like a book because studying is calm and the UI shouldn't be yelling. Both of those came from feeling whether the app matched the mood of what it was for, not from following a design system.

This part doesn't transfer to AI yet. Claude doesn't know how my back felt last March or that I check StudyDesk between classes and Limelog under bad gym lighting. The product decisions came out of those situations, and I don't think you prompt your way there without living through them.

## Things I still don't understand about my own apps

The codebase is, for me, a partially-understood black box. The apps work. I've used them every day for months, so I trust the code. I couldn't explain any of it to a real engineer though.

A few specific examples. There's a drift-correction thing on the Pomodoro timer that recomputes elapsed time when the app comes back from the background, so the timer doesn't desync if I lock my phone for ten minutes. I asked for it, Claude built it, I tested that it works. I could not walk you through the function.

There are last-write-wins merge utilities and BEFORE UPDATE database triggers handling the multi-device conflict cases. What happens if I edit the same workout on my phone and my tablet within seconds of each other. They handle it. How they handle it is somewhere in Claude's head and the repo, not in mine.

The security layer is similar. PBKDF2-SHA256 hashing the local PIN with 250,000 iterations, constant-time comparison. I know what those words mean now, kind of. I didn't pick the iteration count. I trusted Claude that 250,000 was right, and "verified" by asking Claude again in a fresh context, which I realize doesn't really count as independent. There's also a pre-commit hook that scans my commits for credentials before they hit GitHub. Claude wrote it. I just run it.

The most honest one is that I barely understand the terminal commands I use to push code and build the Android version through Capacitor. I have a list of commands in my head, because I've used them so many times, but don’t really understand what they mean. If something errors out, I paste the error back into Claude. That is the entire workflow.

## Would I do it again

Yeah.

What vibe coding actually changed for me isn't that I'm faster, although I am. It's where the bottleneck sits now. The bottleneck used to be syntax, and you had to spend years on it before you could ship anything real. Now syntax is basically free. The bottleneck moved up the stack — architecture, product decisions, being honest with yourself about which features matter and which ones you just like building.

Those are things I can get better at by actually doing them. So that's what I'm doing.

## Production footprint

- Limecore: [limecore.vercel.app](http://limecore.vercel.app)  
- Nexus Command Center: [github.com/Limekana/nexus-command-center](http://github.com/Limekana/nexus-command-center)  
- StudyDesk: [github.com/Limekana/studydesk](http://github.com/Limekana/studydesk)  
- Limelog: [github.com/Limekana/limelog](http://github.com/Limekana/limelog)

