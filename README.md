# Health-Score-Project
This project involves the calculation of a health score for various components critical to Didar's CRM system, such as a company's overall performance, its users, the pipeline, deals, and cases utilized by users, among others. By doing so, members of the "Support Team" and "Customer Success" can leverage these reports to effectively manage users, identify potential causes for churn, and proactively prevent customer attrition in the future.

The project primarily focused on extensive feature engineering to efficiently handle various features. Since the core project's workflow remained consistent, I will specifically detail the 'Deal Health Score' project, while mentioning that the other two projects were developed as variants due to the primary project's success in delivering accurate results.

It is also worth noticing that the report is updated monthly, and the `exportdate` variable in the beginning of the notebook is related to the date of report. Throught explanation of the features is given below.

## Features
1. **Warning Sign**: The optimal approach for utilizing the software to facilitate product sales involves a two-step process: firstly, registering a deal and subsequently logging upcoming activities with due dates to enhance tracking. In the event that a deal remains stagnant without associated activities, the system displays a warning sign to alert the user about their suboptimal usage of the software. Consequently, minimizing the occurrence of this warning sign indicates a higher level of proficiency in software usage by the salesperson. To compute this feature, we calculate the ratio of the cumulative time the warning sign was displayed to the total sum of deals time, and assign as a feature to that BizDomain (company using the software).
2. **ActiveUsers/AddedUsers**: The feature we compute is the ratio of actively engaged users within the BizDomain to the total number of users initially added to the software. Notably, not all users within a BizDomain are active software users. Some users may remain inactive, even though their manager originally purchased a subscription plan with more user slots than required, which could potentially be a contributing factor to the company's churn risk.
3. **Pending/Whole_TransSpeed**: We calculate a crucial feature by comparing the number of deals that are still pending against the number of deals closed within the last two months. However, we impose a condition that only deals exceeding the previous conversion rate of that specific BizDomain's performance are categorized as 'Pending.' It's worth noting that Didar CRM serves a diverse clientele, including manufacturing and industrial companies, as well as online businesses, particularly on platforms like Instagram and Telegram. The process of selling products and the conversion rate from deal registration to closure can vary significantly across these different sectors. As such, it's essential to consider their unique conversion rates. A higher value for this feature indicates a less favorable performance.
4. **DealsRatio**: We calculate a vital performance indicator by assessing the ratio of deals registered in the previous month compared to the month before that. An increasing ratio suggests an improved performance. However, in the event that the ratio exceeds 1 (indicating more deals registered in the previous month than the month before), we cap it at 1, not allowing it to surpass that threshold.
5. **ActivitiesRatio**: Same as the last feature, but for activities.

## Identifying the good/bad bizdomains and feature importance
After calculating the various features, we employed several fundamental machine learning algorithms to determine the relative importance of each feature. To achieve this, we initially identified companies that exhibited poor performance, collaborating closely with the Customer Success (CS) team. Concurrently, we identified companies that demonstrated strong performance, sorting the features in descending order of significance. Subsequently, we labeled these BizDomains accordingly.

We then applied the K-Medoids clustering algorithm to assign labels to unknown BizDomains based on their similarity to the poorly and well-performing BizDomains. Following this, we applied the aforementioned basic machine learning algorithms. Ultimately, we chose the Linear Support Vector Classifier to utilize its feature weights in the final phase of the project.

While this process may appear unconventional, it aligns with the CTL (cluster then label) semi-supervised learning task methodology, as indicated in the literature. Additionally, we have implemented different decision-making weight calculation algorithms, which are available in a separate notebook named 'Weight Calculation.ipynb.' The results from this notebook were integral in determining the weights for Case's Health Score.


## Calculating the Health Score
The best performance a bizodmain can show is having features equal to one in all features which in this case, its health score is equal to 10. otherwise, it would obtain a health score lower than 10. To calculate this, euclicidean weighted distance to the optimal state was calculated for each bizdomain and then scaled to 0-10. For example consider the weights for the 5 mentioned features as (0.1, 0.2, 0.3, 0.15, 0.25) and the the features as (1, 0.75, 0.6, 0.7, 0.5, 0.2). It's score is:

HS = 1 - \sqrt{0.1(1-1)^2 + 0.2(1-0.75)^2 + 0.3(1-0.6)^2 + 0.15(1-0.5)^2 + 0.25(1-0.2)^2} = 0.505

This score would be later multiplied by 10 and the final Health Score is 5.05.


Thank you for reading this project description, let me know if you have any questions!😃😃
