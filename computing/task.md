## Task definition
Bidders have the ability to set a limit on the number of bids they can process at the same time. This feature prevents
them from accepting new bids once they reach their specified limit, enabling bidders to effectively manage their
workload and participate in multiple tasks without overextending themselves.
User can convert a space to a task:

| Role     | Action         |
|----------|----------------|
| Task     | Created        |
| Task     | Accepting Bids |
| Task     | Bid Closed     |
| Task/Job | Processing     |
| Job      | Submitted      |
| Task/Job | Completed      |
| Task/Job | Failed         |
| Task     | Cancelled      |

## Bidding State Machine

![readme_lagrange_machine.png](readme_lagrange_machine.png)

## Task Types

### Resource Usage

Use scenario - space

* When use create/update a space, a default task is created.
* The task is stored in Mysql and create a task in redis as well.
* After the task in redis, the user can start bidding
* User can query its status in redis.
* if the status reached "completed" or "Failed".
* The status will be finalized in Mysql

### API Requests

## Task Publishing

A task need to announce the following factors:

* Resource needs
* Region
* Time
* Pricing

Task information is created and insert to the celery task queue, then it enters the bidding statemachine.

### Example

* When use create/update a space, it is automatically converted to a task
    * task schedule check the hook every minutes
    * it can be in a task queue
* If the reserved hours expired
    * User can manually trigger it

## Computing Provider (CP) Registration

Computing Provider publish its host information on chain:

* Host IP
* Host port
* Owner Address

The v1 version provide a platform for easy signup.

## Task Bidding

### Pipeline

* Producer(Lagrange) Publish the task
* Task is open to bidding status
* CP bidding for the job
* Bidding close when
    * Bid limit reached
    * Task owner decide to close it
* Processing the task
* Each bidder uploaded their bidding result
* The winner will be marked as completed and elector as the leading node
* Task Published and Bidder get paid

### Bid negotiation

A task will generate a bidder id when a bidder join the bidding process.
Bidder will get a job if he gets the bid.
If the ask price in the range of bid price, the SP will win the bid

### Liquidity Provider

Liquidity provider is offering the price match for different type of orders.e.g. storage, computing

## Payment

User pay for the storage when he first time create the space, he can extend it if he wants.