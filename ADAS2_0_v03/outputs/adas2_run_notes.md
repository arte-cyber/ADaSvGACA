# ADaS 2.0 v0.3 Run Notes

This run integrates author2's original seed lab files with red team Demo 1 and Demo 2 artifacts.
The key implementation update is that VM2 auth-log processing is retained. When `vm2log.txt` is present, ADaS 2.0 directly uses it for privilege-escalation and lateral-movement evidence.

## Dataset coverage
- redteam_Demo1
- redteam_Demo2
- Run1
- Run10
- Run2
- Run3
- Run4
- Run5
- Run6
- Run7
- Run8
- Run9
- author1_seed

## Stage counts
### redteam_Demo1
- Reconnaissance: 3
- Initial Access: 3
- Privilege Escalation: 2
- Lateral Movement: 2
- Rapid Follow-on Actions: 4
### redteam_Demo2
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 4
- Lateral Movement: 4
- Rapid Follow-on Actions: 6
### Run1
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### Run10
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 5
### Run2
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### Run3
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### Run4
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### Run5
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### Run6
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### Run7
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 5
### Run8
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 5
### Run9
- Reconnaissance: 5
- Initial Access: 5
- Privilege Escalation: 5
- Lateral Movement: 5
- Rapid Follow-on Actions: 6
### author1_seed
- Reconnaissance: 1
- Initial Access: 1
- Privilege Escalation: 1
- Lateral Movement: 1
- Rapid Follow-on Actions: 1

## Remaining work
- Run unmodified ADaS baseline on the same `conn_features.csv` inputs to fill the original ADaS comparison column.
- If red team can locate VM2 logs from the final run, place them as `vm2log.txt` beside `vm1log.txt`, `vm3log.txt`, and `capture.pcap` and rerun this pipeline.
- Keep the multi-classifier / deterministic-front-end idea as future work unless the team has time after baseline comparison.