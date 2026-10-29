# LAB14 - SSH Honeypot Detection & Response

## 1. Environment

EXT-CLIENT: 172.31.64.50
SSH-DECOY: 172.31.64.40
SOC-LITE: 10.60.214.10

SSH login:
- IP: 172.31.64.40
- Port: 2222
- Username: decoy
- Password: Lab14-Practice-Only!

## 2. SSH-DECOY Terminal

sudo lab14-decoyctl start
sudo lab14-policy init

## 3. EXT-CLIENT Terminal

ssh -tt -o StrictHostKeyChecking=no \
-o UserKnownHostsFile=/dev/null \
-o PreferredAuthentications=password \
-o PubkeyAuthentication=no \
-p 2222 decoy@172.31.64.40

Password: Lab14-Practice-Only!

Enter these commands ONE BY ONE:

whoami
id
uname -a
cat /etc/shadow
exit

Note: This is a simulated shell.
The real /etc/shadow file is not exposed.

## 4. SOC Terminal - Detection

sudo lab14-detect
sudo lab14-review evidence

IMPORTANT:
Record the actual SESSION and SHA256 values
from the SOC output.

## 5. SSH-DECOY - Containment

After reviewing the HIGH alert:

sudo lab14-policy contain APPROVE
sudo lab14-check-policy

## 6. EXT-CLIENT - Post-block Test

sudo lab14-postblock

Expected:
LAB14_POST_BLOCK_PASS: attacker connection dropped

## 7. SOC - Incident Report

Create the report:

sudo nano /var/log/lab14-soc/incident-report.txt

Enter:

src_ip=172.31.64.50
session=SESSION_ID
severity=high
indicators=id,cat /etc/shadow
compromise=not_proven
response=blocked
evidence_sha256=SHA256_VALUE

CRITICAL:
SESSION_ID and SHA256_VALUE are placeholders!

Replace SESSION_ID with the actual SESSION from SOC.
Replace SHA256_VALUE with the actual SHA256 from SOC.

DO NOT submit the report with these placeholders.

After saving the report:

sudo lab14-review report

Expected:
LAB14_INCIDENT_REPORT_PASS

## 8. HOST - Checkwork

checkwork lt-lab14-ssh-deception

Target: 7/7 Y.
