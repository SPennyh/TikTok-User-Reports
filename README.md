# TikTok-User-Reports

# Dashboard
![](https://github.com/SPennyh/TikTok-User-Reports/blob/main/tiktok_report.gif)

# Introduction
This dataset was retrieved on Kaggle https://www.kaggle.com/datasets/raminhuseyn/dataset-from-tiktok. I had been in the process of finding an appropriate dataset in efforts to aid me in learning Power BI. This dataset was deemed a successful candidate, as my goal was to achieve a successful analysis uncovering potential business benefits while also experimenting with the data visualization capabilities. This dataset contains enough categorical and numerical data providing me the ability to explore the functionalities of Power BI such as Power Query and discovering insights within the data.
The goal of this data is to determine whether the TikTok user made a claim of a statement or provided their opinion. Being provided with both categorical data and numerical data allows users to explore multiple prediction methods, whether it be a regression model on the numerical data or a deep learning LLM model using the categorical data. In this report here, I will be exploring a multiple regression method while I’ll be leaving the opportunity to explore this data with an LLM as future considerations.

# The Data
The data consists of 4 categorical types of data including:
 - Claim_status: A binary choice of options. Opinion, where the content of the video is determined to contain the opinion of a TikTok creator. Claim, where the content of the video is determined to contain a claim made by a TikTok Creator.
 - Video_transcription_text: contains a transcription of the TikTok video
 - Verified_status: A binary choice of options. Not verified, where the TikTok creator is not a verified account. Verified, the TikTok creator is a verified account.
 - Author_ban_status: TikTok creators are active, under review or banned.
We also have a 7 numerical data types:
 - Video_id: The video ID of the respective video
 - Video_duration_sec: The duration of the video in seconds
 - Video_view_count: the number of views the video had at time of recording
 - Video_like_count: the number of likes the video had at time of recording
 - Video_share_count: the number of shares the video had at time of recording
 - Video_download_count: the number of downloads the video had at time of recording
 - Video_comment_count: the number of comments the video had at time of recording
I also retrieved a list of the 100 most used words https://en.wikipedia.org/wiki/Most_common_words_in_English

