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

*Conclusion:* No significant difference is found. I can't reject the null hypothesis (1). I do not know whether there is a crucial relationship between my average track duration and stress level.

BUT WHY❓

This is likely because my average track duration alone may not reflect my emotional shifts. For the next step, I will survey the relationship between my stress level and my track number, by utilizing Pearson correlation.


# 2nd EDA & Hypothesis Test (Pearson Correlation)

**Metric (2):** Total Number of My Tracks.

**Hypothesis (2):** My # of tracks significantly changes during my high-stress exam weeks (Stress >= 4).

<img width="868" height="547" alt="Unknown-2" src="https://github.com/user-attachments/assets/6c64c726-e971-42da-bddc-bb5de3078e1c" />

Pearson Correlation: 0.0105, P-value: 0.1208

*Conclusion:* No significant correlation is found. I can't reject the null hypothesis (2). I do not know whether there is a crucial relationship between my stress level and my track number.

BUT WHY❓

This is likely because there might be a relationship between my mood and the qualitative features of the songs (like **Energy and Valence**) but not the quantitative ones. Hence, I will try to investigate that kind of relation in the third EDA & Hypothesis Testing. 


# 3rd EDA & Hypothesis Test (Advanced Feature Engineering-ANOVA: **Proxy Valence** & **Mood States**)

Since raw valence isn't available for all tracks, I assigned **Proxy Valence Scores** based on artist/genre types (e.g., Arabesk = 0.15, Energetic Pop = 1.0). I then created an **Integrated Mood State** which is the target variable for my ML models. Since I have more than two mood categories, ANOVA is the most robust method to compare all groups simultaneously without increasing the risk of statistical error; so, I have used ANOVA for my hypothesis test stage.

**Hypothesis (3):** *My integrated mood* significantly affects *the valence score of the songs* that I am listening to.

<img width="1001" height="547" alt="Unknown-3" src="https://github.com/user-attachments/assets/daa9d5a9-3ff9-480b-81b4-8fdcdb1b70dd" />

ANOVA P-Value: 0.0142

*Conclusion:* **My mood significantly affects the valence score of the songs I am listening.**


# 4. Machine Learning & Model Comparison

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





## How to Run:
1. Install dependencies: "pip install -r requirements.txt"
2. Ensure all data files ("Streaming_History_Audio_*.json", "academic_stress.csv", "menstrual_data.csv", "crying_logs.csv") are in the root folder.
3. Run the script: "python main.py"

## Deliverables:
- "main.py": The core analysis script
- "my_featurized_data.csv": The integrated dataset with all features
- "stress_listening_eda.png": Visualization showing my stress level vs. my listening trend






## 3RD EDA & HYPOTHESIS TEST
This time, I have assigned valence scores to the songs I have listened, by utilizing the singers/genres. In addition, in order to be more realistic, I have added small randomizations to those scores (By the way, my initial target was to set up API connection on Colab and obtain valence scores from Spotify, but unfotunately Spoity did not allow me to do that since I don't have a Premium account.). Furthermore, I have deployed my integrated mood (by considering all aspects-academic stress, menstrual period, and crying) this time. I have defined **5 moods**: **Stable**, **Period Only**, **High Stress Only**, **Period + High Stress**, and **Crying**. Then, I have applied my 3rd EDA and hypothesis test to figure out the relationship between my integrated mood and the valence scores of the songs. Since I have more than two mood categories, ANOVA is the most robust method to compare all groups simultaneously without increasing the risk of statistical error; so, I have used ANOVA for my hypothesis test stage.
And, as it can be seen in the example result below, **p-Value** for this hypothesis test has been **0.0312**! Therefore, for this test, I have concluded that **"My integrated mood significantly affects the valence score of the songs that I am listening to."** (But, it should be noted that since I have included a randomization factor to determine the valence scores of the songs, the code may sometimes give "No significant relationship!" result.)

<img width="1001" height="547" alt="3rd_visualization" src="https://github.com/user-attachments/assets/c54b7e99-48a3-4f45-b5b5-850af430ccda" />


## MACHINE LEARNING: PREDICTIVE MODELING OF  INTEGRATED MOOD STATES
In the final phase of my project, I developed a Supervised Machine Learning model to determine if my emotional and physiological states can be predicted by combining behavioral music data with environmental and academic context.


**A. Methodology & Algorithm Selection**

I implemented a *Decision Tree Classifier* for this analysis.

*Why have I used Decision Tree?* The Decision Tree allows me to visualize the specific logical paths (e.g., if stress is high AND music valence is low, THEN predict 'Crying') that lead to an emotional state, providing a "map" of my mood triggers.

*Hyperparameter Tuning:* I restricted the model to a max_depth=3. This deliberate constraint prevents overfitting, ensuring the model captures general behavioral patterns rather than memorizing noise in the dataset.


**B. Feature Engineering & Justification**

To build a robust model, I selected three specific features as inputs:

*1. My Academic Stress Level (Contextual Anchor):* This is the most critical feature. It provides the "environmental context." The same sad song might represent me "relaxation" during low stress but "emotional distress" during my high-stress periods.

*2. Musical Valence:* This numerical value represents the emotional positivity of the tracks.

*3. Hour of Day:* Captures my circadian rhythm and temporal habits.


**C. Model Validation: 10-Fold Cross-Validation**

To ensure the results were not due to a lucky random split of data, I utilized 10-Fold Cross-Validation.

*-Process:* The dataset was partitioned into 10 subsets. The model was trained 10 times, each time using a different subset as the test set and the remaining 9 as the training set. (This method is the industry standard for ensuring that the model generalizes well to new, unseen data.)


**D. Performance Results & Discussion**

The model yielded highly consistent and successful results:

*Average CV Accuracy (10-Fold):* 76.25%

*Standard Deviation:* 0.0093

<img width="1737" height="814" alt="decision_tree" src="https://github.com/user-attachments/assets/83f02a0b-0d7b-41e1-8f39-6b22873da963" />
<img width="922" height="753" alt="confusion_matrix" src="https://github.com/user-attachments/assets/82a2e527-67f0-4b40-a1c9-99f9c36e2968" />


**Interpretation of Results:**

**High Stability:** The extremely low standard deviation (0.0093) proves that the model's performance is incredibly stable across different data segments. It confirms that the relationship between my stress, music, and mood is consistent and statistically significant.

**Predictive Power:** A 76.25% accuracy rate indicates that my Integrated Mood State is not random. My Spotify footprint and my academic environment (Stress) create a "digital signature" that can be used to monitor and predict my emotional well-being.

**Confusion Matrix Insights:** The model shows high precision in identifying Stable and High Stress states. However, it faces challenges with minority classes like Period Only and Crying. These are often misclassified into the majority categories. This result highlights that emotional extremes often share musical "signatures" with more common states, and further physiological features would be required to distinguish these nuanced moods with 100% precision.
