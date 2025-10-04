---
layout: post
title: "Feature Engineering Techniques That Actually Work in Production"
date: 2024-12-10
category: "Machine Learning"
tags: [feature-engineering, machine-learning, preprocessing, python]
description: "Practical feature engineering techniques I've used successfully in production environments, with code examples and real-world applications."
author: "Chirag Bansal"
---

# Feature Engineering Techniques That Actually Work in Production

Feature engineering is often the difference between a good model and a great one. Through my experience building production ML systems, I've discovered that the most impactful features come from understanding both the data and the business domain deeply.

## Why Feature Engineering Matters

> "Coming up with features is difficult, time-consuming, requires expert knowledge. Applied machine learning is basically feature engineering." - Andrew Ng

In my experience, feature engineering can improve model performance by 20-30% or more, often outweighing the benefits of trying different algorithms.

## 1. Time-Based Features

When working with temporal data, extracting meaningful time features is crucial:

```python
def create_time_features(df, date_column):
    """Extract comprehensive time-based features"""
    df[date_column] = pd.to_datetime(df[date_column])
    
    # Basic time components
    df['year'] = df[date_column].dt.year
    df['month'] = df[date_column].dt.month
    df['day'] = df[date_column].dt.day
    df['dayofweek'] = df[date_column].dt.dayofweek
    df['hour'] = df[date_column].dt.hour
    
    # Cyclical features (important for capturing periodicity)
    df['month_sin'] = np.sin(2 * np.pi * df['month'] / 12)
    df['month_cos'] = np.cos(2 * np.pi * df['month'] / 12)
    df['hour_sin'] = np.sin(2 * np.pi * df['hour'] / 24)
    df['hour_cos'] = np.cos(2 * np.pi * df['hour'] / 24)
    
    # Business-relevant features
    df['is_weekend'] = df['dayofweek'].isin([5, 6]).astype(int)
    df['is_holiday'] = df[date_column].dt.date.isin(holidays).astype(int)
    
    return df
```

**Real-world impact**: In a retail sales prediction project, adding cyclical features improved model accuracy by 15%.

## 2. Aggregation Features

Creating summary statistics at different granularities:

```python
def create_aggregation_features(df, group_cols, target_col, window_sizes=[7, 30, 90]):
    """Create rolling and lag features"""
    for window in window_sizes:
        # Rolling statistics
        df[f'{target_col}_rolling_mean_{window}d'] = (
            df.groupby(group_cols)[target_col]
            .rolling(window=window, min_periods=1)
            .mean()
            .reset_index(level=group_cols, drop=True)
        )
        
        df[f'{target_col}_rolling_std_{window}d'] = (
            df.groupby(group_cols)[target_col]
            .rolling(window=window, min_periods=1)
            .std()
            .reset_index(level=group_cols, drop=True)
        )
        
        # Lag features
        df[f'{target_col}_lag_{window}d'] = (
            df.groupby(group_cols)[target_col]
            .shift(window)
        )
    
    return df
```

## 3. Interaction Features

Combining features to capture relationships:

```python
def create_interaction_features(df, numeric_cols):
    """Create polynomial and interaction features"""
    from sklearn.preprocessing import PolynomialFeatures
    
    # Select most important features to avoid explosion
    important_features = ['feature1', 'feature2', 'feature3']
    
    poly = PolynomialFeatures(degree=2, interaction_only=True, include_bias=False)
    poly_features = poly.fit_transform(df[important_features])
    
    feature_names = poly.get_feature_names_out(important_features)
    poly_df = pd.DataFrame(poly_features, columns=feature_names, index=df.index)
    
    return pd.concat([df, poly_df.iloc[:, len(important_features):]], axis=1)
```

## 4. Target Encoding (with Proper Validation)

Converting categorical variables using target statistics:

```python
def safe_target_encoding(df, categorical_col, target_col, smoothing=1.0):
    """Target encoding with smoothing to prevent overfitting"""
    
    # Calculate global mean
    global_mean = df[target_col].mean()
    
    # Calculate category statistics
    category_stats = df.groupby(categorical_col)[target_col].agg(['count', 'mean'])
    category_stats['smoothed_mean'] = (
        (category_stats['mean'] * category_stats['count'] + global_mean * smoothing) /
        (category_stats['count'] + smoothing)
    )
    
    # Map back to dataframe
    df[f'{categorical_col}_target_encoded'] = (
        df[categorical_col].map(category_stats['smoothed_mean'])
        .fillna(global_mean)
    )
    
    return df
```

**Important**: Always use cross-validation or holdout encoding to prevent data leakage!

## 5. Domain-Specific Features

These are often the most impactful. Examples from different domains:

### E-commerce
```python
def create_ecommerce_features(df):
    """E-commerce specific features"""
    # Customer behavior
    df['avg_time_between_orders'] = df.groupby('customer_id')['order_date'].diff().dt.days.fillna(0)
    df['customer_lifetime_value'] = df.groupby('customer_id')['order_value'].transform('sum')
    
    # Product features
    df['discount_percentage'] = (df['original_price'] - df['sale_price']) / df['original_price']
    df['price_vs_category_avg'] = df['sale_price'] / df.groupby('category')['sale_price'].transform('mean')
    
    return df
```

### Financial Services
```python
def create_financial_features(df):
    """Financial domain features"""
    # Risk indicators
    df['debt_to_income_ratio'] = df['total_debt'] / df['monthly_income']
    df['credit_utilization'] = df['credit_used'] / df['credit_limit']
    
    # Transaction patterns
    df['avg_transaction_amount'] = df.groupby('customer_id')['transaction_amount'].transform('mean')
    df['transaction_frequency'] = df.groupby('customer_id').cumcount() + 1
    
    return df
```

## 6. Text Features (Beyond Bag-of-Words)

When working with text data:

```python
def create_text_features(df, text_column):
    """Extract meaningful text features"""
    # Basic statistics
    df[f'{text_column}_length'] = df[text_column].str.len()
    df[f'{text_column}_word_count'] = df[text_column].str.split().str.len()
    df[f'{text_column}_unique_word_count'] = df[text_column].apply(lambda x: len(set(x.split())))
    
    # Sentiment and readability
    from textstat import flesch_reading_ease
    df[f'{text_column}_readability'] = df[text_column].apply(flesch_reading_ease)
    
    # Question marks, exclamation points (useful for customer service)
    df[f'{text_column}_question_marks'] = df[text_column].str.count('\?')
    df[f'{text_column}_exclamation_marks'] = df[text_column].str.count('!')
    
    return df
```

## Feature Engineering Best Practices

### 1. Start with Domain Knowledge
- Understand the business problem deeply
- Talk to domain experts
- Research existing literature in your field

### 2. Feature Selection is Crucial
```python
from sklearn.feature_selection import SelectKBest, f_classif

def select_best_features(X, y, k=50):
    """Select top k features using statistical tests"""
    selector = SelectKBest(score_func=f_classif, k=k)
    X_selected = selector.fit_transform(X, y)
    selected_features = X.columns[selector.get_support()]
    return X_selected, selected_features
```

### 3. Validate Features Properly
- Use time-based splits for temporal data
- Ensure no data leakage
- Test feature importance and stability

### 4. Document Everything
```python
def document_feature(feature_name, description, creation_logic, business_rationale):
    """Template for feature documentation"""
    return {
        'name': feature_name,
        'description': description,
        'creation_logic': creation_logic,
        'business_rationale': business_rationale,
        'created_date': datetime.now(),
        'created_by': 'Chirag Bansal'
    }
```

## Common Pitfalls to Avoid

1. **Data Leakage**: Using future information to predict the past
2. **Overfitting**: Creating too many features without proper validation
3. **Ignoring Business Logic**: Creating features that don't make business sense
4. **Not Handling Missing Values**: Imputation strategy affects feature quality
5. **Forgetting Feature Scaling**: Different scales can hurt model performance

## Measuring Feature Impact

```python
def measure_feature_impact(X_base, X_with_new_feature, y, cv=5):
    """Measure the impact of adding new features"""
    from sklearn.model_selection import cross_val_score
    from sklearn.ensemble import RandomForestClassifier
    
    model = RandomForestClassifier(random_state=42)
    
    # Baseline performance
    baseline_scores = cross_val_score(model, X_base, y, cv=cv, scoring='accuracy')
    
    # Performance with new feature
    new_scores = cross_val_score(model, X_with_new_feature, y, cv=cv, scoring='accuracy')
    
    improvement = new_scores.mean() - baseline_scores.mean()
    
    print(f"Baseline accuracy: {baseline_scores.mean():.4f} (+/- {baseline_scores.std() * 2:.4f})")
    print(f"New accuracy: {new_scores.mean():.4f} (+/- {new_scores.std() * 2:.4f})")
    print(f"Improvement: {improvement:.4f}")
    
    return improvement
```

## Conclusion

Effective feature engineering is both an art and a science. It requires:
- Deep understanding of the data and domain
- Systematic approach to creation and validation
- Continuous iteration and improvement
- Proper documentation and maintenance

The features that make the biggest impact are often simple, interpretable, and grounded in business logic. Focus on understanding your data deeply rather than creating complex transformations blindly.

---

*What feature engineering techniques have worked best for you? I'd love to hear about your experiences and challenges in the comments!*