# TikTok-User-Reports

# Dashboard
![](https://github.com/SPennyh/TikTok-User-Reports/blob/main/tiktok_report.gif)

# Dependencies
- pandas
- numpy
- statsmodels
- seaborn
- matplotlib
- sklearn

# Introduction
This dataset was retrieved from Kaggle: https://www.kaggle.com/datasets/raminhuseyn/dataset-from-tiktok. I had been in the process of finding an appropriate dataset to aid in my efforts to learn Power BI. This dataset was deemed a suitable candidate, as my goal was to conduct a successful analysis that uncovered potential business benefits while also experimenting with data visualization capabilities. The dataset contains enough categorical and numerical data, providing me with the ability to explore Power BI functionalities such as Power Query and gain insights from the data.
<br />
The goal of this analysis is to determine whether the TikTok user made a claim or expressed an opinion. Having both categorical and numerical data allows users to explore multiple prediction methods, whether it be a regression model on the numerical data or a deep learning LLM model using the categorical data. In this report, I will explore a multiple regression method, leaving the opportunity to further explore this data with an LLM for future consideration.


# The Data
The data consists of 4 categorical types of data including:
 - Claim_status: A binary choice of options. **Opinion**, where the content of the video is determined to contain the opinion of a TikTok creator. **Claim**, where the content of the video is determined to contain a claim made by a TikTok Creator.
 - Video_transcription_text: contains a transcription of the TikTok video
 - Verified_status: A binary choice of options. **Not verified**, where the TikTok creator is not a verified account. **Verified**, the TikTok creator is a verified account.
 - Author_ban_status: TikTok creators are active, **under review** or **banned**.
We also have a 7 numerical data types:
 - Video_id: The video ID of the respective video
 - Video_duration_sec: The duration of the video in seconds
 - Video_view_count: the number of views the video had at time of recording
 - Video_like_count: the number of likes the video had at time of recording
 - Video_share_count: the number of shares the video had at time of recording
 - Video_download_count: the number of downloads the video had at time of recording
 - Video_comment_count: the number of comments the video had at time of recording
I also retrieved a list of the 100 most used words https://en.wikipedia.org/wiki/Most_common_words_in_English

# Data Cleaning
Our goal is to determine whether any of the given variables can be used to predict the claim status of TikTok videos.
<br />
The first step was to drop rows with missing values (NA). Next, I examined the metrics comparing videos based on claim status and opinion status. When observing scatterplots comparing the claim metrics to opinion status metrics, we noticed that the claim metrics had significantly larger values than the opinion metrics. To get a clearer view of the data, I applied a log base 10 transformation to the video_view_count, video_like_count, video_share_count, video_download_count, and video_comment_count.
<br />
The next aspect I wanted to observe was the frequency of words in "claim" and "opinion" videos. The first step was to split the video transcription text using a space delimiter and group the data by word occurrence. From there, I wanted to remove the 100 most commonly used words from the list. To achieve this, I performed a left anti-merge between the list of word occurrences and a list from Wikipedia of the 100 most commonly used words.

# Exploratory Data Analysis (EDA)
The first graph in the top right corner of the dashboard examines the relationship between the log of video view counts as the explanatory variable, and the log of comment, download, like, and share counts as response variables in their respective graphs, with claim_status set as an indicator variable. From an initial impression of all four graphs, we notice a highly similar, strongly correlated appearance. This suggests that TikTok video metrics may not be good predictors of claim status. However, we will revisit this with further statistical inference.
<br />
Now, let’s examine the bar chart at the bottom left of the dashboard. This chart displays the most common occurrences of words that are exclusive to either the claim or opinion statuses. Clicking the buttons located to the left of the chart allows switching between the two claim statuses. The words are sorted from the most to least frequent, showing the top 21 words. In the claims text, we observe action-oriented words like “read,” “learned,” “discovered,” and “revealed,” which may suggest communication and learning. In contrast, the opinion text includes a significant number of possessive nouns, such as “colleagues’” and “friends’,” typically referencing a group. Additionally, we see words that refer to varied perceptions, such as “opinion,” “sentiment,” and “hypothesis.” Overall, there is a notable difference in the tone of words related to claim status, so further analysis and the application of machine learning on the structure of the video transcription may reveal it as a strong predictor.
<br />
Lastly, I wanted to explore whether author_ban_status influences claim status. The last two graphs in the bottom right corner display this information. The first graph shows the average like count by claim status and author ban status. When clicking the previously mentioned claim status buttons, we see that the average like counts for both ban statuses fall within similar ranges. Claim status videos average over 160k likes, while opinion status videos average around 1,100 likes. The second graph looks at the distribution of likes among the different ban statuses. We see that most likes are concentrated within the active ban status, while banned and under-review statuses have similar totals.

# Model Design and Inference

Now looking at the Python notebook, this model will analyze the numerical data, excluding video_id, as the explanatory variables, while claim_status will be our response variable. First, we examine the pairwise correlations among all the variables. We observe a decent distribution of correlation values among the metric variables, while all the log base 10 transformed metrics are highly correlated.
<br />
Next, we aim to determine the best regression model using a forward stepwise selection method. This function will continuously add variables to the model until the largest possible R-squared value is reached. Once the model is obtained, we observe a high R-squared value, which raises concerns about potential overfitting.
<br />
Model formula:
```
claim_status ~ log_view_count + video_view_count + log_comment_count + video_comment_count + log_download_count + log_like_count + video_like_count + video_download_count + video_duration_sec + 1
```
<br />
Before running the model, I checked the variance inflation factors (VIF) due to concerns of multicollinearity. As expected, the log base 10 variables exhibit large VIFs, so multicollinearity remains a potential issue.
<br />
To further validate the model, I obtained a summary statistic. The F-statistic shows a large value of 5155, indicating significant variation among the means of our variables. Finally, we look at the p-value, which is less than 0.00, suggesting strong evidence that these variables influence claim status.
<br />
With the model complete, we make predictions on our test samples and create a table comparing the actual values with the predicted values. Since the predicted values are in floating-point form, they need to be adjusted to match the encoded claim status values. This is done by setting all values less than 0.5 to 0, and all values greater than 0.5 to 1. After adjustment, I create a column to determine whether the predictions were correct.

# Conclusion and Results
The model results show 99.53% accuracy, with a mean squared error (MSE) of 0.03 and a mean absolute error (MAE) of 0.1. These strong results suggest that this is a reliable model for determining whether a video should be flagged as a claim or opinion. However, we need to consider the original dashboard visualizations, where claim-labeled videos tended to receive higher video metrics than opinion-labeled videos. As a result, instances where opinion-labeled videos go viral might be overlooked when applying this model.
<br />
Future experimentation, incorporating the video transcription text into the model, could lead to more realistic outcomes. For now, if the video metrics align with the patterns of low-performing opinion videos and high-performing claim videos, this model is a strong candidate for replacing manual labeling of TikTok videos.

