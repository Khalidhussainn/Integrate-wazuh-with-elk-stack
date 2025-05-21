# Configuring Archive Logs in Wazuh

## Overview

Wazuh provides an Archive Index pattern functionality that offers organizations a robust method for managing extensive log data collections. This archiving system enables enterprises to store, search, and analyze historical security events while maintaining operational efficiency.

## Understanding Wazuh Archives

The Archive Index pattern leverages Elasticsearch's distributed search capabilities to deliver fast and efficient querying across large datasets. This architecture supports data compression to reduce storage requirements while optimizing search performance.

Wazuh's archive functionality serves as a historical repository for security events that have been processed but remain valuable for:
- Compliance requirements
- Security auditing
- Forensic investigations
- Historical trend analysis

When configured, the archive system stores events in compressed files with customizable organization parameters:
- Daily rotation by default
- Configurable file size limits
- Adjustable retention periods
- Structured directory organization by date

## Implementation Guide

### Step 1: Access Root Privileges
```bash
sudo -i
```

### Step 2: Navigate to Filebeat Configuration Directory
```bash
cd /etc/filebeat/
```

### Step 3: Enable Archives in Filebeat Configuration
Edit the filebeat.yml file and locate the archives section. Change the enabled parameter to true:
```bash
vim filebeat.yml
```

Look for this section and modify accordingly:
```yaml
# Archives configuration
setup.template.enabled: true
setup.template.pattern: "wazuh-archives-*"
```

### Step 4: Restart Filebeat Service
```bash
systemctl restart filebeat
```

### Step 5: Configure Wazuh Manager Settings
Navigate to the Wazuh configuration directory:
```bash
cd /var/ossec/etc/
```

### Step 6: Update the Ossec Configuration
Edit the ossec.conf file to enable full logging:
```bash
vim ossec.conf
```

Locate the global section and update the following parameters:
```xml
<ossec_config>
  <global>
    <jsonout_output>yes</jsonout_output>
    <alerts_log>yes</alerts_log>
    <logall>yes</logall>
    <logall_json>yes</logall_json>
    <email_notification>no</email_notification>
    <smtp_server>smtp.example.wazuh.com</smtp_server>
    <email_from>wazuh@example.wazuh.com</email_from>
    <email_to>recipient@example.wazuh.com</email_to>
  </global>
  <!-- Additional configuration sections -->
</ossec_config>
```

### Step 7: Restart the Wazuh Manager
```bash
systemctl restart wazuh-manager.service
```

### Step 8: Configure the Archive Index Pattern
1. Navigate to the Wazuh dashboard and select "Stack Management"
2. Select "Index Patterns" and click "Create index pattern"
3. Enter "wazuh-archives-*" as the index pattern name
4. Click "Next step"
5. Select "timestamp" (not "@timestamp") as the time field
6. Click "Create index pattern"

### Step 9: Disabling Archives (Optional)
To disable the archive functionality, change the `logall` and `logall_json` parameters in the ossec.conf file back to "no" and restart the Wazuh manager:

```xml
<logall>no</logall>
<logall_json>no</logall_json>
```

Then restart the service:
```bash
systemctl restart wazuh-manager.service
```

## Benefits of Archive Configuration

Implementing Wazuh's archive functionality provides several key advantages:

1. **Extended Data Retention**: Maintain security events beyond standard retention periods
2. **Compliance Support**: Fulfill data retention requirements mandated by regulatory frameworks
3. **Forensic Investigation**: Access historical event data for security incident analysis
4. **Resource Optimization**: Separate active monitoring from long-term storage to improve performance
5. **Scalable Storage**: Efficiently manage growing volumes of security event data

## Conclusion

By enabling archive logs in Wazuh, security teams gain access to a comprehensive historical record of security events without compromising real-time monitoring capabilities. This functionality supports advanced security operations including trend analysis, compliance reporting, and forensic investigations. The implementation process requires minimal configuration changes while providing significant benefits for security operations and governance requirements.

The archive feature represents a fundamental component of a mature security monitoring infrastructure, ensuring that valuable security event data remains accessible for as long as organizational policies require.
