# Performance Engine

A Python-based performance tracking and metrics calculation system for Virtu Educate.

## Overview

This performance engine tracks and analyzes user performance across different difficulty levels in educational contexts. It aggregates metrics, calculates statistics, and provides insights into user proficiency based on question attempts, correct answers, and completion status.

## Core Features

- **Difficulty-based Metrics**: Track performance across Easy, Medium, and Hard question difficulties
- **Completion Status Tracking**: Determine whether users have completed each difficulty level
- **Performance Statistics**: Calculate metrics like average attempts per difficulty
- **Extensible Metrics System**: Add custom metrics through a flexible registration system

## Structure

```
performance_engine/
├── __init__.py
├── data_types.py                  # Core data type definitions
├── performance_engine.py          # Main performance statistics engine
├── metrics/
│   ├── __init__.py
│   ├── metric_types.py            # Metric types definitions
│   └── metrics_aggregator.py      # Aggregation of metrics
```

## Data Types

The system uses several key data types:

- `DifficultyEnum`: Represents question difficulty (Easy, Medium, Hard)
- `CompletionStatusEnum`: Tracks completion status (Complete, Incomplete)
- `PerformanceStatsData`: Core data class for performance statistics

## Metrics

The engine calculates several metrics:

1. **Difficulty Score**: Number of correct answers per difficulty level
2. **Difficulty Completion**: Completion status for each difficulty level
3. **Difficulty Ranking**: Difficulties ranked by average attempts

## Usage

```python
from ai_core.performance.performance_engine import PerformanceStats
from ai_core.learning_mode_rules import LearningModeType

# Create a performance stats instance
performance_stats = PerformanceStats.create_performance_stats(
    user_id=123,
    sub_topic_id=456,
    learning_mode=LearningModeType.STANDARD
)

# Get performance statistics
stats = performance_stats()

# Access stats properties
print(f"Passed difficulties: {stats.passed_difficulties}")
print(f"Failed difficulties: {stats.failed_difficulties}")
print(f"Difficulty scores: {stats.difficulty_scores}")
```

## Extending with Custom Metrics

You can create custom metrics by:

1. Implementing a new metric class that inherits from `BaseMetric`
2. Updating the `PerformanceStatsData` class to include the new metric
3. Registering the metric with the `MetricsCalculator`

Example:

```python
from ai_core.performance.metrics.metric_types import BaseMetric

class MyCustomMetric(BaseMetric):
    @property
    def name(self) -> str:
        return "my_custom_metric"
        
    def __call__(self, difficulty_groups, *args, **kwargs):
        # Implement metric calculation
        return calculated_value
        
# Register the metric
calculator = MetricsCalculator(required_correct_questions=2)
calculator.register_metrics([MyCustomMetric()])
```

## Error Handling

The system includes custom exceptions:

- `UserAttemptsNotFoundError`: Raised when no attempts are found for a user on a topic
- `DatabaseQueryError`: Raised when database queries fail

## Dependencies

- pandas: For data manipulation and analysis
- logging: For system logging
- dataclasses: For structured data classes

## TODO 

- add tests
- Create a working version that does depend on Django ORM so that people can try it out without the dependecies 

## Requirements

- Python 3.9+
- pandas
- Django ORM (for database interaction)
