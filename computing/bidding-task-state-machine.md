# Bidding Task State Machine

The bidding task state machine is a system designed to manage the bidding process for tasks, taking into account task details such as price and timeout. In this setup, each task allows a maximum of three bidders to compete simultaneously, with each bidder being assigned a job to complete.

Bidders have the ability to set a limit on the number of bids they can process at the same time. This feature prevents them from accepting new bids once they reach their specified limit, enabling bidders to effectively manage their workload and participate in multiple tasks without overextending themselves.

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

### States

The Bidding State Machine has several predefined states:

1. **created**: This is the initial state when a task is first created. The task stays in this state until bidding is opened.
2. **accepting\_bids**: In this state, the task is open for bidders to place their bids. The task remains in this state until bidding is closed, the bid is cancelled, or the bid fails.
3. **bidding\_closed**: This state indicates that the bidding process for the task has ended. The task transitions to this state from the 'accepting\_bids' state. From here, the task can either be marked as 'submitted' or 'failed'.
4. **submitted**: This state signifies that the task has been submitted successfully. The task moves to this state from the 'bidding\_closed' state. Once a task is in the 'submitted' state, it can then be completed.
5. **completed**: This is the final state indicating that the task has been completed successfully. The task transitions to this state from the 'submitted' state.
6. **failed**: This state indicates that the task has failed. The task can enter this state from the 'bidding\_closed' state. If a task fails, it can be reset to the 'created' state.
7. **cancelled**: This state signifies that the bid for the task has been cancelled. The task can enter this state from the 'accepting\_bids' state. If a bid is cancelled or fails, the task can be reset to the 'created' state.

The transitions between these states are managed by the state machine, which ensures that the task moves through its lifecycle in a controlled and predictable manner.

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
