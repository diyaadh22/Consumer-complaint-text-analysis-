# Consumer Complaints
## This project aims to analyze consumer complaints from a specific dataset to uncover underlying sentiments using natural language processing techniques. By employing sentiment analysis, we aim to categorize complaints into various sentiment classes, enabling us to understand consumer sentiment trends towards products or services.
---
## Data Dictionary📖
The data for this project is sourced from "Consumer Complaints.xlsx," containing consumer complaints collected over a specific period.
Key columns include:

-Consumer complaint narrative: Text of the complaint submitted by the consumer.

-Product: The product category the complaint is about.

-Sub-Product: Sub-Product about which complaint was registered (Text)

-Company: The company the complaint is directed at.

-Issue: Issue about which complaint was registered

-Sub-Issue: Issue about which complaint was registered

-Tags: Description of person who complained (Text)

---
## Data Cleaning 🧹

 The data cleaning process includes adjusting blank cells, ensuring proper date formats, and preparing the text data for analysis.
 
### Adjusting Blank Cells
To maintain data consistency and facilitate analysis, blank cells in several columns are replaced with "N/A".
```
complaints$ZIP.code <- as.numeric(complaints$ZIP.code)
complaints$Consumer.consent.provided. <- ifelse(complaints$Consumer.consent.provided. == "", "N/A",complaints$Consumer.consent.provided.)
complaints$Sub.product <- ifelse(complaints$Sub.product == "", "N/A",complaints$Sub.product)
complaints$Sub.issue<- ifelse(complaints$Sub.issue == "", "N/A",complaints$Sub.issue)
complaints$Consumer.complaint.narrative <- ifelse(complaints$Consumer.complaint.narrative == "", "N/A",complaints$Consumer.complaint.narrative)
complaints$Company.public.response <- ifelse(complaints$Company.public.response == "", "N/A",complaints$Company.public.response)
complaints$Tags<- ifelse(complaints$Tags == "", "N/A",complaints$Tags)
complaints$Consumer.disputed.<- ifelse(complaints$Consumer.disputed. == "", "N/A",complaints$Consumer.disputed.)
```
### Ensuring Proper Date Formats
Dates are formatted to ensure consistency and ease of analysis, converting them from character strings to Date objects using lubridate.
```
consumer_complaints$`Date received` <- as.Date(consumer_complaints$`Date received`, format = "%Y-%m-%d")
consumer_complaints$`Date sent to company` <- as.Date(consumer_complaints$`Date sent to company`, format = "%Y-%m-%d")
```
### Preparing Text Data for Analysis
The 'Consumer complaint narrative' column, essential for sentiment analysis, is preprocessed to remove NA values and ensure it contains character strings. This preprocessing is vital for the tokenization process in sentiment analysis.
```
consumer_complaints <- consumer_complaints %>%
  filter(!is.na(`Consumer complaint narrative`), `Consumer complaint narrative` != "") %>%
  mutate(`Consumer complaint narrative` = as.character(`Consumer complaint narrative`))
```
---
## Data Analysis 📏

### Count of Complaints by Product
Purpose: This bar chart summarizes the total number of complaints for each product category, arranged in descending order of complaint count. It highlights which products are most problematic are debt collection and mortgage

Insight: By identifying products with the highest number of complaints, companies can pinpoint areas requiring immediate attention and improvement. This visualization aids in prioritizing customer service efforts.

![Alt text](https://github.com/diyaadh22/Consumer-complaint-text-analysis-/blob/main/Rplot05.png)

### Top 10 States by Complaint Count
Purpose: This bar chart breaks down the complaint counts by state, showing the top 10 states where the highest numbers of complaints originate. It provides geographical insight into where consumer dissatisfaction is most concentrated.

Insight: Understanding the regional distribution of complaints can help companies tailor their marketing, support, and operational strategies to address specific regional issues

![Alt text](https://github.com/diyaadh22/Consumer-complaint-text-analysis-/blob/main/Rplot03.png)


## Sentiment Analysis
The Bing lexicon categorizes words as positive or negative, providing a straightforward sentiment analysis. The process involves tokenizing the Consumer complaint narrative, filtering out stopwords, and joining with the Bing lexicon to count the sentiments.

Visualization: A bar chart visualizes the frequency of positive versus negative sentiments among the complaints. This helps to gauge the overall sentiment trend within the consumer feedback.
```
bing_sentiments <- complaint_tidy %>%
  inner_join(get_sentiments("bing"), by = "word") %>%
  count(sentiment) %>%
  ggplot(aes(x = sentiment, y = n, fill = sentiment)) +
  geom_col() +
  theme_minimal() +
  labs(x = NULL, y = "Frequency", title = "Bing Sentiment Analysis")
```

![Alt text](https://github.com/diyaadh22/Consumer-complaint-text-analysis-/blob/main/Rplot02.png)

### NRC Sentiment Analysis

The NRC lexicon provides a nuanced sentiment analysis, identifying emotions such as trust, fear, and joy. Similar to the Bing analysis, the process involves tokenizing narratives, filtering, and counting the identified sentiments.

Visualization: A bar chart displays the distribution of various emotions, offering deeper insight into the emotional undertones of consumer complaints.

```
nrc_sentiments <- complaint_tidy %>%
  inner_join(get_sentiments("nrc"), by = "word") %>%
  count(sentiment) %>%
  ggplot(aes(x = sentiment, y = n, fill = sentiment)) +
  geom_col() +
  theme_minimal() +
  labs(x = NULL, y = "Frequency", title = "NRC Sentiment Analysis")
```

![Alt text](https://github.com/diyaadh22/Consumer-complaint-text-analysis-/blob/main/Rplot1%20(1).jpeg)

## Word Cloud
A word cloud is generated from the Consumer complaint narrative to visualize the most frequent terms in the complaints, providing an at-a-glance view of common themes.

```
wordcloud(filtered_words, max.words = 50, scale = c(3, 0.5), random.order = FALSE, colors = brewer.pal(8, "Dark2"))
```

![Alt text](https://github.com/diyaadh22/Consumer-complaint-text-analysis-/blob/main/Rplot.png)




