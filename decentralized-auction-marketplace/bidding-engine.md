# Bidding Engine

The auction engine is a critical component of the LagrangeDAO system. It manages the bidding process for tasks, ensuring that tasks are assigned to the most suitable computing providers. Here's a breakdown of its key functionalities:

1. **Load Provider Pool**: The auction engine initially loads all active computing providers into a pool. These providers are potential bidders for tasks.
2. **Place Bid**: When a task is open for bidding, the auction engine allows a computing provider (bidder) to place a bid on the task. The bid is only successful if the task is currently accepting bids, the bidder has not already placed a bid, and the bidder's collateral is sufficient.
3. **Load Tasks from Redis**: The auction engine fetches all tasks from Redis that are in a state where they can accept bids. It also handles state transitions for tasks, such as moving a task from the 'accepting\_bids' state to the 'bidding\_closed' state when the bidding period ends.
4. **Select Bidders**: The auction engine selects bidders based on certain criteria. For example, it might select the bidders with the highest collateral.
5. **Run Bidding Process**: For each task that is open for bidding, the auction engine runs the bidding process. It allows the selected bidders to place their bids on the task.
6. **List Tasks Available for Bidding**: The auction engine can provide a list of all tasks that are currently open for bidding.

The auction engine is designed to be fair and efficient, ensuring that tasks are distributed evenly among computing providers and that the bidding process is competitive. It plays a crucial role in the operation of the LagrangeDAO network.

&#x20;The data structure for each task in the platform includes:

* uuid: A unique identifier for the task.
* status: The current status of the task (e.g., open, closed, in progress, completed).
* task\_detail\_cid: A content identifier for the task details, stored on a decentralized storage system like IPFS.
* type: The type or category of the task.
* reference\_id: A reference ID for linking related tasks or resources.
* name: The name or title of the task.
* leading\_job\_id: The ID of the job currently in the leading processing status, used for tracking purposes.
* created\_at: The timestamp when the task was created.
* updated\_at: The timestamp when the task was last updated.
* user\_id: The ID of the user who created the task.

When a user publishes a task, multiple providers (blockchain nodes worldwide) can bid on the task. The Bidding Engine evaluates these bids and assigns the task to several bidders with the potential to complete the task effectively. Once they complete the task, the Bidding Engine assesses the quality of their work, updating the leading\_job\_id as necessary to keep track of the best-performing bidder.

Finally, the provider who delivers the highest-quality work is marked as successful and receives a reward from the task publisher. By employing this mechanism, the Bidding Engine promotes efficiency and transparency in the Decentralized Bidding Marketplace, ensuring that tasks are matched with the most suitable providers and completed to the highest standards.

### Autobid

The Decentralized Bidding Marketplace can be configured to include an auto-bid mode for providers, which allows them to automatically participate in all bids without manual intervention. This feature can be particularly useful for providers who want to streamline their bidding process and maximize their chances of securing tasks.

To enable the auto-bid mode, providers need to set up their capability and resource availability for bidding. This information includes the type of resources they can offer (such as storage, network bandwidth, CPU, and GPU), their capacity for each resource, and any other relevant details that may impact their ability to complete tasks.

When auto-bid mode is enabled, the Bidding Engine automatically pushes tasks to the provider based on their configured capabilities and resource availability. The Bidding Engine evaluates the provider's suitability for each task and includes their bid in the competitive bidding process. This automatic participation ensures that providers have a constant presence in the marketplace and can secure tasks that match their expertise and resources.
