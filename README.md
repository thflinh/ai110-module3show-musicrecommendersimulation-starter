# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

This project is a CLI-first simulation of a music recommender that scores each song against a user taste profile, then ranks the catalog to return the top matches. It uses interpretable rules (genre, mood, and energy similarity) and prints human-readable reasons for each recommendation.

---

## How The System Works

Real-world systems like Spotify and YouTube combine collaborative filtering (learning from what similar listeners played, skipped, and saved) and content-based filtering (using song attributes such as mood, tempo, and energy). This simulation focuses on the content-based part so the logic is easy to inspect. My recommender prioritizes genre and mood alignment, then uses an energy closeness rule so songs near the target vibe earn more points than songs that are simply high-energy. In practice, each song is scored individually, then the full list is ranked and the top `k` are returned.

Features used in this simulation:
- Song features: `genre`, `mood`, `energy`, `tempo_bpm`, `valence`, `danceability`, `acousticness`
- UserProfile-like fields (functional path): `genre`, `mood`, `energy`
- UserProfile fields (OOP path): `favorite_genre`, `favorite_mood`, `target_energy`, `likes_acoustic`

Algorithm recipe:
- `+2.0` points for genre match
- `+1.0` point for mood match
- `+ (2.0 * (1 - abs(song_energy - target_energy)))` for energy similarity
- Optional `+0.5` acoustic preference match in OOP mode

Why scoring and ranking both matter:
- Scoring decides how well one song fits one user.
- Ranking turns all per-song scores into an ordered recommendation list.

---

### CLI Verification Screenshot
<img width="961" height="391" alt="Screenshot 2026-04-15 185320" src="https://github.com/user-attachments/assets/c6c60376-6286-4d33-9d57-d118f29c967b" />


---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

Experiments run:
- Tested three profiles in CLI output: **High-Energy Pop**, **Chill Lofi**, and **Deep Intense Rock**.
- Performed a weight-shift thought experiment: doubling energy impact would increase cross-genre songs that match intensity, while reducing strict genre lock-in.
- Compared with and without mood matching (manual reasoning): removing mood causes more false positives where songs are energetic but emotionally off.
- Observed that genre-heavy weighting can repeatedly place songs like `Gym Hero` near the top for upbeat profiles.

### Evaluation Screenshots by Profile
<img width="756" height="509" alt="Screenshot 2026-04-15 185415" src="https://github.com/user-attachments/assets/bdc41a07-5576-423c-831e-44085fe13bdf" />
<img width="752" height="494" alt="Screenshot 2026-04-15 185439" src="https://github.com/user-attachments/assets/40df7053-81ef-46e5-8388-1deb62f18188" />
<img width="838" height="503" alt="Screenshot 2026-04-15 185555" src="https://github.com/user-attachments/assets/0b708947-032e-414e-8e32-67c1e7b5d6c6" />

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

Current limitations and risks:
- Small catalog (18 songs) limits diversity and creates repeated top picks.
- No collaborative signals (likes, skips, playlists), so it cannot learn from community behavior.
- No context awareness (time of day, activity, language, recency).
- Fixed weights may over-prioritize genre and under-represent nuanced taste shifts.

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this

Building this showed me how quickly a simple formula can feel "smart" once scores are ranked and explained, even though the model is shallow. The strongest lesson was that closeness-based numerical scoring (energy similarity) is more realistic than raw higher/lower comparisons because user taste often targets a range, not an extreme.

I also saw how bias appears through design choices, not only data size. If genre gets too much weight, the system keeps recommending familiar buckets and misses adjacent songs that match mood and intensity. In real systems, that creates filter bubbles unless diversity constraints and richer feedback loops are added.


---

## 7. `model_card_template.md`

Combines reflection and model card framing from the Module 3 guidance. :contentReference[oaicite:2]{index=2}  

```markdown
# 🎧 Model Card - Music Recommender Simulation

## 1. Model Name

Give your recommender a name, for example:

> VibeFinder 1.0

---

## 2. Intended Use

- What is this system trying to do
- Who is it for

Example:

> This model suggests 3 to 5 songs from a small catalog based on a user's preferred genre, mood, and energy level. It is for classroom exploration only, not for real users.

---

## 3. How It Works (Short Explanation)

Describe your scoring logic in plain language.

- What features of each song does it consider
- What information about the user does it use
- How does it turn those into a number

Try to avoid code in this section, treat it like an explanation to a non programmer.

---

## 4. Data

Describe your dataset.

- How many songs are in `data/songs.csv`
- Did you add or remove any songs
- What kinds of genres or moods are represented
- Whose taste does this data mostly reflect

---

## 5. Strengths

Where does your recommender work well

You can think about:
- Situations where the top results "felt right"
- Particular user profiles it served well
- Simplicity or transparency benefits

---

## 6. Limitations and Bias

Where does your recommender struggle

Some prompts:
- Does it ignore some genres or moods
- Does it treat all users as if they have the same taste shape
- Is it biased toward high energy or one genre by default
- How could this be unfair if used in a real product

---

## 7. Evaluation

How did you check your system

Examples:
- You tried multiple user profiles and wrote down whether the results matched your expectations
- You compared your simulation to what a real app like Spotify or YouTube tends to recommend
- You wrote tests for your scoring logic

You do not need a numeric metric, but if you used one, explain what it measures.

---

## 8. Future Work

If you had more time, how would you improve this recommender

Examples:

- Add support for multiple users and "group vibe" recommendations
- Balance diversity of songs instead of always picking the closest match
- Use more features, like tempo ranges or lyric themes

---

## 9. Personal Reflection

A few sentences about what you learned:

- What surprised you about how your system behaved
- How did building this change how you think about real music recommenders
- Where do you think human judgment still matters, even if the model seems "smart"

