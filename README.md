
# 🔍 Reddit Emotion and Pattern Analysis with Silent Periods

This project performs **emotion pattern mining** on Reddit's `r/depression` subreddit, detecting emotional transitions and the effect of **silent periods** using NLP, Empath, and PrefixSpan. It visualizes emotional flows through **Sankey diagrams** and computes user activity statistics.

### 📌 Reference

Based on:  
[Tracing the Emotional Roadmap of Depressive Users on Social Media Through Sequential Pattern Mining](https://ieeexplore.ieee.org/document/9477614), IEEE, 2021.

---

## 📁 Features

- ✅ **Reddit Crawler** using PRAW to extract posts & comments  
- ✅ **Text Preprocessing** (tokenization, lemmatization, stemming, emoji handling)  
- ✅ **Emotion Detection** using Empath  
- ✅ **Frequent Pattern Mining** using PrefixSpan  
- ✅ **Silent Period Detection** (>3 days inactivity)  
- ✅ **Pattern Confidence & Sequential Confidence** metrics  
- ✅ **Top-K Pattern Tabulation** with `tabulate`  
- ✅ **Emotion Flow Visualization** using Sankey diagrams (Plotly)  
- ✅ **User Activity Filtering** (active ≥ 15 days)  
- ✅ **Statistical Summary** (posts, comments, active users)

---

## 🧰 Requirements

Install dependencies using pip:

```bash
pip install praw nltk emoji tabulate plotly empath prefixspan
```

Download NLTK data:

```python
import nltk
nltk.download('stopwords')
nltk.download('punkt')
nltk.download('wordnet')
```

---

## 📥 Reddit API Setup

Update the following with your credentials:

```python
reddit = praw.Reddit(
   client_id="YOUR_CLIENT_ID",
   client_secret="YOUR_CLIENT_SECRET",
   user_agent="YOUR_USER_AGENT"
)
```

---

## ⚙️ How It Works

### 1. **Data Collection**
- Crawls top 250 posts from `r/depression`
- Extracts posts and all comments
- Records author activity with timestamps

### 2. **Preprocessing**
- Removes HTML, punctuation
- Tokenizes and lemmatizes text
- Translates emojis to text
- Removes stopwords

### 3. **Emotion Extraction**
- Uses Empath to generate emotion scores
- Keeps emotions with score > 0.2

### 4. **User Filtering**
- Keeps only users active on ≥ 15 different days

### 5. **Pattern Mining**
- Builds emotion sequences for each user
- Inserts `'silence'` token if a gap > 3 days is found
- Mines frequent sequential patterns using PrefixSpan

### 6. **Pattern Evaluation**
- Computes:
  - **Support** (frequency)
  - **Confidence** (likelihood of last item given prefix)
  - **Sequential Confidence** (likelihood of sequence given first item)

### 7. **Visualization**
- Displays top-10 patterns per window size (w=1 to 5)
- Sankey diagrams show emotion flows:
  - With and without silent periods

---

## 📊 Sample Output

### Terminal Tabular Output

```
🔹 Discovered Rules With Silent Periods: Top-10 Sequential Patterns

Window Size 3:
+---------------------------+----------+------------+------------------------+
|         Pattern           | Support  | Confidence | Sequential Confidence |
+---------------------------+----------+------------+------------------------+
| sadness, silence, anger   |   8      |   0.62     |         0.75          |
| joy, sadness, silence     |   6      |   0.5      |         0.67          |
...
```

### Sankey Diagrams (Plotly)

- Visualize how users transition emotionally from one state to another
- Compare flows with and without silent gaps

---

## 📈 Statistics

```
The crawler retrieved 478 user records, 924 posts, and 4312 comments and replies.
We filtered the collected data to maintain only users active for at least 15 days, resulting in 61 frequent users.
The average number of posts and comments per user is 86.1.
```

---

## 🧠 Insights

- Silent periods often precede **negative emotional shifts** (e.g., sadness → silence → anger)
- Frequent transitions like **joy → sadness → anxiety** are common in emotionally distressed users
- Patterns help uncover **temporal emotional trajectories** in mental health communities

---
