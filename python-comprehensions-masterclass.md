# Python Comprehensions Master Class
*Complete Guide for Data Analysis & Web Development*

## Table of Contents
1. [The General Idea of Comprehensions](#1-general-idea)
2. [List Comprehension](#2-list-comprehension)
3. [Set Comprehension](#3-set-comprehension)
4. [Dictionary Comprehension](#4-dictionary-comprehension)
5. [Generator Comprehension](#5-generator-comprehension)
6. [Best Practices & Performance](#6-best-practices)

---

## 1. The General Idea of Comprehensions

### The Concept
**Comprehensions** are Python's elegant way to create collections (lists, sets, dictionaries, generators) in a concise, readable manner. They combine loops, conditionals, and transformations into a single line of code.

**Why use comprehensions?**
- **Readability**: More Pythonic and expressive
- **Performance**: Often faster than traditional loops
- **Memory efficiency**: Generator comprehensions use lazy evaluation
- **Functional style**: Encourages immutable data transformations

### The Syntax
```python
# General pattern for all comprehensions
[expression for item in iterable if condition]     # List
{expression for item in iterable if condition}     # Set
{key: value for item in iterable if condition}     # Dict
(expression for item in iterable if condition)     # Generator
```

---

## 2. List Comprehension

### The Concept
Creates a new list by applying an expression to each item in an iterable, optionally filtering with conditions. Most commonly used comprehension type.

### The Syntax
```python
# Basic syntax
new_list = [expression for item in iterable]

# With condition
new_list = [expression for item in iterable if condition]

# Nested loops
new_list = [expression for item1 in iterable1 for item2 in iterable2]
```

### Mini-Project: E-commerce Analytics Dashboard

#### Sample Data Setup
```python
products = [
    {'id': 1, 'name': 'Laptop', 'price': 999.99, 'category': 'Electronics', 'stock': 15, 'rating': 4.5},
    {'id': 2, 'name': 'Mouse', 'price': 29.99, 'category': 'Electronics', 'stock': 50, 'rating': 4.2},
    # ... more products
]
```

#### 1. Basic List Comprehension
```python
# Extract product names
product_names = [product['name'] for product in products]
# Result: ['Laptop', 'Mouse', 'Keyboard', ...]
```

#### 2. Transformation
```python
# Format prices for display
formatted_prices = [f"${product['price']:.2f}" for product in products]
# Result: ['$999.99', '$29.99', '$79.99', ...]

# Calculate discounted prices (20% off)
discounted_prices = [product['price'] * 0.8 for product in products]
```

#### 3. Conditional Filtering
```python
# Find electronics products over $50
expensive_electronics = [
    product['name'] for product in products 
    if product['category'] == 'Electronics' and product['price'] > 50
]

# Low stock alerts
low_stock_products = [
    f"{product['name']} ({product['stock']} left)" 
    for product in products 
    if product['stock'] < 10
]
```

#### 4. Complex Analysis
```python
# Calculate performance score: (rating * stock) / price * 100
performance_scores = [
    {
        'name': product['name'],
        'score': round((product['rating'] * product['stock']) / product['price'] * 100, 2)
    }
    for product in products
]
```

#### 5. HTML Generation for Web Development
```python
# Generate HTML product cards
html_cards = [
    f"""<div class="product-card" data-id="{product['id']}">
        <h3>{product['name']}</h3>
        <p class="price">${product['price']:.2f}</p>
        <p class="category">{product['category']}</p>
        <p class="rating">★ {product['rating']}/5</p>
        <p class="stock">Stock: {product['stock']}</p>
    </div>"""
    for product in products
]
```

---

## 3. Set Comprehension

### The Concept
Creates a new set using curly braces `{}`. Sets automatically eliminate duplicates and are perfect for finding unique values, removing duplicates, and set operations.

### The Syntax
```python
# Basic syntax
new_set = {expression for item in iterable}

# With condition
new_set = {expression for item in iterable if condition}
```

### Mini-Project: Web Analytics Unique Visitor Tracking

#### Sample Data Setup
```python
web_traffic = [
    {'ip': '192.168.1.10', 'page': '/home', 'user_agent': 'Chrome', 'status_code': 200, 'response_time': 150},
    {'ip': '192.168.1.15', 'page': '/products', 'user_agent': 'Firefox', 'status_code': 200, 'response_time': 230},
    # ... more traffic entries
]
```

#### 1. Basic Set Comprehension
```python
# Unique visitors (IP addresses)
unique_ips = {entry['ip'] for entry in web_traffic}
# Automatically removes duplicates
```

#### 2. Conditional Filtering
```python
# Unique pages with successful visits
successful_pages = {
    entry['page'] for entry in web_traffic 
    if entry['status_code'] == 200
}

# User agents with fast response times
fast_user_agents = {
    entry['user_agent'] for entry in web_traffic 
    if entry['response_time'] < 500
}
```

#### 3. Complex Analysis
```python
# Browser-page combinations
browser_page_combos = {
    f"{entry['user_agent']}:{entry['page']}" 
    for entry in web_traffic 
    if entry['status_code'] == 200
}
```

#### 4. Security Analysis
```python
# IPs with failed access to sensitive pages
sensitive_pages = ['/login', '/checkout']
suspicious_ips = {
    entry['ip'] for entry in web_traffic
    if entry['page'] in sensitive_pages and entry['status_code'] != 200
}
```

#### 5. Set Operations for Funnel Analysis
```python
# Conversion funnel analysis
product_visitors = {entry['ip'] for entry in web_traffic if entry['page'] == '/products'}
checkout_visitors = {entry['ip'] for entry in web_traffic if entry['page'] == '/checkout'}

# Visitors who browsed but didn't buy
browsed_not_bought = product_visitors - checkout_visitors

# High-intent visitors
high_intent_visitors = product_visitors & checkout_visitors
```

---

## 4. Dictionary Comprehension

### The Concept
Creates dictionaries using concise syntax. Powerful for transforming data, creating lookup tables, grouping data, and building key-value mappings.

### The Syntax
```python
# Basic syntax
new_dict = {key_expression: value_expression for item in iterable}

# With condition
new_dict = {key_expression: value_expression for item in iterable if condition}

# From existing dict
new_dict = {key: value for key, value in old_dict.items() if condition}
```

### Mini-Project: Customer Analytics & API Response Builder

#### Sample Data Setup
```python
users = [
    {
        'id': 1, 'email': 'user1@example.com', 'age': 25, 'subscription': 'premium',
        'active': True, 'total_spent': 2500.00, 'location': 'New York'
    },
    # ... more users
]
```

#### 1. Basic Dictionary Comprehension
```python
# Create lookup tables
user_by_id = {user['id']: user['email'] for user in users}
email_to_user = {user['email']: user['id'] for user in users}
```

#### 2. Data Transformation for APIs
```python
# Create API-ready user profiles
api_user_profiles = {
    user['id']: {
        'display_name': user['email'].split('@')[0].title(),
        'account_status': 'Premium' if user['subscription'] == 'premium' else 'Standard',
        'customer_value': 'High' if user['total_spent'] > 2000 else 'Medium' if user['total_spent'] > 500 else 'Low',
        'engagement_level': 'Active' if user['active'] else 'Inactive'
    }
    for user in users
}
```

#### 3. Customer Segmentation
```python
# High-value active customers
high_value_customers = {
    user['email']: user['total_spent'] 
    for user in users 
    if user['total_spent'] > 1000 and user['active']
}
```

#### 4. Data Aggregation by Location
```python
# Group users by location first (helper step)
from collections import defaultdict
location_data = defaultdict(list)
for user in users:
    location_data[user['location']].append(user)

# Aggregate metrics by location
location_analytics = {
    location: {
        'user_count': len(users_in_location),
        'avg_spent': sum(u['total_spent'] for u in users_in_location) / len(users_in_location),
        'premium_users': sum(1 for u in users_in_location if u['subscription'] == 'premium')
    }
    for location, users_in_location in location_data.items()
}
```

#### 5. RESTful API Response Generation
```python
# GET /analytics/dashboard endpoint
dashboard_response = {
    'metrics': {
        'total_users': len(users),
        'active_users': sum(1 for u in users if u['active']),
        'premium_users': sum(1 for u in users if u['subscription'] == 'premium'),
        'total_revenue': sum(u['total_spent'] for u in users)
    },
    'subscription_breakdown': {
        subscription: len([u for u in users if u['subscription'] == subscription])
        for subscription in ['basic', 'premium', 'enterprise']
    }
}
```

---

## 5. Generator Comprehension for Memory Optimization

### The Concept
Creates generator objects that produce values on-demand using lazy evaluation. Critical for memory optimization with large datasets as it doesn't store all values in memory at once.

### The Syntax
```python
# Basic syntax
generator = (expression for item in iterable)

# With condition
generator = (expression for item in iterable if condition)

# Must iterate to get values
for value in generator:
    print(value)
```

### Mini-Project: Big Data Processing Pipeline

#### Memory Usage Comparison
```python
# List comprehension - loads all into memory
error_list = [log['id'] for log in logs if log['level'] == 'ERROR']
# Memory usage: ~41,880 bytes for 10K items

# Generator comprehension - lazy evaluation
error_generator = (log['id'] for log in logs if log['level'] == 'ERROR')  
# Memory usage: ~200 bytes regardless of dataset size
# 99% memory savings!
```

#### 1. Streaming Data Processing
```python
def process_error_logs_streaming(log_generator, batch_size=1000):
    # Generator for error logs only
    error_logs = (log for log in log_generator if log['level'] == 'ERROR')
    
    batch_count = 0
    while True:
        # Get next batch using islice
        batch = list(islice(error_logs, batch_size))
        if not batch:
            break
            
        # Process batch without loading all data into memory
        batch_count += 1
        avg_response_time = sum(log['response_time'] for log in batch) / len(batch)
        print(f"Batch {batch_count}: {len(batch)} errors, avg response: {avg_response_time:.1f}ms")
```

#### 2. Memory-Efficient Aggregation
```python
def calculate_service_metrics_streaming(log_generator):
    # Generator for successful requests only
    successful_requests = (
        log for log in log_generator 
        if log['status_code'] == 200
    )
    
    # Process without storing all data
    service_stats = {}
    for log in successful_requests:
        service = log['service']
        if service not in service_stats:
            service_stats[service] = {'count': 0, 'total_response_time': 0}
        
        service_stats[service]['count'] += 1
        service_stats[service]['total_response_time'] += log['response_time']
    
    return service_stats
```

#### 3. Pipeline Processing with Chained Generators
```python
def create_processing_pipeline(log_source):
    # Stage 1: Filter for recent logs
    recent_logs = (
        log for log in log_source
        if (datetime.now() - log['timestamp']).seconds < 3600
    )
    
    # Stage 2: Filter for high-impact events
    high_impact = (
        log for log in recent_logs
        if log['level'] in ['ERROR', 'WARNING'] or log['response_time'] > 2000
    )
    
    # Stage 3: Transform for alerting system
    alerts = (
        {
            'alert_id': f"ALERT_{log['id']}",
            'severity': 'HIGH' if log['level'] == 'ERROR' else 'MEDIUM',
            'service': log['service'],
            'message': f"{log['level']} in {log['service']}: {log['response_time']}ms response"
        }
        for log in high_impact
    )
    
    return alerts
```

#### 4. Real-time Dashboard with Sliding Window
```python
def create_dashboard_generator(log_source, window_size=100):
    window = []
    
    for log in log_source:
        # Maintain sliding window
        window.append(log)
        if len(window) > window_size:
            window.pop(0)
        
        # Generate metrics every window_size logs
        if len(window) == window_size:
            error_rate = len([l for l in window if l['level'] == 'ERROR']) / len(window) * 100
            avg_response = sum(l['response_time'] for l in window) / len(window)
            
            yield {
                'timestamp': datetime.now(),
                'error_rate': round(error_rate, 2),
                'avg_response_time': round(avg_response, 1),
                'total_requests': len(window)
            }
```

---

## 6. Best Practices & Performance Guide

### When to Use Each Comprehension Type

| Comprehension Type | Use When | Memory Usage | Performance | Example Use Case |
|-------------------|----------|--------------|-------------|------------------|
| **List `[]`**     | Need random access, multiple iterations | High | Fast creation | API responses, UI data |
| **Set `{}`**      | Need unique values, membership testing | Medium | Fast lookups | Deduplication, tags |
| **Dict `{k:v}`**  | Need key-value mapping, fast lookups | Medium | Fast access | Caching, indexing |
| **Generator `()`**| Large datasets, one-time iteration | Low | Lazy evaluation | Log processing, streams |

### Decision Tree

- **Small dataset (<1000 items)**: Use list comprehension - performance difference negligible
- **Large dataset (>100K items)**: Use generator comprehension - memory efficiency critical
- **Need multiple iterations**: Use list comprehension - generators are consumed after first use
- **Need unique values**: Use set comprehension - automatic deduplication
- **Need fast lookups**: Use dict comprehension - O(1) access time
- **Stream processing**: Use generator comprehension - lazy evaluation
- **API responses**: Use list/dict comprehension - JSON serializable
- **Database queries**: Use generator comprehension - process results one by one

### Common Pitfalls and Solutions

#### ❌ Pitfall 1: Generator Consumed After First Iteration
```python
# Wrong - generator is consumed
gen = (x for x in range(5))
print(list(gen))  # [0, 1, 2, 3, 4]
print(list(gen))  # [] - Empty!

# Solution - recreate generator or convert to list
def create_gen():
    return (x for x in range(5))

gen1 = create_gen()  # Fresh generator each time
gen2 = create_gen()
```

#### ❌ Pitfall 2: Complex Logic in Comprehensions
```python
# Too complex - hard to read
complex_comp = [
    x * 2 if x % 2 == 0 else x * 3 if x % 3 == 0 else x
    for x in range(20)
    if x > 5 and x < 15 and x != 10
]

# Better - extract logic to functions
def transform_number(x):
    if x % 2 == 0: return x * 2
    elif x % 3 == 0: return x * 3
    return x

def should_include(x):
    return 5 < x < 15 and x != 10

clean_comp = [transform_number(x) for x in range(20) if should_include(x)]
```

### Advanced Patterns

#### Matrix Operations
```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Flatten matrix
flattened = [item for row in matrix for item in row]

# Transpose matrix
transposed = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
```

#### Conditional Expressions
```python
# Using ternary operator in comprehensions
numbers = range(-5, 6)
processed = [abs(x) if x < 0 else x * 2 for x in numbers]
```

#### Dictionary with Zip
```python
keys = ['name', 'age', 'city']
values = ['Alice', 25, 'New York']
person = {k: v for k, v in zip(keys, values)}
```

#### Set Operations
```python
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]

unique_in_first = {x for x in list1 if x not in list2}
common_elements = {x for x in list1 if x in list2}
```

### Performance Optimization Tips

1. **Use generator comprehensions for large datasets** to save memory
2. **Avoid complex logic inside comprehensions** - extract to functions
3. **Use set comprehensions when you need unique values**
4. **Use dict comprehensions instead of** `dict([(k,v) for k,v in ...])`
5. **Consider using `map()` and `filter()`** for simple transformations
6. **Use `any()` and `all()` with generator expressions** for boolean tests
7. **Cache expensive operations outside comprehensions** when possible
8. **Use `itertools` for advanced iteration patterns**

### Real-World Project Template

```python
class DataProcessor:
    """Template for real-world data processing using comprehensions"""
    
    def __init__(self, data):
        self.data = data
    
    def filter_and_transform(self, condition_func, transform_func):
        """Generic filter and transform using generator"""
        return (transform_func(item) for item in self.data if condition_func(item))
    
    def group_by(self, key_func):
        """Group data using dictionary comprehension"""
        groups = {}
        for item in self.data:
            key = key_func(item)
            if key not in groups:
                groups[key] = []
            groups[key].append(item)
        return {k: list(v) for k, v in groups.items()}
    
    def summarize(self, key_func, value_func):
        """Summarize data using dictionary comprehension"""
        summary_data = self.group_by(key_func)
        return {
            group: {
                'count': len(items),
                'total': sum(value_func(item) for item in items),
                'avg': sum(value_func(item) for item in items) / len(items)
            }
            for group, items in summary_data.items()
        }

# Example usage
processor = DataProcessor(sales_data)
summary = processor.summarize(
    key_func=lambda x: x['region'],
    value_func=lambda x: x['amount']
)
```

---

## Summary

🎉 **You now understand all Python comprehension types**  
🚀 **You can choose the right comprehension for any scenario**  
💡 **You know performance implications and best practices**  
🔧 **You have practical patterns for real-world projects**  
⚡ **You can optimize memory usage for large datasets**

**Key Takeaways:**
- Use **list comprehensions** for small to medium datasets that need multiple iterations
- Use **set comprehensions** when you need unique values or fast membership testing
- Use **dictionary comprehensions** for creating lookup tables and transforming key-value data
- Use **generator comprehensions** for large datasets and streaming data processing
- Extract complex logic to separate functions for better readability
- Always consider memory implications when working with large datasets

This comprehensive guide provides you with the knowledge to use Python comprehensions professionally in both data analysis and web development projects.