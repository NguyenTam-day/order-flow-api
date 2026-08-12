# Class Diagram - OrderFlow Domain Model

``` mermaid
classDiagram
    direction LR

    class Employee {
        +Int id
        +String telegramUserId
        +String name
        +Role role
        +String phone
        +Boolean isActive
        +String pin
        +login()
        +createOrder()
        +claimOrder()
        +updateFulfillmentStatus()
    }

    class Role {
        <<enumeration>>
        SERVICE_STAFF
        BARISTA
        MANAGER
    }

    class MenuCategory {
        +Int id
        +String name
        +Int sortOrder
    }

    class MenuItem {
        +Int id
        +String name
        +Decimal price
        +String description
        +Boolean isAvailable
    }

    class Order {
        +Int id
        +String orderCode
        +String customerName
        +Decimal totalAmount
        +PaymentStatus paymentStatus
        +FulfillmentStatus fulfillmentStatus
        +DateTime createdAt
        +DateTime updatedAt
        +addItem()
        +applyPayment()
        +advanceStatus()
    }

    class PaymentStatus {
        <<enumeration>>
        UNPAID
        PENDING
        PAID
        UNDERPAID
        OVERPAID
        PAYMENT_REVIEW
    }

    class FulfillmentStatus {
        <<enumeration>>
        PENDING_PAYMENT
        QUEUED
        PREPARING
        READY
        DELIVERED
    }

    class OrderItem {
        +Int id
        +Int quantity
        +Decimal unitPrice
        +Decimal subtotal
        +String note
    }

    class Payment {
        +Int id
        +PaymentMethod method
        +Decimal amount
        +String reference
        +DateTime confirmedAt
        +confirmCash()
        +generateQR()
    }

    class PaymentMethod {
        <<enumeration>>
        CASH
        QR
    }

    class SePayTransaction {
        +Int id
        +String transactionId
        +String bankCode
        +Decimal transferAmount
        +String content
        +String status
        +DateTime receivedAt
        +verifyAmount()
        +verifyContent()
    }

    class Reconciliation {
        +Int id
        +Date date
        +Int totalOrders
        +Decimal totalCash
        +Decimal totalQr
        +Decimal expectedRevenue
        +Decimal actualRevenue
        +Decimal diff
        +String status
        +reconcile()
    }

    class Notification {
        +Int id
        +String type
        +Json payload
        +DateTime sentAt
        +Boolean delivered
        +sendTelegram()
    }

    class AuditLog {
        +Int id
        +String action
        +String entityType
        +Int entityId
        +Json changes
        +DateTime createdAt
        +record()
    }

    Employee "1" --> "*" AuditLog : performs
    Employee "1" --> "1" Role : has
    Employee "1" --> "0..*" Notification : receives

    MenuCategory "1" --> "*" MenuItem : contains
    Order "1" --> "*" OrderItem : contains
    MenuItem "1" --> "*" OrderItem : is ordered as
    Order "1" --> "1" Payment : paid by
    Payment "1" --> "*" SePayTransaction : verified by
    Order "*" --> "1" Reconciliation : included in
    Order "1" --> "1" FulfillmentStatus : has
    Order "1" --> "1" PaymentStatus : has
    Payment "1" --> "1" PaymentMethod : uses
```
