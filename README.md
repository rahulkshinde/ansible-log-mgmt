# Ansible Logrotate

This repo contains an Ansible solution to automate log management for a custom systemd service, `custom-monitor.service`, running on the `sre-log-rotate` host.

The solution deploys a `logrotate` configuration:

* **Log Path:** `/var/log/custom-monitor/*.log`
* **Rotation Schedule:** Daily
* **Retention:** 7 days
* **Compression:** Enabled
* **Empty Log Handling:** Skips rotation if logs are empty (`notifempty`)
* **Post-Rotation Hook:** Reloads/restarts `custom-monitor.service` after rotation.

## Execution Steps

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/rahulkshinde/ansible-log-mgmt.git
    cd ansible-log-mgmt
    ```

2.  **Update Inventory:**
    Edit the `inventory.yml` file for any host or username changes.

3.  **Run the Playbook:**
    ```bash
    ansible-playbook -i inventory.yml logrotate.yml
    ```

## ✅ Verification

After running the playbook successfully, you can SSH into the `sre-log-rotate` host and verify the following:

1.  **Directory and Permissions:**
    ```bash
    ls -ld /var/log/custom-monitor/
    # Expected output should show drwxr-xr-x and the directory exists.
    ```

2.  **Logrotate Configuration File:**
    ```bash
    cat /etc/logrotate.d/custom-monitor
    # Check that the file exists and its content matches the requirements (daily, rotate 7, etc.)
    ```

3.  **Extra Check:**
    Run the playbook a second time. All tasks should show `ok=X`, and `changed=0`.
    ```bash
    ansible-playbook -i inventory.yml logrotate.yml
    ```
