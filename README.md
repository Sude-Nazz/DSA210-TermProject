# DSA210-TermProject

# A Multivariate Analysis of My Music Consumption Patterns Based on My Menstrual Cycle, Academic Stress, and Emotional State

## PURPOSE:
I am investigating how my music consumption (via Spotify history) correlates with my academic stress, emotional state (crying logs), and biological cycles (menstrual data).

## Things I have done in Milestone 1:
- **Data Merging:** I have combined my Spotify JSON files (extended streaming history data) with custom CSV logs.
- **Handling Missing Data:** Non-identified days in the academic calendar are defaulted to a stress level of 2.
- **Integration:** I have utilized range-based matching for stress/period dates and specific date matching for crying incidents.
- **EDA:** I have visualized the relationship between my stress level and my average track duration.
- **Statistics:** I have performed a T-Test to see if my stress level significantly changes my listening behavior.

## How to Run:
1. Install dependencies: "pip install -r requirements.txt"
2. Ensure all data files ("Streaming_History_Audio_*.json", "academic_stress.csv", "menstrual_data.csv", "crying_logs.csv") are in the root folder.
3. Run the script: "python main.py"

## Deliverables:
- "main.py": The core analysis script
- "my_featurized_data.csv": The integrated dataset with all features
- "stress_listening_eda.png": Visualization showing my stress level vs. my listening trend

### Initial Findings (Milestone 1):
My initial exploratory analysis is focused on the relationship between **Academic Stress Levels** and **Average Track Duration**. 
- **p-Value:** 0.2663
- **Interpretation:** The current results do not show a statistically significant difference in listening duration based on my stress level.

![my stress level vs. my listening trend](stress_listening_eda.png)

**Why?**
This is likely because my average track duration alone may not reflect my emotional shifts. For the next step, I am going to integrate **Spotify Audio Features (Energy, Valence)** to see if the *type* of music changes, even if the *duration* remains the same.


## 2ND EDA & HYPOTHESIS TEST
I have applied second EDA and hypothesis test to figure out the relationship between my stress level and the number of tracks, by utilizing Pearson correlation.
Therefore, we hav had a visualization showing my stress level vs. my number of tracks.
We have obtained the **p-Value** as **0.1208** (>0.05). So, the correlation between my stress level and the number of tracks is not significant.
**Why?**
This is likely because there might be a relationship between my mood and the qualitative features of the songs (like Energy and Valence) but not the quantitative ones.
Hence, I will try to investigate that kind of relation.


<img width="868" height="547" alt="2nd_ visualization" src="https://github.com/user-attachments/assets/60b35342-c7cd-4a66-88f6-1cf51c078810" />


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
