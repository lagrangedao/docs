# Bidding Task State Machine

The bidding task state machine is a system designed to manage the bidding process for tasks, taking into account task details such as price and timeout. In this setup, each task allows a maximum of three bidders to compete simultaneously, with each bidder being assigned a job to complete.

Bidders have the ability to set a limit on the number of bids they can process at the same time. This feature prevents them from accepting new bids once they reach their specified limit, enabling bidders to effectively manage their workload and participate in multiple tasks without overextending themselves.

### States

The Bidding State Machine has several predefined states:

1. **Created (initial state):** The task is created but has not yet started accepting bids.
2. **Accepting\_Bids:** The task is open for bidding, and bidders can submit their bids.
3. **Processing:** The bidding has closed, and the task is being processed.
4. **Submitted:** The task has been processed and is now awaiting approval.
5. **Completed:** The task has been approved and marked as completed.
6. **Failed:** The task has failed during processing, requiring manual intervention or resubmission.
7. **Cancelled:** The task has been canceled, either due to a failed bidding process or other reasons.\


* `open_bidding`: Transition from Created to Accepting\_Bids.
* `close_bidding`: Transition from Accepting\_Bids to Processing.
* `cancel_bid`: Transition from Accepting\_Bids to Cancelled.
* `failed_bids`: Transition from Accepting\_Bids to Cancelled.
* `complete_task`: Transition from Submitted to Completed.
* `mark_as_submitted`: Transition from Processing to Submitted.
* `mark_as_failed`: Transition from Processing to Failed.
* `reset_accepting_bids_to_created`: Transition from Accepting\_Bids to Created.
* `reset_failed_bids_to_created`: Transition from Failed to Created.

The Bidding State Machine has defined transitions between states:

### Transition Between States

1. open\_bidding: Transition from 'Created' to 'Accepting\_Bids'.
2. close\_bidding: Transition from 'Accepting\_Bids' to 'Processing'.
3. cancel\_bid: Transition from 'Accepting\_Bids' to 'Cancelled'.
4. failed\_bids: Transition from 'Accepting\_Bids' to 'Cancelled'.
5. complete\_task: Transition from 'Submitted' to 'Completed'.
6. mark\_as\_submitted: Transition from 'Processing' to 'Submitted'.
7. mark\_as\_failed: Transition from 'Processing' to 'Failed'.
8. reset\_accepting\_bids\_to\_created: Transition from 'Accepting\_Bids' to 'Created'.
9. reset\_failed\_bids\_to\_created: Transition from 'Failed' to 'Created'.

### Rules

The bidding task state machine should include the following rules:

* A bidder cannot place a bid if they have exceeded their limit on the number of jobs they can process simultaneously.
* Once a bidder has completed a job, they cannot be assigned any further jobs on the same task.

If the task is cancelled, all bids and jobs associated with the task are cancelled as well
