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

- Secrets vs Keys
    - Secrets Manager = rotation + credentials
    - Parameter Store = config values
    - KMS = key lifecycle + grants

- High-frequency Exam Traps
    - RDS encryption cannot be enabled after creation
    - Missing org CloudTail
    - Logs not encrypted
    - Wrong use of IAM vs. key policy vs. grant

### Section 2: Detection and Logging Deep Recall Sheet

- Know difference precisely:
    - **AWS CloudTrail**
        - records API calls
        - management vs. data events
        - org tails aggregate accounts
    - **AWS GuardDuty**
        - anomaly + threat detection
        - no agents
        - uses Flow + DNS + Cloudtrail
    - **AWS Security Hub**
        - aggregates findings
        - cross-account dashboards
        - compliance score
    - **AWS Detective**
        - post-finding investigation
        - entity relationship graphs
    - **AWS config**
        - configuration history
        - drift detection
        - rule evaluation
    - **Common troublshooting causes**
        - trail not multi-region
        - log bucket policy wrong
        - KMS deny
        - org trail not enabled
        - data events not turned on

### Section 3: Incident Response Scenario Drills

- Scenario | Compromised EC2
    1. Detach IAM role
    2. Apply quarantine security group
    3. Snapshot volumes
    4. Do NOT terminate yet
    5. Analyze snapshot copy
    6. Rebuild from clean AMI

- Scanrio | Key Exfiltration
    1. Disable key immediately
    2. Rotate credentials
    3. Review CloudTrail usage
    4. Add SCP deny is needed
    5. Reissue least-privilege creds

- Scenario | Data Exfiltration Alert
    1. Check GuardDuty finding
    2. Review VPC Flow Logs
    3. Block egress SG/NACL
    4. Preserve logs
    5. Snapshot systems

- Scenario | Configuration Drift
    1. Config rule triggers
    2. Auto-remediate with Lambda
    3. Record timeline
    4. Prevent with SCP or pipeline guardrail

### Section 4: Data Protection Rapid Review Encryption in Transit

- Enforce TLS at:
    - ALB
    - CloudFront
    - API Gateway
    - S3 policy
    - RDS parameter group

- Encryption at Rest Rules
    - EBS encrypt by default
    - S3 SSE-S3 (Server-side encryption) vs. SSE-KMS (Server-side encryption--Key-management Service)
    - RDS must be encrypted at creation
    - Snapshot copy to encrypt later

- Secrets Handling
    - Secrets Manager:
        - Rotation
        - DB (Database) credentials
        - Lambda rotation support
    
    - Parameter Store:
        - Config values
        - Optional encryption
        - No rotation
    
    - KMS (Key Management Service):
        - envelope encryption
        - grants for cross-account
        - key policy **DOES NOT EQUAL** IAM policy

### Section 5: Practice Question Workbook Prompts

**Directions**
- Write answers on paper.
- For each answer write:
    - Service
    - Correct Tool
    - Why others are wrong
    - Security principle

**End of directions**

#### Detection

- Q1: Which service detects anomalous API behavior?
- Q2: Which log shows S3 object access?
- Q3: Which service aggregates findings?

#### Incident Response

- Q1: First step after EC2 compromise?
- Q2: First step after exposed key?
- Q3: How to isolate workload?

#### Data Protection

- Q1: How to force TLS on S3?
- Q2: Secrets Manager vs Parameter Store?
- Q3: When to use KMS grants?
     

