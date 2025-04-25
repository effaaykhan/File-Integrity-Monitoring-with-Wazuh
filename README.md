# File-Integrity-Monitoring-with-Wazuh
This repo contains the step-by-step guide to enable the file integrity monitoring in Windows and Linux endpoints with Wazuh

# Windows

### Windows-Agent Configuration
- Step 1: Open the ```C:\Program Files (x86)\ossec-agent\ossec.conf``` file under ```syscheck``` and add the following lines to monitor the directories or files:
  - To monitor the directories in real-time only
    ```
    <directories>realtime"yes">//PATH-TO-THE-MONITORED-DIRECTORY</directories>
    ```
  - You can add the following also to monitor the report changes, who data etc:
    ```
    <directories check_all="yes" realtime="yes" whodata="yes" report_changes="yes">\\PATH-TO-THE-MONITORED-DIRECTORIES</directories>
    ```
- Step 2: Restart Wazuh-Agent
    ```
    Restart-Service -Name WazuhSvc
    ```
### Wazuh-Server Configuration
- Step 1: Add the following rules in ```/var/ossec/etc/rules/local_rules.xml```:
    ```
    <group name="Win-syscheck,">
       <rule id="100303" level="7">
         <if_sid>550</if_sid>
         <field name="file" type="pcre2">(?i)C:\\Users.+Downloads</field>
         <description>File modified in the Downloads folder</description>
       </rule>
       <rule id="100304" level="7">
         <if_sid>554</if_sid>
         <field name="file" type="pcre2">(?i)C:\\Users.+Downloads</field>
         <description>File added in the Downloads folder.</description>
       </rule>
    </group>
    ```

# Ubuntu

### Ubuntu-Agent Configuration
- Step 1: Open the ```var/ossec/etc/ossec.conf``` file under ```syscheck``` and add the following lines to monitor the directories or files:
  ```
  <directories realtime="yes" check_all="yes" whodata="yes" report_changes="yes">/root</directories>
  ```
- Step 2: Restart Wazuh-Agent
  ```
  systemctl restart wazuh-agent
  ```
### Wazuh-Server Configuration
- Step 1: Add the following rules in ```/var/ossec/etc/rules/local_rules.xml```:
  ```
  <group name="Lin-syscheck,">
    <rule id="100300" level="7">
      <if_sid>550</if_sid>
      <field name="file">/home/</field>
      <description>File modified in /home/ directory.</description>
    </rule>
    <rule id="100301" level="7">
      <if_sid>554</if_sid>
      <field name="file">/home/</field>
      <description>File added to /home/ directory.</description>
    </rule>
  </group>
  ```

  
