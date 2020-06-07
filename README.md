
# Amazon Voting Behaviors Reveal Social Conformity
MACS 30123 (Spring 2020) Large-Scale Computing for the Social Sciences Final Project

## Social Science Background
            The emergence of customer review sites encourages the exchange of ideas and the expression of opinions. The reviews of others become an increasingly important factor in our decision-making processes. Hence, understanding how these reviews are received and evaluated becomes essential to businesses and online platforms. Since reviews are enormous and vary largely in quality, it is important to understand the helpfulness of reviews so that to improve our efficiency and experience. In this project, we intend to develop a framework to understand how opinions are evaluated in online communities.

            Instead of focusing on the review itself, we seek to understand how people evaluate other’s reviews on products. Fortunately, Amazon not only includes the reviews given by users, but also the helpfulness of their reviews (e.g. with the annotation “5 of 10 people found this helpful”). Each review comes with both a rating of the product and a helpfulness vote. While the helpfulness evaluation can provide us with many interesting insights, it has not been widely studied. Past research has attempted to treat it as a classification problem to assess how helpful each review is and managed to rank correlations of up to 0.66 (Kim et al., 2006). Other research evaluated the helpfulness vote as a function of its relation to other competing products (Danescu-Niculescu-Mizil et al., 2009). That is, the helpfulness vote also depends on the evaluation of reviews of the same products. These researches suggest that there is a subtle relation between helpfulness evaluations and our social feedback. We want to understand how social feedback mechanisms display in these reviews and how they affect our evaluation outcomes.

            Our evaluation of other’s expressions of opinions on the same product reflects the social effects in a group setting. We consider a social psychology theory – conformity – as the starting point of our analysis. Conformity is a type of social influence that involves a change in belief or behavior due to group pressures. It usually indicates an agreement with the majority opinion. The most famous conformity experiment was conducted by Solomon Asch (1951), where participants were asked to complete a simple line judgment task. While there was an obvious answer to the task, participants conform to the majority opinion in the group even though they knew it was not correct. Do helpfulness votes in Amazon reviews also reveal such social conformity? We hypothesize that a review will be conceived as more helpful when its rating is closer to the average rating of the product. If social conformity exists, does it vary across product categories and marketplaces? According to Bond and Smith (1996), collectivist countries tend to show higher levels of conformity than individualist countries. Hence, we hypothesize that reviews in collectivist marketplaces will experience higher conformity than those in individualist marketplaces.
    
## Large-Scale Computing Strategies
            The AWS Open Data Registry contains a collection of data that is located in S3 storage and makes it easy to work with from within the AWS cloud ecosystem. Amazon Customer Reviews Dataset includes over 130+ million customer reviews that express opinions and describe experiences regarding Amazon’s iconic products. The collection of reviews spans from 1995 to 2015 and varies across geographical regions. This allows us to evaluate the perception of a product in different marketplaces at scale. 

            To start, we created a cluster of 8 m5.xlarge EC2 instances for the AWS EMR notebook. We worked with PySpark Kernel to load the Amazon reviews dataset. To ensure comparison across geographical regions, only product categories that have enough data in all marketplaces are chosen in this study. We have four categories eventually: books, music, electronics, and video. (Note that the same approach is applied to all categories, and so only the code for books is heavily annotated.) The helpfulness ratio of a review is defined as the proportion of people who found it helpful. The rating difference between a review and the product average is calculated in absolute difference using PySpark SQL functions. Then, the ratings are aggregated and divided into buckets to represent bins of differences. Our results showed that reviews in all product categories reveal social conformity. When the review rating is closer to the average product rating, the review is evaluated as more helpful.

            Next, we examined whether such conformity varies across marketplaces. The marketplace in Amazon reviews dataset is a 2 letter country code of the place where the review was written, and so it represents the opinions and evaluations from people in that country. The dataset includes reviews from the US, UK, DE (Germany), FR (France), and JP (Japan). We expect that reviews written in Japan (collectivist) will show higher levels of conformity than those in other countries (individualist). Our results confirmed the hypothesis for products in books, music, and electronics. We observed a smaller rating difference when the review is conceived as less helpful. That is, even for reviews that are not helpful at all, their ratings are still close to the product average rating. There is one exception, though. For products in videos, reviews in France showed the highest level of conformity. However, this can be explained by its disproportionally fewer data in reviews. There are only 530 reviews in FR while other marketplaces have at least 2000 reviews. This might lead to the biased results.

            To understand the characteristics of reviews that are evaluated as highly helpful and highly unhelpful, we performed Natural Language Processing (NLP) using PySpark functions and Spark NLP in Google Colab. Spark NLP is an NLP library build natively on Apache Spark and includes pre-trained models and pipelines to perform deep learning NLP research. We first applied the pre-trained sentiment analysis model to the review body. For all product categories, we found no difference in sentiment values between highly helpful and highly unhelpful reviews. This makes sense because the helpfulness of reviews does not necessarily depend on whether it expresses positive or negative emotions.

            We then created a pipeline of estimators and transforms using Spark ML library to look at the top terms in the two classes of reviews. The pipeline is a workflow of transformation to our target dataset and includes tokenizer, stop words remover, count vectorizer, and IDF. We extracted the high scoring words in both helpful and unhelpful reviews. The results suggest that these keywords are closely related to their product categories. For instance, the top words in Music are Elvis, album, song, record, bands, shows, etc. while top words for Books are edition, history, view, audiobook, horrors, and read. However, the difference between the top words in the two classes is not distinctive.

## Implication
            Our finding is consistent with the conformity theory: reviews are evaluated as more helpful when their ratings are closer to the average rating of the product. The theory holds across product categories. We also observe that reviews in the collectivist marketplace show higher levels of conformity than those in individualistic marketplaces. We primarily used PySpark in the AWS EMR notebook and Google Colab to perform large-scale computation and analysis. Helpfulness evaluations on Amazon provide a novel way to assess how opinions are evaluated in online communities. In future research, we can look at more data to validate our theory and to understand the mechanisms of social feedback. And hopefully, our approach can benefit researchers on other online communities as well.

## References
* Bond, R., & Smith, P. B. (1996). Culture and conformity: A meta-analysis of studies using Asch's (1952b, 1956) line judgment task. Psychological bulletin, 119(1), 111.
* Danescu-Niculescu-Mizil, C., Kossinets, G., Kleinberg, J., & Lee, L. (2009, April). How opinions are received by online communities: a case study on amazon. com helpfulness votes. In Proceedings of the 18th international conference on World wide web (pp. 141-150).
* Kim, S. M., Pantel, P., Chklovski, T., & Pennacchiotti, M. (2006, July). Automatically assessing review helpfulness. In Proceedings of the 2006 Conference on empirical methods in natural language processing (pp. 423-430).

                                      
