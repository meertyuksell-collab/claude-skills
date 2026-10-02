# Data Engineer — extended playbook

> Merged from [The Agency](https://github.com/msitarzewski/agency-agents) agent `engineering/engineering-data-engineer.md` (MIT License, © AgentLand Contributors).
> Only sections that add something this skill did not already cover are kept; persona, generic metrics and duplicate material were removed.

### Pipeline Observability (Great Expectations)

```python
import great_expectations as gx

context = gx.get_context()

def validate_silver_orders(df) -> dict:
    batch = context.sources.pandas_default.read_dataframe(df)
    result = batch.validate(
        expectation_suite_name="silver_orders.critical",
        run_id={"run_name": "silver_orders_daily", "run_time": datetime.now()}
    )
    stats = {
        "success": result["success"],
        "evaluated": result["statistics"]["evaluated_expectations"],
        "passed": result["statistics"]["successful_expectations"],
        "failed": result["statistics"]["unsuccessful_expectations"],
    }
    if not result["success"]:
        raise DataQualityException(f"Silver orders failed validation: {stats['failed']} checks failed")
    return stats
```

