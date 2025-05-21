# Enabling Archive Logs in Wazuh

*Published on May 15, 2024*

## Overview

Wazuh uses the **Archive Index pattern** to index and store logs, assisting organizations in managing large volumes of log data efficiently. This scalable and customizable method leverages **Elasticsearch**, a distributed search engine, to provide fast and effective searching of large datasets. The Archive Index pattern supports data compression, reducing storage costs and improving search efficiency.

### What is Wazuh Archive?

The Wazuh archive feature enables automatic storage of security events in compressed archive files for future analysis. It is ideal for storing events that are no longer relevant for real-time monitoring but are valuable for auditing, compliance, or forensic purposes. Key features include:

- **Storage**: Events are stored in compressed files in a configurable location and format.
- **Organization**: Archive files are organized by date, with one file per day.
- **Configuration Options**: 
  - Maximum size of each archive file.
  - Frequency of archive file creation.
  - Retention period for archived data.

## Implementation Steps

Follow these steps to enable archive logs in Wazuh:

### Step 1: Switch to Root User
```bash
sudo -i
