# Tinker Notes

A study companion for an Arduino / Tinkercad robotics course, built for one real learner, a friend taking a free vocational course. It turns his own class notes into a concept map and practical Tinkercad challenges.

> Live demo: _coming soon (GitHub Pages)_

## The problem

My friend takes notes after every class, but notes alone don't tell you whether you can actually *build* something. A general-purpose chatbot can quiz you, but it forgets everything between sessions and doesn't know what you've actually covered.

## Design decisions

- **The learner's notes are the only source of truth.** The app has no knowledge of the course syllabus, schedule or the instructor's slides. Concepts, challenges and hints are built only from what he wrote. His original text is never rewritten. If a note looks technically wrong, the app flags it and leaves the correction to him.
- **At most one clarifying question per note**, and he can always skip it.
- **Challenges grow like a real project.** Each Tinkercad challenge extends the previous one (a traffic light gains a potentiometer, then timing, then a pedestrian button…) and uses only concepts already in his notes.
- **Deterministic checking first.** Every challenge defines the exact Serial Monitor output. That output is compared line by line in plain JavaScript, with no AI involved. He also ticks off observable checks in the simulation.
- **Failures are diagnosed by type:**
  - *Application error*: the concept is in his notes but was applied wrongly. He gets a hint, never the solution.
  - *Knowledge gap*: the challenge needs something he hasn't learned yet. The app names the topic and **he decides** whether to learn it now. A mini lesson ends with him writing his own note, so new knowledge always enters through his words.
- **No grades.** Progress is tracked per concept: *Seen* (in his notes), *Practiced* (used in a challenge), *Mastered* (3 challenges solved without hints).
- **Error notebook.** Every failed attempt requires a short note on what he was thinking. That reflection is the most useful part.
- **Safety.** Only the simulator or low-voltage kits. The app never proposes anything connected to mains (230 V).

## Architecture

- A single static `index.html`, with no build step, no server and no account.
- **AI: Google Gemini, using the learner's own free API key** from Google AI Studio. The key is stored only in his browser's `localStorage` and never leaves his machine, except in requests to Google.
- **Data lives in the browser.** A weekly reminder prompts him to download a JSON backup, and he can restore it on any machine.

Constraints that shaped this: the course is free, so the tool had to be free for him too, and it couldn't depend on me running or paying for a server.

## Iterating with a real user

v1 covers notes → concept map → daily challenge → Serial check → diagnosis → error notebook → backup.
Planned after his feedback: AI review of pasted code, circuit screenshots, spaced review, and simplification tips after an attempt.

## Run it

Open `index.html` in Chrome or Edge, go to **Ajustes**, and paste a Gemini API key from <https://aistudio.google.com/apikey>.
