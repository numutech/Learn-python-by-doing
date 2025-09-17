# Python Functions - Use It Like Professional

## 1. Basic Definition

A **function** is a reusable block of code that performs a specific task. In data analysis, functions help us organize code, avoid repetition, and create modular solutions for complex data processing tasks.

```python
# Basic function definition
def analyze_sales_data():
    """Function to analyze sales performance"""
    print("Analyzing sales data...")
```

Functions in data analysis are essential for:

- **Data cleaning** and preprocessing
- **Statistical calculations**
- **Data visualization**
- **Report generation**


## 2. Structure of Function

Every Python function follows a specific structure:

```python
def function_name(parameters):
    """
    Docstring: Brief description of function
    """
    # Function body
    # Code logic here
    return value  # Optional return statement

# Example from sales data analysis project
def calculate_revenue(quantity, price):
    """
    Calculate total revenue from quantity and price
    
    Args:
        quantity (int): Number of items sold
        price (float): Price per item
    
    Returns:
        float: Total revenue
    """
    revenue = quantity * price
    return revenue
```

**Key components:**

- `def` keyword to define function
- Function name (follow snake_case convention)
- Parameters in parentheses
- Docstring for documentation
- Function body (indented)
- Optional return statement


## 3. Parameters and Arguments

**Parameters** are variables in function definition, **Arguments** are actual values passed when calling the function.

```python
# Parameters: customer_data, threshold
def identify_high_value_customers(customer_data, threshold):
    """Identify customers with purchases above threshold"""
    high_value = []
    for customer, purchase_amount in customer_data.items():
        if purchase_amount >= threshold:
            high_value.append(customer)
    return high_value

# Arguments: actual data and value
customers = {'Alice': 5000, 'Bob': 3000, 'Charlie': 7500}
result = identify_high_value_customers(customers, 5000)  # Arguments
print(result)  # Output: ['Alice', 'Charlie']
```

**Types of parameters:**

- **Positional parameters**: Order matters
- **Default parameters**: Have default values

```python
def calculate_discount(price, discount_rate=0.1):
    """Calculate discounted price with default 10% discount"""
    return price * (1 - discount_rate)

# Using default parameter
discounted = calculate_discount(1000)  # Uses 10% discount
# Using custom parameter
discounted_custom = calculate_discount(1000, 0.15)  # Uses 15% discount
```


## 4. Keyword Arguments (*args and **kwargs)

### *args (Variable Positional Arguments)

```python
def calculate_total_sales(*monthly_sales):
    """Calculate total sales from multiple months"""
    total = sum(monthly_sales)
    return total

# Can pass any number of arguments
q1_total = calculate_total_sales(15000, 18000, 22000)
yearly_total = calculate_total_sales(15000, 18000, 22000, 25000, 30000, 28000)
```


### **kwargs (Variable Keyword Arguments)

```python
def generate_sales_report(**metrics):
    """Generate sales report with flexible metrics"""
    report = "Sales Report:\n"
    for metric, value in metrics.items():
        report += f"{metric.title()}: {value}\n"
    return report

# Flexible keyword arguments
report = generate_sales_report(
    revenue=150000,
    units_sold=500,
    average_order_value=300,
    customer_satisfaction=4.5
)
print(report)
```


### Combined Usage

```python
def advanced_sales_analysis(region, *months, **options):
    """Advanced analysis with positional, *args, and **kwargs"""
    print(f"Analyzing {region} region")
    print(f"Months: {months}")
    
    for option, value in options.items():
        print(f"{option}: {value}")

advanced_sales_analysis("North", "Jan", "Feb", "Mar", 
                        include_forecast=True, 
                        comparison_year=2024)
```


## 5. Using Different Functions Inside a Function

Functions can call other functions, creating modular and reusable code:

```python
def clean_sales_data(raw_data):
    """Clean raw sales data"""
    # Remove duplicates, handle missing values
    cleaned = [x for x in raw_data if x is not None]
    return cleaned

def calculate_statistics(data):
    """Calculate basic statistics"""
    if not data:
        return None
    
    return {
        'mean': sum(data) / len(data),
        'min': min(data),
        'max': max(data),
        'count': len(data)
    }

def comprehensive_analysis(raw_sales_data):
    """Main analysis function that uses other functions"""
    # Use cleaning function
    clean_data = clean_sales_data(raw_sales_data)
    
    # Use statistics function
    stats = calculate_statistics(clean_data)
    
    # Additional analysis
    if stats:
        stats['range'] = stats['max'] - stats['min']
    
    return stats

# Example usage
raw_data = [1200, 1500, None, 1800, 2000, 1350]
result = comprehensive_analysis(raw_data)
print(result)
```


## 6. Scope and Namespace

**Scope** determines where variables can be accessed. **Namespace** is a container holding variable names.

```python
# Global scope
global_sales_target = 100000

def regional_analysis(region_name):
    # Local scope
    local_target = global_sales_target * 0.3  # Accessing global variable
    regional_sales = 35000  # Local variable
    
    print(f"{region_name}: Target {local_target}, Actual {regional_sales}")
    
    if regional_sales >= local_target:
        return "Target Met"
    else:
        return "Below Target"

# regional_sales is not accessible here (local scope)
result = regional_analysis("West Coast")
```

**LEGB Rule**: Python searches for variables in this order:

- **L**ocal scope
- **E**nclosing scope
- **G**lobal scope
- **B**uilt-in scope


## 7. Nonlocal and Global Scope

### Global Keyword

```python
total_processed_records = 0  # Global variable

def process_customer_batch(batch_size):
    global total_processed_records  # Modify global variable
    
    # Simulate processing
    processed_in_batch = batch_size
    total_processed_records += processed_in_batch
    
    print(f"Processed {processed_in_batch} records")
    print(f"Total processed: {total_processed_records}")

process_customer_batch(100)
process_customer_batch(150)
```


### Nonlocal Keyword

```python
def create_sales_tracker():
    """Create a sales tracking function using nonlocal"""
    total_sales = 0  # Enclosing scope variable
    
    def add_sale(amount):
        nonlocal total_sales  # Access enclosing scope variable
        total_sales += amount
        return total_sales
    
    def get_total():
        return total_sales
    
    return add_sale, get_total

# Create tracker functions
track_sale, get_sales_total = create_sales_tracker()

# Use the functions
print(track_sale(1500))  # 1500
print(track_sale(2300))  # 3800
print(get_sales_total())  # 3800
```


## 8. Difference Between Print and Return

**Print** displays output, **Return** sends value back to caller:

```python
def analyze_with_print(data):
    """Function that only prints results"""
    average = sum(data) / len(data)
    print(f"Average sales: {average}")
    # No return - function returns None

def analyze_with_return(data):
    """Function that returns results"""
    average = sum(data) / len(data)
    return average

# Usage comparison
sales_data = [1200, 1500, 1800, 1350]

# Print version - cannot use result in further calculations
analyze_with_print(sales_data)  # Prints: Average sales: 1462.5
result1 = analyze_with_print(sales_data)  # result1 is None

# Return version - can use result
result2 = analyze_with_return(sales_data)  # result2 is 1462.5
bonus_calculation = result2 * 0.05  # Can use in further calculations
```

**Best Practice**: Use `return` for functions that produce values, use `print` only for displaying information to users.

## 9. Handling Multiple Returns

Functions can return multiple values using tuples:

```python
def comprehensive_sales_metrics(sales_data):
    """Return multiple sales metrics"""
    if not sales_data:
        return None, None, None, None
    
    total = sum(sales_data)
    average = total / len(sales_data)
    minimum = min(sales_data)
    maximum = max(sales_data)
    
    return total, average, minimum, maximum

# Multiple ways to handle multiple returns
sales = [1200, 1500, 1800, 1350, 2000]

# Method 1: Tuple unpacking
total, avg, min_val, max_val = comprehensive_sales_metrics(sales)
print(f"Total: {total}, Average: {avg}")

# Method 2: Receive as tuple
metrics = comprehensive_sales_metrics(sales)
print(f"All metrics: {metrics}")

# Method 3: Access individual elements
result = comprehensive_sales_metrics(sales)
print(f"Maximum sale: {result[3]}")
```

**Named tuples** for better readability:

```python
from collections import namedtuple

SalesMetrics = namedtuple('SalesMetrics', ['total', 'average', 'minimum', 'maximum'])

def enhanced_sales_metrics(sales_data):
    """Return named tuple for better readability"""
    if not sales_data:
        return None
    
    return SalesMetrics(
        total=sum(sales_data),
        average=sum(sales_data) / len(sales_data),
        minimum=min(sales_data),
        maximum=max(sales_data)
    )

# Usage with named access
metrics = enhanced_sales_metrics([1200, 1500, 1800])
print(f"Average: {metrics.average}")  # More readable than metrics[1]
```


## 10. Lambda Functions

**Lambda functions** are small, anonymous functions for simple operations:

```python
# Traditional function
def calculate_tax(amount):
    return amount * 0.1

# Lambda equivalent
tax_calculator = lambda amount: amount * 0.1

# Usage in data analysis
sales_data = [1000, 1500, 2000, 1200]

# Apply tax calculation using lambda with map()
taxes = list(map(lambda x: x * 0.1, sales_data))
print(taxes)  # [100.0, 150.0, 200.0, 120.0]

# Filter high-value sales using lambda
high_sales = list(filter(lambda x: x > 1300, sales_data))
print(high_sales)  # [1500, 2000]

# Sort customers by sales using lambda
customers_sales = [('Alice', 1500), ('Bob', 1200), ('Charlie', 1800)]
sorted_customers = sorted(customers_sales, key=lambda x: x[1], reverse=True)
print(sorted_customers)  # [('Charlie', 1800), ('Alice', 1500), ('Bob', 1200)]
```

**When to use lambda:**

- Simple, one-line functions
- With `map()`, `filter()`, `sorted()`
- Event handling and callbacks
- Functional programming patterns

**When NOT to use lambda:**

- Complex logic (use regular functions)
- Multiple statements
- When function needs documentation

***

# Practical Data Analysis Project: E-commerce Sales Dashboard

Now let's apply all these function concepts in a comprehensive data analysis project that creates an e-commerce sales dashboard using only Python functions.

```python
import random
from datetime import datetime, timedelta
from collections import namedtuple, defaultdict

# ==================== DATA GENERATION FUNCTIONS ====================

def generate_sample_data(num_records=1000):
    """Generate sample e-commerce sales data using global scope"""
    global PRODUCTS, REGIONS, CUSTOMERS
    
    # Global constants for data generation
    PRODUCTS = ['Laptop', 'Phone', 'Tablet', 'Headphones', 'Watch']
    REGIONS = ['North', 'South', 'East', 'West', 'Central']
    CUSTOMERS = [f'Customer_{i:04d}' for i in range(1, 201)]
    
    sales_data = []
    base_date = datetime(2024, 1, 1)
    
    for _ in range(num_records):
        record = {
            'date': base_date + timedelta(days=random.randint(0, 365)),
            'customer_id': random.choice(CUSTOMERS),
            'product': random.choice(PRODUCTS),
            'quantity': random.randint(1, 5),
            'unit_price': random.uniform(100, 2000),
            'region': random.choice(REGIONS)
        }
        record['total_amount'] = record['quantity'] * record['unit_price']
        sales_data.append(record)
    
    return sales_data

# ==================== DATA CLEANING FUNCTIONS ====================

def clean_sales_record(record):
    """Clean individual sales record - demonstrates function composition"""
    if not record or record['total_amount'] <= 0:
        return None
    
    # Clean customer ID
    record['customer_id'] = record['customer_id'].strip().upper()
    
    # Round monetary values
    record['unit_price'] = round(record['unit_price'], 2)
    record['total_amount'] = round(record['total_amount'], 2)
    
    return record

def clean_dataset(raw_data, *validation_functions, **cleaning_options):
    """
    Clean entire dataset using *args for validation functions and **kwargs for options
    Demonstrates: *args, **kwargs, function composition
    """
    cleaned_data = []
    
    # Default cleaning options
    remove_nulls = cleaning_options.get('remove_nulls', True)
    min_amount = cleaning_options.get('min_amount', 0)
    max_amount = cleaning_options.get('max_amount', float('inf'))
    
    for record in raw_data:
        # Apply cleaning function
        cleaned_record = clean_sales_record(record)
        
        if remove_nulls and cleaned_record is None:
            continue
            
        if cleaned_record and (cleaned_record['total_amount'] < min_amount or 
                              cleaned_record['total_amount'] > max_amount):
            continue
            
        # Apply additional validation functions
        valid = True
        for validation_func in validation_functions:
            if not validation_func(cleaned_record):
                valid = False
                break
        
        if valid:
            cleaned_data.append(cleaned_record)
    
    return cleaned_data

# Validation functions for cleaning
def validate_date(record):
    """Validation function for date range"""
    if not record:
        return False
    return record['date'].year == 2024

def validate_quantity(record):
    """Validation function for reasonable quantity"""
    if not record:
        return False
    return 1 <= record['quantity'] <= 10

# ==================== ANALYSIS FUNCTIONS WITH SCOPE EXAMPLES ====================

# Global variables for tracking
total_records_processed = 0
analysis_cache = {}

def calculate_basic_metrics(data):
    """Calculate basic sales metrics - demonstrates global scope modification"""
    global total_records_processed
    
    if not data:
        return None
    
    total_records_processed += len(data)
    
    amounts = [record['total_amount'] for record in data]
    
    return {
        'total_revenue': sum(amounts),
        'average_order_value': sum(amounts) / len(amounts),
        'min_order': min(amounts),
        'max_order': max(amounts),
        'order_count': len(amounts),
        'records_processed': total_records_processed
    }

def create_regional_analyzer():
    """
    Create regional analysis function using nonlocal scope
    Demonstrates: closures, nonlocal keyword
    """
    regional_totals = defaultdict(float)  # Enclosing scope
    
    def analyze_region(data, region):
        nonlocal regional_totals  # Access enclosing scope
        
        region_data = [r for r in data if r['region'] == region]
        region_revenue = sum(r['total_amount'] for r in region_data)
        regional_totals[region] = region_revenue
        
        return {
            'region': region,
            'revenue': region_revenue,
            'orders': len(region_data),
            'cumulative_totals': dict(regional_totals)
        }
    
    def get_regional_summary():
        return dict(regional_totals)
    
    return analyze_region, get_regional_summary

# ==================== MULTIPLE RETURN FUNCTIONS ====================

# Named tuple for customer metrics
CustomerMetrics = namedtuple('CustomerMetrics', 
                           ['customer_id', 'total_spent', 'order_count', 
                            'avg_order_value', 'favorite_product'])

def analyze_customer_behavior(data, customer_id):
    """
    Comprehensive customer analysis with multiple returns
    Demonstrates: named tuples, multiple return values
    """
    customer_data = [r for r in data if r['customer_id'] == customer_id]
    
    if not customer_data:
        return None, None, None
    
    # Calculate metrics
    total_spent = sum(r['total_amount'] for r in customer_data)
    order_count = len(customer_data)
    avg_order_value = total_spent / order_count
    
    # Find favorite product
    product_counts = defaultdict(int)
    for record in customer_data:
        product_counts[record['product']] += record['quantity']
    
    favorite_product = max(product_counts.items(), key=lambda x: x[1])[0]
    
    # Return multiple formats
    metrics_tuple = CustomerMetrics(
        customer_id=customer_id,
        total_spent=round(total_spent, 2),
        order_count=order_count,
        avg_order_value=round(avg_order_value, 2),
        favorite_product=favorite_product
    )
    
    # Return as named tuple, dict, and summary string
    metrics_dict = {
        'customer_id': customer_id,
        'total_spent': round(total_spent, 2),
        'order_count': order_count,
        'avg_order_value': round(avg_order_value, 2),
        'favorite_product': favorite_product
    }
    
    summary = f"Customer {customer_id}: ${total_spent:.2f} across {order_count} orders"
    
    return metrics_tuple, metrics_dict, summary

# ==================== LAMBDA FUNCTIONS FOR DATA PROCESSING ====================

def advanced_data_filtering(data):
    """Demonstrate lambda functions in data analysis context"""
    
    # Lambda for filtering high-value customers
    high_value_filter = lambda record: record['total_amount'] > 1000
    high_value_orders = list(filter(high_value_filter, data))
    
    # Lambda for extracting and transforming data
    revenue_by_product = defaultdict(float)
    for product in PRODUCTS:
        product_revenue = sum(map(lambda r: r['total_amount'] if r['product'] == product else 0, data))
        revenue_by_product[product] = product_revenue
    
    # Lambda for sorting products by revenue
    sorted_products = sorted(revenue_by_product.items(), 
                           key=lambda x: x[1], reverse=True)
    
    # Lambda for customer ranking
    customer_totals = defaultdict(float)
    for record in data:
        customer_totals[record['customer_id']] += record['total_amount']
    
    top_customers = sorted(customer_totals.items(), 
                          key=lambda x: x[1], reverse=True)[:10]
    
    return {
        'high_value_orders_count': len(high_value_orders),
        'product_ranking': sorted_products,
        'top_customers': top_customers,
        'high_value_percentage': len(high_value_orders) / len(data) * 100
    }

# ==================== PRINT vs RETURN DEMONSTRATION ====================

def print_sales_summary(data):
    """Function that prints summary - demonstrates print usage"""
    metrics = calculate_basic_metrics(data)
    
    print("=== SALES SUMMARY ===")
    print(f"Total Revenue: ${metrics['total_revenue']:,.2f}")
    print(f"Average Order Value: ${metrics['average_order_value']:.2f}")
    print(f"Total Orders: {metrics['order_count']:,}")
    print(f"Order Range: ${metrics['min_order']:.2f} - ${metrics['max_order']:.2f}")
    
    # This function returns None (implicit)

def generate_sales_report(data):
    """Function that returns formatted report - demonstrates return usage"""
    metrics = calculate_basic_metrics(data)
    
    report = f"""
=== SALES ANALYSIS REPORT ===
Generated on: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}

Key Metrics:
- Total Revenue: ${metrics['total_revenue']:,.2f}
- Average Order Value: ${metrics['average_order_value']:.2f}
- Total Orders: {metrics['order_count']:,}
- Order Range: ${metrics['min_order']:.2f} - ${metrics['max_order']:.2f}
- Records Processed: {metrics['records_processed']}
"""
    
    return report

# ==================== MAIN DASHBOARD FUNCTION ====================

def create_sales_dashboard(num_records=1000):
    """
    Main dashboard function that demonstrates all function concepts
    Integrates: all function types, scoping, multiple returns, lambda functions
    """
    
    print("🚀 E-COMMERCE SALES DASHBOARD")
    print("=" * 50)
    
    # 1. Generate and clean data (function composition, *args, **kwargs)
    print("\n📊 Generating sample data...")
    raw_data = generate_sample_data(num_records)
    
    print("🧹 Cleaning dataset...")
    clean_data = clean_dataset(raw_data, validate_date, validate_quantity,
                              remove_nulls=True, min_amount=50, max_amount=10000)
    
    print(f"✅ Processed {len(raw_data)} raw records → {len(clean_data)} clean records")
    
    # 2. Basic metrics (global scope demonstration)
    print("\n📈 BASIC METRICS")
    basic_metrics = calculate_basic_metrics(clean_data)
    print_sales_summary(clean_data)  # Demonstrates print function
    
    # 3. Regional analysis (nonlocal scope demonstration)
    print("\n🗺️ REGIONAL ANALYSIS")
    analyze_region, get_regional_summary = create_regional_analyzer()
    
    for region in REGIONS:
        region_result = analyze_region(clean_data, region)
        print(f"{region}: ${region_result['revenue']:,.2f} ({region_result['orders']} orders)")
    
    # 4. Customer behavior analysis (multiple returns)
    print("\n👥 TOP CUSTOMER ANALYSIS")
    sample_customers = random.sample(CUSTOMERS, 5)
    
    for customer in sample_customers:
        result = analyze_customer_behavior(clean_data, customer)
        if result[0]:  # Check if customer has data
            named_tuple, metrics_dict, summary = result
            print(f"  {summary} | Fav: {named_tuple.favorite_product}")
    
    # 5. Advanced filtering with lambda functions
    print("\n🎯 ADVANCED INSIGHTS (Lambda Functions)")
    lambda_results = advanced_data_filtering(clean_data)
    
    print(f"High-value orders (>$1000): {lambda_results['high_value_orders_count']} "
          f"({lambda_results['high_value_percentage']:.1f}%)")
    
    print("\nTop 3 Products by Revenue:")
    for i, (product, revenue) in enumerate(lambda_results['product_ranking'][:3], 1):
        print(f"  {i}. {product}: ${revenue:,.2f}")
    
    print("\nTop 3 Customers by Total Spending:")
    for i, (customer, total) in enumerate(lambda_results['top_customers'][:3], 1):
        print(f"  {i}. {customer}: ${total:,.2f}")
    
    # 6. Generate exportable report (return vs print)
    print("\n📋 GENERATING EXPORTABLE REPORT")
    report = generate_sales_report(clean_data)
    
    # Show difference between print and return
    print("Report ready for export!")
    # report could be saved to file, emailed, etc. because it's returned
    
    # 7. Global state summary
    print(f"\n📊 Global Processing Stats: {total_records_processed} total records processed")
    
    return {
        'clean_data': clean_data,
        'basic_metrics': basic_metrics,
        'regional_summary': get_regional_summary(),
        'lambda_insights': lambda_results,
        'exportable_report': report
    }

# ==================== RUN THE COMPLETE ANALYSIS ====================

if __name__ == "__main__":
    # Execute the complete data analysis dashboard
    dashboard_results = create_sales_dashboard(1500)
    
    # Demonstrate that we can use returned values for further analysis
    print(f"\n🎉 Analysis complete! Dashboard returned {len(dashboard_results)} result sets")
    print(f"Regional totals available: {list(dashboard_results['regional_summary'].keys())}")
```


## Project Summary

This comprehensive data analysis project demonstrates all Python function concepts:

### **Function Concepts Covered:**

1. **Basic Functions**: `generate_sample_data()`, `clean_sales_record()`
2. **Function Structure**: Proper docstrings, parameters, return statements
3. **Parameters \& Arguments**: Various parameter types in `clean_dataset()`
4. **\*args \& \*\*kwargs**: Flexible arguments in `clean_dataset()` and `advanced_sales_analysis()`
5. **Function Composition**: Functions calling other functions throughout
6. **Scope \& Namespace**: Global variables `total_records_processed`, `PRODUCTS`
7. **Nonlocal \& Global**: `create_regional_analyzer()` with nonlocal, global modifications
8. **Print vs Return**: `print_sales_summary()` vs `generate_sales_report()`
9. **Multiple Returns**: `analyze_customer_behavior()` returns tuple, dict, and string
10. **Lambda Functions**: Extensive use in `advanced_data_filtering()`

### **Data Analysis Features:**

- **Data Generation**: Realistic e-commerce sales data
- **Data Cleaning**: Validation and filtering functions
- **Statistical Analysis**: Revenue, averages, rankings
- **Regional Analysis**: Geographic performance tracking
- **Customer Behavior**: Individual customer metrics and preferences
- **Advanced Insights**: Lambda-powered filtering and ranking
- **Report Generation**: Exportable business reports

This project showcases how professional data analysts use Python functions to create maintainable, scalable, and efficient data processing pipelines. Each function has a specific purpose, demonstrates clean coding practices, and contributes to a comprehensive business intelligence solution.

