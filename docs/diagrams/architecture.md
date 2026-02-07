## Product Choice

- Wildberries
- [website](https://www.wildberries.ru/)
- Wildberries (Russian: Уайлдберрис, Uayldberris) is the largest Russian online retailer. It was founded in 2004 by Tatyana Kim.

## Main components
![Wildberries Component Diagram](diagrams/out/wildberries/architecture-component/Component%20Diagram.svg)

- **Customer Mobile App**: The app people use on their phones to shop—browse items, add to cart, and place orders. It talks to Wildberries’ servers through the internet.

- **Storefront Gateway**: Like a reception desk for the website and app—it receives all customer requests and sends them to the right internal service (e.g., “show me products” → Catalog Service).

- **Catalog & Search Service**: Handles everything about products: what’s available, descriptions, prices—and makes search fast (e.g., when you type “phone” and get results instantly).

- **Order Management (OMS)**: The system that manages your order from start to finish—checks if payment went through, reserves items, and tells logistics where to send it.

- **Event Bus (Kafka)**: A messaging system that lets different parts of the platform talk to each other *without waiting*—for example, when an order is placed, it quietly tells the notification service “send a confirmation SMS” in the background.

## Data Flow
![Wildberries Sequence Diagram](Lab_01\lab-01-market-product-and-git\docs\diagrams\src\wildberries\architecture-deployment.puml)

After the user completes payment in the bank's page, the bank sends a success webhook the Order Management Service (OMS). OMS processes the callback, updates the order status to **PAID**, commits the stock reservation via Kafka, and acknowledges the payment gateway — all without blocking warehouse operations.


## Deployment


## Assumption

