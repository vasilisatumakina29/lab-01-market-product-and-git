## Product Choice

- Wildberries
- [Link](https://www.wildberries.ru/)
- Wildberries (Russian: Уайлдберрис, Uayldberris) is the largest Russian online retailer. It was founded in 2004 by Tatyana Kim.

## Main components

![Wildberries Component Diagram](diagrams/out/wildberries/architecture-component/Component%20Diagram.svg)

[Wildberries Component Diagram](https://github.com/vasilisatumakina29/lab-01-market-product-and-git/blob/architecture_description/docs/diagrams/src/wildberries/architecture-component.puml)

- **Customer Mobile App**: The app people use on their phones to shop—browse items, add to cart, and place orders. It talks to Wildberries’ servers through the internet.

- **Storefront Gateway**: Like a reception desk for the website and app—it receives all customer requests and sends them to the right internal service (e.g., “show me products” → Catalog Service).

- **Catalog & Search Service**: Handles everything about products: what’s available, descriptions, prices—and makes search fast (e.g., when you type “phone” and get results instantly).

- **Order Management (OMS)**: The system that manages your order from start to finish—checks if payment went through, reserves items, and tells logistics where to send it.

- **Event Bus (Kafka)**: A messaging system that lets different parts of the platform talk to each other *without waiting*—for example, when an order is placed, it quietly tells the notification service “send a confirmation SMS” in the background.

## Data Flow

![WildBerries Sequence Diagram](diagrams/out/wildberries/architecture-sequence/Sequence%20Diagram.svg)
 
[Wildberries Sequence Diagram](https://github.com/vasilisatumakina29/lab-01-market-product-and-git/blob/architecture_description/docs/diagrams/src/wildberries/architecture-sequence.pumll)

I picked the group called **“Warehouse Fulfillment (Async)”**.

After the order is paid, the warehouse gets to work. It finds the item, packs it, and marks the order as ready to ship — all in the background while the customer waits.

Who talks to whom and what they send:
- The system tells the **warehouse** that an order is paid and ready.
- The **warehouse** checks its database, packs the item, and updates the order status.
- Then it sends a message like *“Your order is packed!”* back to the **customer’s app**.


## Deployment

![WildBerries Deployment Diagram](diagrams/out/wildberries/architecture-deployment/Deployment%20Diagram.svg)

[Wildberries Diployment Diagram](https://github.com/vasilisatumakina29/lab-01-market-product-and-git/blob/architecture_description/docs/diagrams/src/wildberries/architecture-deployment.puml)

- **Apps** (like the WB mobile app) run on users’ phones or computers.  
- **All Wildberries services** (gateways, order system, payment, etc.) run in their main data center, inside containers (Kubernetes).  
- **Databases** (for orders, search, files, etc.) are also in that same data center, but in separate, secure clusters.  
- **Kafka** (for sending messages between services) runs in the same data center too.  
- **External partners** (banks, delivery companies, SMS services) are outside — Wildberries just sends them requests over the internet.

## Assumption

- *I assume the Event Bus is used for all async operations, so the system stays fast and does not wait for slow steps.*  
- *I assume cache stores user sessions and cart data close to the gateway, so frequent actions are super fast.*

## Open Questions

- How are failed payments retried or canceled — automatically, or does a human need to step in?
- What happens if the warehouse runs out of stock after? 