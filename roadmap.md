# Proposal for Rule Initialization and Adjustment for Sensitive File Detection and Group Recommendation in Cloud Storage

## Motivation:
Modifying the existing classifier for different users (i.e., different decision trees) poses a constrained scope problem. Furthermore, the adjustments to the branches are nebulous, as they need to be defined for each feature extracted. Current XGBoost trees seem to lack precision, akin to a "throwing spaghetti at the wall to see what sticks" strategy, raising questions about their interpretability. [Complete with further challenges associated with XGBoost.]

We propose a two-phase solution to overcome these challenges: The Initialization Phase and the Reinforcement Learning Phase.


## A. Initialization Phase

### 1. Individual Sensitive File Detection

- **User Study:** Gather data from a diverse user population about their perception of file sensitivity. Employ AI tools, such as word2vec, for feature extraction from these files.
  
- **Rule Extraction:**  Leverage formal reasoning / Inductive Logic Programming (ILP) methodologies to extract potential rules based on these features to determine a file's sensitivity. Select the most promising rules from a plethora of potential ones using a metric like information entropy.

- **Validation and Initial Weights Assignment:** Implement these rules on a distinct validation user set to ascertain their effectiveness and applicability. This process also enables initial weights (votes) assignment to the rules, taking into account the satisfying states of these rules. We suggest partitioning the original user study data into training and validation sets for rule extraction and testing respectively.

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
