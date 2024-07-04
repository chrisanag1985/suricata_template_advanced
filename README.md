# suricata_template_advanced
Advanced Zabbix Template for Suricata


Add to suricata.yaml

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
