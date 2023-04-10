# Bidding Engine

The Bidding Engine in the Decentralized Bidding Marketplace serves as the core component that manages the task publishing process and the competitive bidding among providers. It ensures an efficient and transparent mechanism for matching tasks with the most suitable bidders. The data structure for each task in the platform includes:

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
