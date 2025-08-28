# 🎮 Steam Game Recommender System  
*Personalized game recommendations using collaborative and content-based filtering*  
---

## 1. Project Background  

### 1.1 Background of the Problem Domain  
The video game industry has grown significantly over the past decade, generating **$187.7 billion in 2024**, with projections surpassing $200 billion by 2025 (Knezovic, 2025). Steam, developed by Valve, is one of the largest gaming platforms, reporting **132M monthly active users** and breaking records with **40M concurrent users** in 2025.  

As the game catalog grows, users struggle to discover games that fit their interests. **Recommender systems** have therefore become essential to help personalize game discovery beyond popularity-based methods.  

### 1.2 Problem Description  
Although Steam offers thousands of games, users often face **choice overload**, leading to irrelevant or repetitive recommendations. This project develops two approaches:  

- **Collaborative Filtering** (memory-based & model-based)  
- **Content-Based Filtering**  

Both are evaluated to identify their strengths in improving game discovery.  

---

## 2. Data Understanding & Integration  

**Dataset:** [Steam Video Games Dataset – Kaggle](https://www.kaggle.com/)  
- >200,000 records of user interactions  
- Attributes:  
  - `user_id`, `game_title`, `behavior_name` (purchase/play), `value` (hours played or binary purchase)  

**Data Preparation:**  
- Removed unnamed/duplicate columns  
- Dropped 707 duplicate rows  
- Normalized playtime using Min-Max scaling  
- Filtered dataset to **only ‘play’ interactions** (more meaningful for engagement)  
- Label encoded `user_id` and `game_title` for processing  

📊 **Exploratory Insights:**  
- Top 10 most purchased games: *Dota 2*, *Team Fortress 2*  
- Top 10 most played games: *Dota 2*, *Counter-Strike*, *Skyrim*  
- Skewed distribution of playtime → normalization was required  

---

## 3. Recommender Systems  

### 3.1 Memory-Based Collaborative Filtering  

#### 3.1.1 User-to-User Collaborative Filtering Process  
- Built a **user-item matrix** (rows = users, cols = games, values = normalized playtime)  
- Applied **cosine similarity** between users  
- Identified top-5 similar users and recommended unplayed games weighted by similarity  

✅ Produced diverse & personalized results when overlapping histories existed.  

#### 3.1.2 Item-to-Item Collaborative Filtering Process  
- Transposed user-item matrix → cosine similarity between games  
- Recommended games often co-played with titles the user already engaged in  
- Example: Similar to *Dota 2* → other MOBAs & popular multiplayer titles  

✅ Generated **contextually relevant suggestions** closely aligned with user history.  

---

### 3.2 Model-Based Collaborative Filtering Using SVD  
- Applied **Singular Value Decomposition (SVD)** via Surprise library  
- Tuned with **GridSearchCV** for latent factors, learning rate, regularization  
- Predicted unseen ratings for each user and recommended top-5 new games  

✅ Captured **latent factors** in user-game interactions.  
⚠️ Limitation: Repeated recommendations across users due to data sparsity.  

---

### 3.3 Content-Based Filtering  
- Used **TF-IDF vectorization** on game titles  
- Cosine similarity between titles to suggest thematically related games  
- Supported:  
  1. Game-to-game recommendations  
  2. Profile-based recommendations (aggregated by user’s played games)  

✅ Useful for **cold-start problems** where user history is limited.  
⚠️ Limited by absence of rich metadata (genres, tags, developer info).  

---

### 3.4 Summary of Results  
| Method | Strengths | Weaknesses |  
|--------|-----------|------------|  
| **User-to-User** | Personalized, interpretable | Needs overlap in histories |  
| **Item-to-Item** | Contextual, relevant | Limited if user history is sparse |  
| **SVD** | Captures hidden patterns | Tended to recommend same popular games |  
| **Content-Based** | Good for cold-start, thematic | Dependent on text similarity only |  

➡️ **Best Choice**: **Memory-based collaborative filtering** (user & item).  
➡️ Content-based kept as a **complementary approach**.  

---

## 4. Discussion on Build Quality  
- Preprocessing ensured **clean, normalized data**.  
- Models were reproducible with fixed random states.  
- Grid search optimized hyperparameters for SVD.  
- Comparative evaluation (10 users, 4 methods) highlighted trade-offs.  

📌 The combination of collaborative + content-based filtering formed a **flexible hybrid foundation**.  

---

## 5. Conclusion  
This project successfully built a **Steam video game recommender system** using real interaction data.  

- Collaborative filtering (user-based & item-based) delivered **practical and personalized recommendations**.  
- Model-based SVD captured **latent patterns**, though limited by sparsity.  
- Content-based filtering added value in **cold-start cases**.  

👉 Overall, the recommender system effectively addressed game discovery challenges and showed potential for **hybrid extensions** with richer metadata.  
---
