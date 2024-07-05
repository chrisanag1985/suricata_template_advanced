# suricata_template_advanced

It needs Suricata v7.x otherwise many of the statistics like memcap pressure don't exist in previous versions.

## Advanced Zabbix Template for Suricata

This Zabbix Template gives you an excellent insight into the Suricata internals.
It helps you to detect performance issues.

It collects the logs from the Statistics JSON file and sends them (Zabbix Active) to the Zabbix Server.

> It is an ongoing process!!!

## Contains

- Global metrics for Suricata
- Metrics per Capture Thread for Suricata
- Graphs
- Dashboard
- Triggers


## Deployment
1. Import the template to Zabbix

2. Copy the files to the `/usr/local/bin/` (or anywhere in the `$PATH`)

- `convert2zabbix` 
- `convert_threads_2zabbix` 

then give execute permissions (`chmod +x`) at the 2 files


3. Add to `suricata.yaml`

```
  - eve-log:
      enabled: yes
      filetype: regular
      filename: stats_t.json
      types:

        - stats:
            totals: yes       # stats for all threads merged together
            threads: yes       # per thread stats
            deltas: no        # include delta values

```

> The filename you enter above must also be set to the Zabbix `Suricata-Template` UserMacro `{$LOG_FILENAME}` ( Full Path needed )

4. Add to /etc/zabbix/zabbix_agent2.d/UserParameters.conf the following lines

```
UserParameter=suricata.iface-list,suricatasc -c "iface-list" /var/run/suricata/suricata-command.socket | convert2zabbix
UserParameter=suricata.discovery.threads[*],tail -1 $1 | convert_threads_2zabbix
```

Explanation:

1st line: Detects Suricata Capturing Interfaces with LLD 

2nd line: Detects Workers with LLD 


5. Restart the `zabbix-agent2.service` and `suricata.service`

6. After the Suricata appends `at least one line` at the statistics file, run the 2 Discovery Rules for the host that the Suricata template is linked to ( `Suricata Interface Discovery`, `Suricata Thread Discovery` )
