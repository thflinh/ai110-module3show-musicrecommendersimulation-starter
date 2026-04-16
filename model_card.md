# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name

**VibeBridge 1.0**

---

## 2. Intended Use

This recommender suggests the top 5 songs from a small catalog based on a user profile (genre, mood, and target energy). It is designed for classroom exploration of recommendation logic, not production use with real users.

---

## 3. How the Model Works

The model compares each song to the user profile and gives points for matching features. A song gets the biggest boost for matching genre, a smaller boost for matching mood, and a variable boost for being close to the user's target energy. In the OOP path, it can also add a small bonus when acoustic preference matches. After scoring every song, it ranks them highest to lowest and returns the top results with explanation text.

---

## 4. Data

The dataset contains 18 songs in `data/songs.csv` with attributes: title, artist, genre, mood, energy, tempo, valence, danceability, and acousticness. I expanded the starter 10-song catalog by adding 8 songs across more genres (metal, classical, latin, country, chiptune, world, and drum and bass). The dataset still reflects a narrow, synthetic view of music taste and lacks regional, language, and era diversity.

---

## 5. Strengths

The system works well for clear profiles such as high-energy pop and chill lofi because genre + mood + energy combine into intuitive rankings. It is also transparent: each recommendation includes reasons, making it easy to audit the logic and debug surprising outputs.

---

## 6. Limitations and Bias

The model over-relies on fixed weights, which can favor dominant genres in the catalog and create filter-bubble behavior. It ignores listening history signals like skips, replays, and playlist context, so it cannot adapt to evolving preferences. Because the dataset is small, the same songs can surface repeatedly even when users have different tastes.

---

## 7. Evaluation

I tested three user profiles: High-Energy Pop, Chill Lofi, and Deep Intense Rock. I checked whether top songs matched intuitive vibe expectations and whether explanation reasons made sense. I also ran a sensitivity check by considering a weight shift (more energy emphasis, less genre emphasis) and noted how it would improve cross-genre discovery while reducing strict genre matching.

---

## 8. Future Work

- Add collaborative signals (likes/skips/playlists) alongside content features.
- Introduce diversity constraints so the top 5 are not all from one style cluster.
- Personalize weights per user instead of using one global recipe.

---

## 9. Personal Reflection

The biggest learning moment was seeing that recommendation quality depends as much on design choices as on code correctness. A simple weighted formula can produce outputs that feel meaningful, but it can also lock users into narrow loops when one feature dominates. This project made me more aware that real recommenders need both good ranking math and product guardrails for fairness, diversity, and user control.
