# AWS Security Specialty

## Section 1: Ultra-condensed Cheat Sheet

### Detection Stack - Know cold

- CloudTrail = API audit trail

- GuardDuty = threat detection (ML + logs)

- Security Hub = Finding aggregator + score

- Detective = investigation graphs

- Config = compliance + drift timeline

- VPC Flow Logs = network traffic evidence

### Logging Pipelines

- CloudTrail -> EventBridge -> Lambda

- GuardDuty -> Security Hub -> ticket/remediation

- Config -> Noncompliant -> auto-remediation

### Incident Response Core Sequence

- Isolate -> Snapshot -> Rotate -> Replace -> Review

- Compromised EC2:
    - detach IAM role
    - quarentine SG (Security group)
    - Snapshot EBS
    - analyze copy
    - rebuild

- Compromised keys:
    - disable key
    - rotate
    - CloudTrail lookup

- Encryption In Transit Enforcement
    - ALB listner redirect HTTP -> HTTPS
    - S3 bucket policy deny aws:SecureTransport=false
    - API Gateway TLS only
    - RDS require SSL
    - CloudFront HTTPS only