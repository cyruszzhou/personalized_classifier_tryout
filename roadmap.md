# Proposal for Rule Initialization and Adjustment for Sensitive File Detection and Group Recommendation in Cloud Storage

## Motivation:
Adjusting the existing classifier to accommodate varying user needs (i.e., different decision trees) presents a problem of limited applicability, as it is impractical to create unique models for each user's individual preferences. Moreover, the quantification of adjustments to the branches is undefined, necessitating case-by-case calibration based on each feature extracted. The current application of XGBoost trees seems to operate on a trial-and-error basis, akin to "throwing spaghetti at the wall to see what sticks". This approach obscures interpretability, inhibiting understanding of which factors most significantly contribute to the sensitivity prediction. Additionally, XGBoost’s sensitivity to hyperparameters and the potential for overfitting due to its complexity exacerbates these challenges, leaving room for inaccuracies and an over-reliance on certain features. The lack of innate support for reinforcement learning in tree-based models further limits its capacity for continuous learning and real-time adaptation.

Further complicating the situation, we find challenges when considering more data-intensive machine learning methods such as multi-layer perceptrons (MLP). These methods, while powerful, are notoriously data-hungry. They require massive amounts of data to perform accurately, which may prove impractical or even impossible when relying on user studies for data collection. Conducting studies that yield data on a scale sufficient for MLP methods is resource-intensive, both in terms of time and cost.

Moreover, the need for large volumes of data may also infringe on user privacy and raise ethical concerns. Users might be uncomfortable with their sensitive files being used on such a large scale, even if the data is anonymized and secured. This introduces a barrier to gathering sufficient data for MLP or other data-intensive methods to be effectively utilized.

To address these challenges, we propose a structured, two-pronged approach: The Initialization Phase and the Reinforcement Learning Phase. This approach aims to enhance model performance by embedding rule-based learning, bringing a balance between interpretability and prediction power, and adapting to user feedback in real-time for continuous improvement.

## A. Initialization Phase

### 1. Individual Sensitive File Detection

- **User Study:** Gather data from a diverse user population about their perception of file sensitivity. Employ AI tools, such as word2vec, for feature extraction from these files.
  
- **Rule Extraction:**  Leverage formal reasoning / Inductive Logic Programming (ILP) methodologies to extract potential rules based on these features to determine a file's sensitivity. Select the most promising rules from a plethora of potential ones using a metric like information entropy.

- **Validation and Initial Weights Assignment:** Implement these rules on a distinct validation user set to ascertain their effectiveness and applicability. This process also enables initial weights (votes) assignment to the rules, taking into account the satisfying states of these rules. We suggest partitioning the original user study data into training and validation sets for rule extraction and rule weight/vote initialization, separately.

### 2. Group Recommendations for Sensitive Files

- **User Study:** Accumulate data from users about their sensitivity perception and file groupings. Either users can categorize sensitive files, or we can identify files that users generally handle or delete together. Use an identical feature extraction method as in the Individual Sensitive File Detection phase.

- **Rule Extraction:**  Employ formal reasoning / ILP techniques to derive potential rules for sensitive file groupings. Select the most effective ones from a pool of rules using a suitable metric, like information entropy.

- **Validation and Initial Weights Assignment:** Test these rules on a separate user validation set to verify their precision and applicability. Similar to the Individual Sensitive File Detection phase, this process also serves to assign initial weights to the rules. Again, we recommend splitting the original user study data into training and validation sets.

## B. Reinforcement Learning Phase

After the rule initialization phase, we embark on the reinforcement learning phase to continuously adjust the rule sets based on user feedback (e.g., file deletions).

### 1. Individual Sensitive File Detection

- **Defining Rewards and Penalties:** The system receives a reward when it correctly identifies a sensitive file, and a penalty when it fails to do so or when it falsely flags a non-sensitive file.

- **Rule Adjustment:** For each deletion event, evaluate the current set of rules against the deleted file. If a rule correctly predicted the file's sensitivity, increase its weight. If it predicted incorrectly, decrease its weight. The size of the adjustment can depend on the rule's variability across users, with more variable rules receiving larger adjustments.

- **Exploration and Exploitation:** Balance between trying out new or less certain rules (exploration) and using the most effective known rules (exploitation).

### 2. Group Recommendations for Sensitive Files

- **Defining Rewards and Penalties:** The system receives a reward when it correctly recommends a group of files similar to a deleted sensitive file, and a penalty when it recommends irrelevant files.

- **Rule Adjustment:** After each group recommendation, evaluate the effectiveness of the rules used in the recommendation. If a rule correctly predicted the group, increase its weight. If it predicted incorrectly, decrease its weight. As with individual file detection, the size of the adjustment can depend on the rule's variability across users.

- **Exploration and Exploitation:** Balance between trying out new or less certain rules (exploration) and using the most effective known rules (exploitation).
