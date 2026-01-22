CC LAB - 2
Performance Optimizations
1. Checkout Route Optimization (/checkout)
Bottleneck: Inefficient loop logic in fee calculation

Original code used while fee > 0: loop with total += 1 and fee -= 1
This created O(n) time complexity where n = sum of all event fees
For large fees (e.g., 1000), this meant 1000 iterations per fee
Change: Replaced with direct fee summation

Before: while fee > 0: total += 1; fee -= 1

After: for e in events: total += e[0]
Why performance improved:

Reduced time complexity from O(sum of fees) to O(number of events)
Eliminated unnecessary arithmetic operations
Direct addition instead of repetitive incrementing
2. Events Route Optimization (/events)
Bottleneck: Unnecessary computational waste loop

3,000,000 iterations of waste += i % 3 calculation
CPU-intensive operations blocking request processing
Change: Removed the entire wasteful computation

Before: 
waste = 0 
for i in range(3000000): 
    waste += i % 3

After: Removed completely
Why performance improved:

Eliminated 3M arithmetic operations per request
Removed artificial CPU blocking
Response time reduced from seconds to milliseconds
3. My Events Route Optimization (/my-events)
Bottleneck: Dummy counter loop

1,500,000 iterations of dummy += 1 increment
Artificial computational delay serving no purpose
Change: Removed the entire dummy computation

Before: 
dummy = 0
for _ in range(1500000): 
    dummy += 1

After: Removed completely  
Why performance improved:

Eliminated 1.5M increment operations per request
Removed unnecessary CPU cycles
Immediate response after database query completion
Performance Testing
Use Locust for load testing:

Events route: locust -f locust/events_locustfile.py
My Events route: locust -f locust/myevents_locustfile.py
Checkout route: locust -f locust/checkout_locustfile.py
Key Learnings
Avoid unnecessary computations in request handlers
Optimize algorithmic complexity for better scalability
Profile and measure performance before and after changes
Simple optimizations can yield dramatic improvements
