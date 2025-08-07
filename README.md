# AWS CDK Stack: EC2 CloudWatch Alarms

This AWS CDK stack (`CloudWatchAlarmEC2`) automatically creates CloudWatch alarms for EC2 instances based on your configuration. It supports:

- CPU utilization alarms
- Memory utilization alarms
- Disk utilization alarms
- System status check alarms
- Optional SNS topic creation and email notifications

## 📦 Features

- Tags all resources with common metadata for auditing.
- Dynamically fetches EC2 instances in the region.
- Filters instances if an `inventory` list is provided.
- Supports both Windows and Linux EC2 monitoring.
- Allows usage of an existing SNS topic or creates a new one.

---

## ⚙️ Required Configuration in `cdk.json`

Below are the keys you need to define in `cdk.json` under `"context"`.

### Required Parameters

| Key                      | Type           | Description                                                                 |
|--------------------------|----------------|-----------------------------------------------------------------------------|
| `project`                | string          | Name of your project. Used in naming alarms.                                |
| `environment`            | string          | Environment name (`dev`, `prod`, etc.). Used in naming.                     |
| `number`                 | string or int   | Just a reference value (can be used for internal tracking or naming).       |
| `thresholds`             | array of int    | CPU/Memory/Disk usage thresholds, e.g., `[70, 90]`.                         |
| `evaluation_periods`     | int             | Number of periods to evaluate alarm.                                       |
| `datapoints_to_alarm`    | int             | Number of data points that must be breaching to trigger alarm.             |
| `period_minutes`         | int             | Period duration in minutes for metric evaluation.                          |
| `cpu_namespace`          | string          | CloudWatch namespace for CPU metrics (typically `AWS/EC2`).                |
| `cpu_statistic`          | string          | Statistic to use for CPU (e.g., `Average`, `Maximum`).                     |
| `memory_namespace`       | string          | Namespace for memory metrics (`CWAgent` or custom).                        |
| `memory_statistic`       | string          | Statistic for memory (`Average`, `Maximum`, etc.).                         |
| `memory_metric_windows`  | string          | Metric name for Windows memory usage.                                      |
| `memory_metric_linux`    | string          | Metric name for Linux memory usage.                                        |
| `disk_namespace`         | string          | Namespace for disk metrics.                                               |
| `disk_statistic`         | string          | Statistic for disk (`Average`, etc.).                                      |
| `sns_topic_name`         | string          | Name for SNS topic (if created).                                           |
| `device_windows`         | string          | Windows disk device name (e.g., `C:`).                                     |
| `fstype_windows`         | string          | Windows file system type (e.g., `NTFS`).                                   |
| `device_linux`           | string          | Linux disk device (e.g., `/dev/xvda1`).                                    |
| `fstype_linux`           | string          | Linux filesystem type (e.g., `xfs`, `ext4`).                               |
| `inventory`              | array of string | Optional. List of EC2 Instance IDs to monitor. Empty = monitor all.        |
| `AlarmEmails`            | array of string | Email addresses to subscribe to SNS topic.                                 |
| `existing_topic_arn`     | string          | ARN of existing SNS topic (used if `createsns` is `"false"`).              |
| `createsns`              | string          | `"true"` to create new SNS topic; `"false"` to use existing.               |

### Optional Tags

The following keys can be added for resource tagging:

- `CreatedBy`
- `Owner`
- `OwnerEmail`
- `ProjectName`
- `Purpose`
- `Region`
- `AppName`
- `Environment`
- `TicketReference`
- `AppEnv`
- `AppOwner`
- `AppOwnerEmail`

---

## 📂 Example `cdk.json`

```json
{
  "context": {
    "project": "MyProject",
    "environment": "prod",
    "number": "001",
    "thresholds": [70, 90],
    "evaluation_periods": 2,
    "datapoints_to_alarm": 1,
    "period_minutes": 5,
    "cpu_namespace": "AWS/EC2",
    "cpu_statistic": "Average",
    "memory_namespace": "CWAgent",
    "memory_statistic": "Average",
    "memory_metric_windows": "Memory % Committed Bytes In Use",
    "memory_metric_linux": "memory_used_percent",
    "disk_namespace": "CWAgent",
    "disk_statistic": "Average",
    "sns_topic_name": "MyAlarmSNSTopic",
    "device_windows": "C:",
    "fstype_windows": "NTFS",
    "device_linux": "/dev/xvda1",
    "fstype_linux": "xfs",
    "inventory": [],
    "AlarmEmails": ["admin@example.com"],
    "existing_topic_arn": "arn:aws:sns:us-west-2:123456789012:ExistingTopic",
    "createsns": "true"
  }
}
