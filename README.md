# suricata_template_advanced

## Advanced Zabbix Template for Suricata

This Zabbix Template gives you an excellent insight into the Suricata internals.
It helps you to detect performance issues.

It collects the logs from the Statistics JSON file and sends them (Zabbix Active) to the Zabbix Server.

It is an ongoing process!!!

## Contains

- Global metrics for Suricata
- Metrics per Capture Thread for Suricata
- Graphs
- Dashboard

## TODO

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

> The filename you enter above must also be set to the Zabbix `Suricata-Template` UserMacro `{$LOG_FILENAME}`

4. Add to /etc/zabbix/zabbix_agent.d/Userparameters.conf

```
UserParameter=suricata.iface-list,suricatasc -c "iface-list" /var/run/suricata/suricata-command.socket | convert2zabbix
UserParameter=suricata.discovery.threads[*],tail -1 $1 | convert_threads_2zabbix
```


5. Restart the `zabbix-agent2` and `suricata`

6. After the Suricata appends `at least one line` at the statistics file, run the 2 Discovery Rules for the host that the template is linked  ( `Suricata Interface Discovery`, `Suricata Thread Discovery` )
