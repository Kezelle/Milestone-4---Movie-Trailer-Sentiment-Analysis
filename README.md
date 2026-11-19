# Milestone 4 - Avengers End Game Movie Trailer - Sentiment Analysis
This repository will show how we will perform sentiment analysis for the movie trailers that I got from Kaggle.

https://www.kaggle.com/datasets/nitaisatapathy/youtube-movie-trailer-comments

# Can a Free Algorithm Understand *Avengers* Fans, or Do You Need an AI? 🎬🤖

An experiment comparing old-school text tools against modern Large Language Models (LLMs) on *Avengers: Endgame* YouTube trailer comments.

---

## What is this project about?

Every day, millions of people leave comments online. Companies and creators use **Sentiment Analysis**—teaching computers to read text and decide if it is **Positive**, **Negative**, or **Neutral**—to understand how audiences feel.

Today, modern AI models like Claude or ChatGPT are great at reading between the lines, catching sarcasm, and understanding slang. **The catch?** They take longer to run and cost money for every batch of text you send them. Meanwhile, older, simpler text tools are **100% free** and run in a split second.

This project asks a practical question: **When is a free, simple tool "good enough," and when is it actually worth paying for an AI?**

---

## Meet the Contenders

To find out, I tested three different levels of technology on 500 real *Avengers: Endgame* trailer comments that I read and labeled by hand to create an "answer key."

### 1. The "Dictionary Look-Up" Tools (Free & Instant)
These tools don't actually "read" sentences. They just look up individual words in a built-in dictionary of positive and negative words and add up the score.
* **VADER:** Built specifically for social media (it understands emojis, ALL CAPS, and exclamation marks). *Custom tweak: I taught it that words like "doom" and "doomsday" are neutral movie titles, not bad things.*
* **TextBlob:** A standard, general-purpose word scorer.

### 2. The "Pattern Learner" (Free & Fast)
* **Naïve Bayes (Classic Machine Learning):** Instead of using a pre-made dictionary, this model learns from examples by counting which words show up most often in positive vs. negative comments. I tested two versions:
  * **Trained on AI guesses:** Learned from ~3,500 comments automatically labeled by VADER.
  * **Trained on human labels:** Learned directly from my 500 hand-labeled comments.

### 3. The Modern AI (Smart, Slower, Costs Pennies)
* **Claude Haiku 5.5:** A lightweight, modern Large Language Model given a simple prompt explaining the *Avengers* context and told to watch out for sarcasm.

---

## The Experiment: Cleaning the Data

Starting with **66,802 raw YouTube comments**, I filtered for *Avengers* trailers (**8,409 comments**), kept only English comments, and stripped out links, `@mentions`, and hashtags while keeping emojis and punctuation.

I also ran a "before and after" test to see how strictly cleaning the data affects the results:
* **Run 1 (Light Cleanup — 5,293 comments):** Kept almost all English comments, including super-short reactions and memes.
* **Run 2 (Strict Cleanup — 4,848 comments):** Filtered out comments where fewer than 70% of the words were recognized English or Marvel slang (like *Thanos*, *MCU*, or *rewatch*). This removed 445 messy comments, leaving longer, more detailed reviews.

---

## The Scoreboard

Here is how each tool performed against my 500 human-labeled comments, plus a special **"Hard Cases" test** made up of the 30 trickiest comments that stumped the basic models:

| Model | Type | Overall Accuracy | Balanced Score (Macro-F1) | Tricky Comments (30 Hard Cases) | Time per 1,000 Comments | Cost per 1,000 Comments |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **VADER** | Dictionary | 48.8% *(55.8% in Run 1)* | 0.457 *(0.531 in Run 1)* | 3.3% (1/30) | **0.33 sec** | **$0.00** |
| **TextBlob** | Dictionary | **59.2%** | **0.489** | 0.0% (0/30) | 0.46 sec | **$0.00** |
| **Naïve Bayes (VADER-trained)** | Pattern Learner | 41.6% | 0.375 | 3.3% (1/30) | 0.41 sec | **$0.00** |
| **Naïve Bayes (Human-trained)** | Pattern Learner | 50.4% | 0.478 | **23.3% (7/30)** | < 0.45 sec | **$0.00** |
| **Claude Haiku 5.5** | Modern AI | *[Pending]* | *[Pending]* | *[Pending]* | *[Pending]* | ~$0.01 |

*(Note: "Balanced Score" ensures a model can't cheat by guessing "Neutral" on every single comment to get a high accuracy.)*

---

## What Went Wrong? (Why Simple Tools Struggle)

Looking at the mistakes made by the free tools revealed four hilarious and very human blind spots:

1. **Slang Flips the Meaning:**
   * *Comment:* `"Crazy getting this recommended in December 2025! (Anybody here? 🔥🔥)"`
   * *What happened:* Humans know `"Crazy"` + fire emojis means excitement (**Positive**). VADER and TextBlob saw `"Crazy"` and marked it **Negative**.
2. **Happy Tears vs. Sad Tears:**
   * *Comment:* `"Today is the day endgame happen 😢 future generation will never understand this"`
   * *What happened:* The fan is nostalgic and moved (**Positive**), but VADER saw the crying emoji (`😢`) and assumed they were upset (**Negative**).
3. **Swearing Out of Excitement:**
   * *Comment:* `"I think that this shit is going to mark a new era in the cinema history OMG!"`
   * *What happened:* The word `"shit"` triggered an automatic **Negative** score, completely missing the hype.
4. **Famous Movie Quotes:**
   * *Comment:* `"Some people forget this trailer, But not us, Not us"`
   * *What happened:* Because this iconic Captain America quote contains `"forget"` and `"not"`, simple models marked a neutral fan quote as **Negative**.

---

## Key Takeaways: Which Tool Should You Pick?

* **Quality beats quantity in training data:** Training a simple model on **500 human-labeled comments** beat training it on **3,478 auto-labeled comments** by nearly 9% in accuracy—and made it **7x better** at handling tricky comments.
* **Use Dictionary Tools (VADER)** if you need to scan millions of social media posts in real time for free to get a rough pulse on public mood.
* **Use Classic Machine Learning** only if you have thousands of human-labeled examples and strict privacy rules that prevent sending data to outside AI companies.
* **Use a Lightweight AI (Claude Haiku)** when nuance matters. At roughly **$0.01 per 1,000 comments**, an LLM easily pays for itself when you need to understand slang, emojis, sarcasm, and pop-culture references.

---

## What's in This Repository?

* `notebook.ipynb` — The complete step-by-step Python code for cleaning the data, training the models, and generating charts.
* `Avengers_Movie_trailer_review.csv` — The cleaned dataset of English *Avengers* trailer comments.
* `manually_validated_sample - with human labels.csv` — The 500 comments labeled by hand used as our ground-truth answer key.
* `data/hard_cases_30.csv` — The 30 toughest comments used to stress-test the models.
* `results/metrics.csv` & `results/charts/` — Exported performance tables and comparison bar charts.

---

## How to Run This Project Yourself

1. **Install the required Python libraries:**
   ```bash
   pip install pandas numpy matplotlib seaborn nltk textblob vaderSentiment wordcloud scikit-learn langdetect anthropic python-dotenv
   ```

2. **Add your API key (for the Claude AI step):**
   Create a file named `.env` in the main folder and add your Anthropic key:
   ```env
   ANTHROPIC_API_KEY=your_api_key_here
   ```

3. **Run the notebook:**
   Open `notebook.ipynb` in Jupyter Notebook or VS Code and run the cells from top to bottom.

