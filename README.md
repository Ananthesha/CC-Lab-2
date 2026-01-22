# CC LAB – 2  
## Performance Optimizations

This project focuses on identifying performance bottlenecks in a FastAPI application and optimizing them to improve scalability and response times. Load testing was performed using **Locust** to validate the improvements.

---

## 1. Checkout Route Optimization (`/checkout`)

### Bottleneck  
Inefficient loop logic in fee calculation.

The original implementation calculated the total fee using a loop that incremented the total one unit at a time.

### Before (Inefficient Implementation)
```python
while fee > 0:
    total += 1
    fee -= 1
```
Time Complexity: O(n) where n = fee

For large fees (e.g., 1000), this resulted in 1000 iterations per event

Caused unnecessary arithmetic operations
After (Optimized Implementation)
```
for e in events:
    total += e[0]
```

Why Performance Improved:
Reduced time complexity from O(sum of fees) to O(number of events)
Eliminated unnecessary loop iterations
Direct addition instead of repetitive increments
Improved scalability and response time

2. Events Route Optimization (/events)
Bottleneck
Unnecessary computational waste loop.
The route contained a CPU-intensive loop that performed meaningless arithmetic operations.
Before (Inefficient Implementation)

```
waste = 0
for i in range(3000000):
    waste += i % 3
```
3,000,000 arithmetic operations per request
CPU-intensive and blocked request processing
Increased response time significantly

After (Optimized Implementation)
```
Removed completely
```
Why Performance Improved
Eliminated 3 million arithmetic operations per request
Removed artificial CPU blocking
Response time reduced from seconds to milliseconds
Improved throughput under concurrent load

3. My Events Route Optimization (/my-events)
Bottleneck:
Dummy counter loop causing artificial computational delay.
Before (Inefficient Implementation)
```
dummy = 0
for _ in range(1500000):
    dummy += 1
```
1.5 million increment operations per request
No functional purpose
Wasted CPU cycles

After (Optimized Implementation)
```
Removed completely
```
Why Performance Improved
Eliminated unnecessary computation
Immediate response after database query completion
Reduced CPU usage and improved latency
Performance Testing
Load testing was performed using Locust to evaluate system performance before and after optimizations.

Locust Commands
# Events route
locust -f locust/events_locustfile.py

# My Events route
locust -f locust/myevents_locustfile.py

# Checkout route
locust -f locust/checkout_locustfile.py

Key Learnings:
Avoid unnecessary computations inside request handlers
Optimize algorithmic complexity for better scalability
Measure performance before and after changes
Simple optimizations can yield dramatic improvements
Load testing tools like Locust are essential for validating optimizations

Conclusion:
After applying the optimizations:
CPU usage was significantly reduced
Response times improved drastically
Application handled concurrent users more efficiently
Overall scalability and performance were enhanced
