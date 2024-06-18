# testTask
![img.png](assets%2Fimg.png)

```PUML
title Payment  Process in Online Shop (Test task)

actor Customer
participant "Web Interface (UI)" as Web
participant "Backend Service" as Srv
participant "Database" as DB
participant "Payment Gateway" as PG

Customer -> Web: Select product and add to cart
activate Web
Web -> Srv: Check product availability
activate Srv
Srv -> DB: Check product availability
activate DB
DB --> Srv: Confirm availability
Srv -> DB: Reserve product
DB --> Srv: Confirm reserve
deactivate DB
Srv --> Web: Update product count
deactivate Srv

Web --> Customer: UI update
deactivate Web
Customer -> Web: Select cart
activate Web
Web --> Customer: Display cart
Customer -> Web: Proceed to checkout

Web -> Srv: Submit order details
activate Srv
Srv -> DB: Order & products status update
activate DB
DB --> Srv: Confirm
deactivate DB
Srv -> Web: Success result
deactivate Srv

Web --> Customer: Open payment page
deactivate Web
Customer -> Web: Starts the payment process
activate Web

Web -> Srv: Payment checkout
activate Srv
Srv -> DB: Init payment session
activate DB
DB --> Srv: Result
deactivate DB
Srv -> PG: Process payment
activate PG

PG -> PG: Validate card details
PG -> PG: Check balance
PG -> Srv: Operation result (success or failure)
deactivate PG

Srv -> DB: Update payment session
activate DB
DB --> Srv: Result
deactivate DB

alt Successful Payment
    Srv -> DB: Update order status to "paid"
    activate DB
    DB --> Srv: Result
    deactivate DB
    Srv -> Web: Success URL redirect
else Unsuccessful Payment
    Srv -> DB: Update order status to "payment error"
    activate DB
    DB --> Srv: Result
    deactivate DB
    Srv -> Web: Failed URL redirect
end

deactivate Srv
Web -> Customer: Display payment result
deactivate Web
```