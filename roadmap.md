# Proposal for Rule Initialization and Adjustment for Sensitive File Detection and Group Recommendation in Cloud Storage

## A. Initialization Phase

### 1. Individual Sensitive File Detection

- **User Study:** Collect data from a broad spectrum of users on which files they consider sensitive. Use AI tools such as word2vec for feature extraction from these files.
  
- **Rule Extraction:** Employ a SAT solver to extract potential rules based on the collected features that can determine the sensitivity of a file. Generate multiple potential rules and select the most promising ones based on a specific metric, such as information entropy.

- **Validation:** Apply these rules to a separate validation set of users to evaluate their effectiveness and generalizability. Consider splitting the original user study data into a training set (for rule extraction) and a validation set (for rule testing).

### 2. Group Recommendations for Sensitive Files

- **User Study:** Collect data from users not only on which files they find sensitive but also on the grouping of these files. Users can group sensitive files into categories or we can observe which files users typically handle or delete together. Use the same feature extraction method as in the Individual Sensitive File Detection section.

- **Rule Extraction:** Use the SAT solver or a similar machine learning technique to derive potential rules that determine the groupings of sensitive files. Generate a pool of rules and select the most effective ones using an appropriate metric, like information entropy.

- **Validation:** Validate these rules on a separate validation set of users to test their accuracy and generalizability. As with the Individual Sensitive File Detection section, consider splitting the original user study data into training and validation sets.

## B. Reinforcement Learning Phase

After the rule initialization phase, we embark on the reinforcement learning phase to continuously adjust the rule sets based on user feedback (e.g., file deletions).

### 1. Individual Sensitive File Detection

- **Defining Rewards and Penalties:** The system receives a reward when it correctly identifies a sensitive file, and a penalty when it fails to do so or when it falsely flags a non-sensitive file.

- **Rule Adjustment:** For each deletion event, evaluate the current set of rules against the deleted file. If a rule correctly predicted the file's sensitivity, increase its weight. If it predicted incorrectly, decrease its weight.

- **Exploration and Exploitation:** Balance between trying out new or less certain rules (exploration) and using the most effective known rules (exploitation).

### 2. Group Recommendations for Sensitive Files

- **Defining Rewards and Penalties:** The system receives a reward when it correctly recommends a group of files similar to a deleted sensitive file, and a penalty when it recommends irrelevant files.

- **Rule Adjustment:** After each group recommendation, evaluate the effectiveness of the rules used in the recommendation. If a rule correctly predicted the group, increase its weight. If it predicted incorrectly, decrease its weight.

- **Exploration and Exploitation:** Balance between trying out new or less certain rules (exploration) and using the most effective known rules (exploitation).

