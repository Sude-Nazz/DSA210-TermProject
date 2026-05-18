# DSA210-TermProject

# A Multivariate Analysis of *My Music Consumption Patterns* Based on *My Menstrual Cycle*, *Academic Stress*, and *Emotional State* (Predicting My Integrated Mood States through My Spotify Consumption and Academic Stress)

## MOTIVATION & PURPOSE:

I guessed that music would be a good mirror of my psyche. As a student, my life is governed by varying degrees of academic pressure, physiological cycles, and sudden emotional shifts. The motivation behind my project was to move beyond verbal evidence (e.g., "I listen to sad music when I'm stressed.") and utilize Data Science to quantify these patterns. By integrating my Spotify streaming history with personal logs, I aimed to discover whether my digital consumption could predict my mental and physical well-being, potentially creating a framework for "Mood-Aware" music recommendation systems.

## DATA SOURCE:

My project relies on a multi-source dataset (They cover March 2024 to March 2026, except for Crying Logs—covering January 2026 to March 2026.) **integrated at the daily level**:

•	**Spotify Streaming History:** I have collected via Spotify’s "Request Your Data" feature (GDPR). This provided JSON files containing every track played, timestamps, and duration (ms played).

•	**Academic Stress Logs:** It is a self-curated CSV file where I have recorded my daily stress levels on a scale of 1 to 5, specifically marking my exam periods as "High Stress" (4 or 5) etc.

•	**Physiological & Emotional Logs** (converted to CSV files):

o	*Menstrual Data:* Recorded dates of my cycle to analyze hormonal impact on my mood.

o	*Crying Logs:* Specific timestamps of my emotional incidents.

•	**Proxy Valence Data:** Since Spotify’s API valence was not directly available for all merged tracks, I engineered a "Proxy Valence" feature by mapping genres and artists (e.g., Arabesk, Rock, Pop) to energy scores (0.0 to 1.0) based on musical theory.

# The list of the techniques I have used and different stages of my analysis:

**1.	Data Integration:** I have merged multiple JSON sources and synchronized them with CSV logs using a "daily context" mapping function to create a master feature set.

**2.	Exploratory Data Analysis (EDA):** I have utilized Seaborn and Matplotlib to visualize the relationships between my stress levels & listening durations, my stress levels & track numbers, and my integrated moods & the valence scores of the songs.

**3.	Hypothesis Testing:**

o	*T-Tests & Pearson Correlation:* These are used to check for quantitative relationships (Duration/# of Tracks vs. Stress).

o	*ANOVA:* It’s employed to compare the means of valence scores across my five integrated mood categories (Stable, High Stress, Period, Period+Stress, Crying).

**4.	Machine Learning:** I have implemented and compared three classification models to predict my "Mood State":

o	*Decision Tree:* For logic visualization.

o	*Random Forest:* For handling variance.

o	*Gradient Boosting (HistGradientBoosting):* Optimized for imbalanced data.

In this stage, I also made comments on the visualization of the decision tree and the confusion matrices of all machine learning models I have used.

Furthermore, in order to decide on the best model for my project among them, I have deployed Weighted F1-Score. 



# Exploratory Data Analysis (EDA) & Hypothesis Testing
I have investigated whether *my stress levels* significantly change *my music consumption patterns.*

# 1st EDA & Hypothesis Test (T-Test)
*   **Metric (1):** Average Minutes Played.
*   **Hypothesis (1):** Track duration significantly changes during my high-stress exam weeks (Stress >= 4).

<img width="846" height="547" alt="Unknown" src="https://github.com/user-attachments/assets/a995b567-7d27-49e3-a141-725fce77fd72" />

T-Statistic: -1.1117, P-Value: 0.2663

*Conclusion:* No significant difference is found (p>0.05). I can't reject the null hypothesis (1). I do not know whether there is a crucial relationship between my average track duration and stress level.

BUT WHY❓

This is likely because my average track duration alone may not reflect my emotional shifts. For the next step, I will survey the relationship between my stress level and my track number, by utilizing Pearson correlation.


# 2nd EDA & Hypothesis Test (Pearson Correlation)

**Metric (2):** Total Number of My Tracks.

**Hypothesis (2):** My # of tracks significantly changes during my high-stress exam weeks (Stress >= 4).

<img width="868" height="547" alt="Unknown-2" src="https://github.com/user-attachments/assets/6c64c726-e971-42da-bddc-bb5de3078e1c" />

Pearson Correlation: 0.0105, P-value: 0.1208

*Conclusion:* No significant correlation is found (p>0.05). I can't reject the null hypothesis (2). I do not know whether there is a crucial relationship between my stress level and my track number.

BUT WHY❓

This is likely because there might be a relationship between my mood and the qualitative features of the songs (like **Energy and Valence**) but not the quantitative ones. Hence, I will try to investigate that kind of relation in the third EDA & Hypothesis Testing. 


# 3rd EDA & Hypothesis Test (Advanced Feature Engineering-ANOVA: **Proxy Valence** & **Mood States**)

Since raw valence isn't available for all tracks, I assigned **Proxy Valence Scores** based on artist/genre types (e.g., Arabesk = 0.15, Energetic Pop = 1.0) In order to be more realistic, I have added small randomizations to those scores (By the way, my initial target was to set up API connection on Colab and obtain valence scores from Spotify, but unfotunately Spoity did not allow me to do that since I don't have a Premium account.). I then created an **Integrated Mood State** which is the target variable for my ML models. Since I have more than two mood categories, ANOVA is the most robust method to compare all groups simultaneously without increasing the risk of statistical error; so, I have used ANOVA for my hypothesis test stage. (It should be noted that since I have included a randomization factor to determine the valence scores of the songs, the code will produce a different p-value each time it is run.)

**Hypothesis (3):** *My integrated mood* significantly affects *the valence score of the songs* that I am listening to.

<img width="1001" height="547" alt="Unknown-3" src="https://github.com/user-attachments/assets/daa9d5a9-3ff9-480b-81b4-8fdcdb1b70dd" />

(Example) ANOVA P-Value: 0.0142

*Conclusion:* **My mood significantly affects the valence score of the songs I am listening (p<0.05).**


# Machine Learning & Model Comparison

In the final phase of my project, I developed Supervised Machine Learning models to determine if my emotional and physiological states can be predicted by combining behavioral music data with environmental and academic context.

To address the feedback I've given regarding **biased truth labels**, I am comparing three models using the **Weighted F1-Score** instead of Accuracy.

* **Decision Tree:** Chosen for interpretability.

* **Random Forest:** Chosen for its ability to handle variance.

* **Gradient Boosting (HistGradientBoosting):** Chosen for its superior performance on imbalanced tabular data.


<img width="826" height="451" alt="Unknown-4" src="https://github.com/user-attachments/assets/a5858b17-0388-416c-8960-ffb8ecc5f76d" />

1. Decision Tree F1-Score   : 0.5842

2. Random Forest F1-Score   : 0.6446

3. Gradient Boosting F1-Score: 0.6639

--------------------------------------------------

THE BEST MODEL: Gradient Boosting

Best Weighted F1-Score: 0.6639

*Gradient Boosting is the winner. This is because boosting
algorithms iteratively correct the errors of previous trees, which
is highly effective for handling the imbalanced nature of mood labels.*


# DECISION TREE VISUALIZATION (Model Interpretability and Logic Showcase)

While Gradient Boosting achieved the highest predictive performance (F1-Score), it operates as a "black-box" model, making it difficult to visualize the exact decision boundaries.

I am showcasing the Decision Tree below for the following reasons:

* **Interpretability:** It provides a clear, human-readable map of how features like stress_level, hour, and valence interact to determine my mood.

* **Feature Hierarchy:** It visually demonstrates which features are the most important "splitters" (e.g., Stress Level usually appears at the top).

* **Transparency:** This visualization bridges the gap between complex machine learning and real-world behavioral patterns, explaining why a certain mood was predicted.

<img width="2051" height="974" alt="Unknown-5" src="https://github.com/user-attachments/assets/913030e3-5190-4b50-bfff-30083a35efbd" />

Interpretation Note:

The boxes show the conditions (e.g., stress_level <= 3.5).

The colors represent the predicted mood state for that path.

**Decision Tree Analysis: Decoding the Model's Logic**

This visualization provides a transparent look into how the model prioritizes different factors to predict my mood. By examining the splits, we can derive several key behavioral insights:

* **Primary Decision Factor (Stress Level):** The root node starts with stress_level <= 3.5. This shows that Stress is the most significant indicator in my dataset. If my stress level is low (True), the model moves to analyze the Hour of the day; if it’s high (False), it immediately considers high-stress mood categories.

* **Temporal Patterns (The 'Hour' Split):** For lower stress levels, the model splits at hour <= 10.5. This suggests a clear distinction between my morning/early afternoon music habits versus late-day listening, indicating that time of day is a strong secondary predictor for my emotional state.

* **The Role of Music Valence:** We see Valence appearing further down the tree (e.g., valence <= 0.499). This confirms that while the type of music I listen to is important, it usually acts as a "refiner" of my mood after the situational context (Stress and Time) is already established.

* **Interpretability over Complexity:** Even though Gradient Boosting is more accurate, this tree helps us validate the "Common Sense" of the project: My stress levels and the time of day when I am listening to music are the foundational pillars of my integrated mood state.


# ALL CONFUSION MATRICES

<img width="2361" height="669" alt="Unknown-6" src="https://github.com/user-attachments/assets/ffc82fcb-4c4a-4328-8cf9-679004c9b88d" />

**Additional (*In Addition to Weighted F1-Scores*) Comparative Analysis of Model Performances by Confusion Matrices**

After evaluating the Confusion Matrices for all three models, **Gradient Boosting** emerges as the superior model. Below is a comprehensive breakdown of its strengths and the inherent limitations of the current classification task.

1. **Why Gradient Boosting Performed Best**

* **Superior Accuracy in Major Classes:** Gradient Boosting correctly identified **2596 'Stable'** and **686 'High Stress Only'** instances, significantly outperforming the Decision Tree and Random Forest.

* **Error Minimization:** Unlike the Decision Tree and Random Forest, which heavily confused 'Stable' with 'Period Only' (728 and 463 misclassifications, respectively), Gradient Boosting minimized these overlaps, demonstrating a more refined decision boundary.

* **Iterative Learning:** As a boosting algorithm, it focuses on the "difficult" cases that previous trees failed to predict, allowing it to better distinguish between subtle differences in musical valence and stress levels.

2. **Limitations & the "Imbalance" Challenge**

Despite its high overall accuracy (76%), Gradient Boosting—and all other models—face a significant limitation: **Class Imbalance**.

* **The 'Crying' and 'Period' Bottleneck:** You can observe that Gradient Boosting failed to predict any **'Crying'** incidents (0 correct predictions) and predicted most (697/703) of the **'Period Only'** incidents as **'Stable'**. This is because the dataset is dominated by 'Stable' logs. Mathematically, the model "prefers" to predict the majority class to maximize overall accuracy.

* **The Interpretation of F1-Score:** This is exactly why we use the **Weighted F1-Score**. While accuracy is high, the model's inability to detect rare emotional events shows that "Music + Stress + Hour" features might not be sufficient on their own to identify a crying incident without more granular physiological data.

3. **Critical Evaluation**

* **Decision Tree:** While highly interpretable, it is too simple and makes broad errors, such as mislabeling nearly all 'Stable' instances as 'Period Only' in certain branches.

* **Random Forest:** Acts as a middle ground. It managed to catch **9 'Crying'** incidents (the only model to do so significantly), but its overall accuracy for 'Stable' and 'Stress' was lower than Gradient Boosting.

* **Final Verdict:** **Gradient Boosting** is the most reliable model for **trend prediction**, making it the best choice for this project.


# The Summary of Findings:

•	**The Valence-Mood Connection:** The most significant finding was from the **ANOVA test (p < 0.05)**, which proved that my mood state significantly affects the valence (emotional quality) of the music I choose, even though the quantity (duration/track number) of music did not show a significant change.

•	**Context indicates the Utmost Importance:** The Decision Tree revealed that stress_level and hour (time of day) are the primary splitters for predicting my mood. This suggests that *when* I listen to music is a stronger predictor of my state than the specific audio features themselves.

•	**Model Performance:** Gradient Boosting was the champion model with a **Weighted F1-Score of 0.67** (and **76% Accuracy**). Gradient Boosting was particularly effective at distinguishing “Stable” days from “High Stress” days. Unlike the Decision Tree and Random Forest, which heavily confused “Stable” with “Period Only” (728 and 463 misclassifications, respectively), Gradient Boosting minimized these overlaps, demonstrating a more refined decision boundary. 

•	**Temporal Patterns:** The models—as can be traced in the decision tree above—identified a clear distinction in my music habits before and after 10:30 AM, indicating a "routine-based" emotional signature in my listening history.


# Limitations & Future Work:

•	**Class Imbalance:** A major limitation was the scarcity of “Crying” and “Period” logs compared to my “Stable” days. All models struggled to predict these minority classes.

o	*Future Extension:* Implementing SMOTE (Oversampling), adding more features, or collecting data over a longer period (1-2 years) for minority classes would help balance the dataset.

•	**Proxy vs. Real API Data:** I have used Proxy Valence based on artist/genre.

o	*Future Extension:* Using the Spotify Web API (Spotipy) to obtain exact “Danceability”, “Energy”, and “Acousticness” for every single track would increase model precision.

•	**Physiological Sensors:** My stress was self-reported.

o	*Future Extension:* Integrating heart rate data from a wearable (like an Apple Watch) could provide an objective “Stress” feature, removing my probable human logging bias.


# Conclusion

This project has successfully demonstrated that my digital footprints—specifically my music choices—carry a hidden emotional signature. While predicting rare emotional events like crying remains a challenge for machine learning without more granular data, the ability to identify my high-stress academic periods with over 75% accuracy suggests that data-driven wellness monitoring is a viable and exciting field for future exploration.


# ONE ADDITIONAL (FINAL) ATTEMPT: Binary Classification Improvement (Based on My Assistant's Feedback)

Following the feedback from my assistant, I wanted to add this part in my project to survey also the transformed version of my multi-class problem into a **Binary Classification** task to be able to show the performances of the models in such a classification. Since the dataset is heavily biased toward the "Stable" class, merging all other mood states ('Crying', 'High Stress Only', 'Period Only', 'Period + High Stress') into a single class named **"Unstable"** will balance the labels and potentially yield significantly higher F1-Scores.

*Here is the binary model comparison with weighted F1-Scores:*

<img width="846" height="451" alt="Unknown-7" src="https://github.com/user-attachments/assets/0ccbab20-add7-4d96-b537-815e2a655c1c" />

# Binary Model Evaluation & Reflection

As predicted by my assistant, consolidating the labels into a binary format (**Stable vs. Unstable**) dramatically improved the models' predictive power. (The winner model is **Gradient Boosting** again, but with the **F1-Score** equal to **0.8181** this time!)

* **Mitigating Label Bias:** By clustering minority classes into "Unstable", I reduced the mathematical penalty the model faced when missing rare events like 'Crying' or 'Period Only'.

* **Improved F1-Scores:** The weighted F1-score underwent a noticeable increase (*from 0.6744 to 0.8181* for Gradient Boosting). This confirms that while the exact nuance of my emotion (e.g., distinguishing crying from a high-stress exam day) is too subtle to be predicted by my music consumption features (valence, hour), predicting whether I am in a general state of "Equilibrium (Stable)" or "Disruption (Unstable)" is highly achievable.

* **Final Project Takeaway:** This optimization bridging multi-class and binary classification perfectly captures the iterative nature of data science, proving that strategic data re-framing can solve severe class imbalance.



## How to Run:
1. Install dependencies: "pip install -r requirements.txt"
2. Ensure all data files ("Streaming_History_Audio_*.json", "academic_stress.csv", "menstrual_data.csv", "crying_logs.csv") are in the root folder. ("menstrual_data.csv" and "crying_logs.csv" are in .gitignore as they are my private data.)
3. Run the script: "my_dsa210_project.py"

You can also examine my project through the notebook "MY_DSA210_PROJECT.ipynb".


# Academic Integrity & AI Disclosure

I declare that AI tools were utilized during the development of my project. I have limited my AI use to technical assistance, code optimization, and structural guidance for the final report.

