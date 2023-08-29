# Provider Reputation System

The Lagrange reputation system is designed to assess and quantify the trustworthiness and reliability of individual computing providers in the Lagrange computing network. It helps the bidding engine make informed decisions when assigning tasks to providers and promotes accountability within the network. Such a reputation system considers the recent performance, behavior, and feedback received by the providers to establish their reputation scores in a dynamic way.

In this system, each computing provider is assigned a reputation score between 0 and 100 that reflects their past performance and interactions within the network. The reputation score can be based on various factors, including success rate, reachability, and region. Higher reputation scores generally indicate more reliable and trustworthy providers, increasing the likelihood that they will receive tasks they can be rewarded for completing. Likewise, providers with lower reputation scores may need to improve their performance to attract more users and enhance their reputation.

## Scoring Computing Providers

The scoring method takes into account various factors to assign scores to computing providers. These factors include:
1. Reliability and Uptime: Providers that consistently maintain high availability and uptime, minimizing downtime and interruptions, are considered more reliable.

2. Task Completion Rate: Providers that consistently complete assigned tasks and return a valid and excellent result can increase their success rate, further increasing their overall score.

3. Region: The region score of a provider aims to reflect the relative scarcity or abundance of providers in a specific region and adjusts the score accordingly. By assigning a higher region score to providers in regions with fewer competitors, the scoring system incentivizes the establishment and growth of computing infrastructure in underserved or less populated areas. 

## The Score Equation

```
Overall Reputation Score = Base Score + 65 * Weekly Score + 25 * Monthly Score + 10 * Region Score
```
**Base Score:** The initial, and thus base, score that everyone will start with assuming the other weighted factors are unitialized and/or 0.

**Weekly Score:** Reflects the provider's performance on a weekly basis. Given the highest weighting to emphasize recent performance. Notice that this score can be negative if recent performance is very poor!

**Monthly Score:** Reflects the provider's performance on a monthly basis. This score can also be negative if a provider behaves poorly during the period.

**Region Score:** Takes into account the provider's location and the number of competitors in that region.

Overall, a provider's reputation score will still be capped between 0 and 100.


## How is Weekly and Monthly Scores Calculated?

#### Monthly example:
```
Monthly Score = 50 * Monthly reachability + 50 * Monthly success rate
```
The equation for a weekly score is the same except that the time period of which reachability and success rate are calculated will obviously differ.

### Reachability: 
```
(# of successful requests - # of failed requests) / total # of requests made to provider by server
```
The Lagrange server will periodically send a ping to each provider. If the provider receives the ping and the server receives a response back, then the request will be saved as a success. Otherwise, the request will be saved as failed.

### Success Rate:
```
(# of completed tasks - # of failed/incomplete tasks) / total # of tasks assigned to provider
```
This equation provides a quantitative measure of a provider's performance and success in completing assigned tasks. Clearly, higher success rate indicates a higher level of reliability, efficiency, and effectiveness in task completion. On the other hand, a lower success rate may indicate room for improvement or potential issues in fulfilling assigned tasks.

## How is Region Score Calculated?
```
Region Score = 1 - ( # of providers in the region / total # of providers ) 
```
So, if for example 20% of providers globally reside in North America, then a provider in this region would get a region score of `1 - 0.2 = 0.8`. Once again, by assigning a higher region score to providers in regions with limited competition, the scoring system addresses potential imbalances in service distribution and also promotes regional growth.
