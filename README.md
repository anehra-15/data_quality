# Ilum Data Quality Framework
This project aims at providing a high level use case of a configuration-driven data quality framework for Ilum, utilizing Deequ for scalable validation on Apache Spark. It enables dynamic rule configuration using YAML/JSON config and integrates seamlessly with Ilum’s data lakehouse.

## Main idea behind this use case
- Ilum is a data lakehouse, and ensuring availability of high-quality data before ingestion for business specific pipelines prevents bad data from polluting the system and improves the overall integrity of the data for the downstream systems.
- A config-driven framework means teams across the organization can define data quality rules without modifying any code.

## Why is this a unique approach?
- Prevents bad data from being ingested.
- Config-driven, so a low-code approach enables business teams to define rules dynamically without modifying code.
- Scalable for multiple datasets
- Leverage Deequ for built-in checks (nulls, uniqueness, max, min) etc. and the capability to write your own Custom Checks (e.g., complex regex, business rules).
- Integrate Directly with Ilum (Spark-based processing).
- Provide Actionable Alerts on data quality issues and failures.

## Flow Chart of the overall process
![alt text](img/flowchart.png)

## Steps involved in the 
- Step1: Config file
  - Define the source path of the data to be read
  - Define the Schema for the data
  - Define the rules and checks to run and the columns to run them on
  - ## Example Configuration File (JSON Format)
This framework is driven by a JSON configuration file, allowing users to define data quality rules dynamically.

```json
{
  "dataset": "customer_data",
  "source_path": "s3://ilum-data/customers/",
  "format": "parquet",
  "checks": [
    {
      "column": "customer_id",
      "rules": [
        { "type": "not_null" },
        { "type": "unique" }
      ]
    },
    {
      "column": "email",
      "rules": [
        { "type": "regex", "pattern": "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$" }
      ]
    },
    {
      "column": "zip_code",
      "rules": [
        { "type": "length", "min": 5, "max": 5 }
      ]
    }
  ],
  "thresholds": {
    "max_null_percent": 5,
    "min_completeness": 0.95
  },
  "alerts": {
    "type": "email",
    "recipients": ["data-team@company.com"]
  }
}
```


- Step2: 
