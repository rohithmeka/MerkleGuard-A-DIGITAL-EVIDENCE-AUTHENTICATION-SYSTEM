# MerkleGuard
### Multi-Layer Digital Evidence Integrity & Chain-of-Custody Management System

**Secure the evidence. Detect the changes. Trace the custody.**

## Overview

Digital evidence plays a critical role in investigations, audits, and security-related workflows. However, maintaining the integrity of digital evidence throughout its lifecycle is a significant challenge.

Even a small modification to a file can compromise its integrity. Traditional file hashing can identify that a file has changed, but it does not inherently explain where the modification occurred or provide a complete record of evidence custody.

**MerkleGuard** is a Python-based digital evidence integrity and custody-management prototype designed to detect unauthorized changes, localize modified data regions, and maintain a structured record of evidence handling.

The system integrates cryptographic hashing, Merkle trees, adaptive data chunking, entropy analysis, metadata verification, and HMAC-protected records into a unified evidence verification workflow.

Rather than simply reporting that a file has changed, MerkleGuard attempts to identify the affected regions and associate failed verification events with the corresponding custody records.

## Problem Statement

Digital evidence must remain consistent and verifiable throughout its handling lifecycle.

However, several challenges can affect evidence integrity:

- Unauthorized modification of digital evidence.
- Changes to file metadata.
- Difficulty identifying the exact region of a modified file.
- Lack of structured evidence custody records.
- Difficulty comparing original and subsequently submitted evidence.
- Maintaining trustworthy verification reports.

MerkleGuard addresses these challenges through a layered integrity-verification and custody-management architecture.

## Key Features

### 1. Evidence Registration

Registers digital evidence through an initial authentication and approval workflow.

The system extracts binary file content and relevant metadata to establish an initial reference state.

### 2. Metadata Integrity Verification

Extracts and evaluates evidence metadata separately from file content.

A dedicated Metadata Root is generated to help identify metadata-level changes independently of content modifications.

### 3. Shannon Entropy Analysis

Calculates Shannon entropy over fixed 32-KB anchor regions.

Entropy analysis helps characterize data regions and supports the adaptive chunking process.

### 4. Adaptive Data Chunking

Implements adaptive chunking using three supported chunk sizes:

- 4 KB
- 16 KB
- 32 KB

This allows the system to organize evidence into regions for more granular integrity verification.

### 5. SHA-256 Cryptographic Hashing

Uses domain-separated SHA-256 hashing to generate cryptographic leaf hashes.

These hashes form the foundation of the Merkle tree structure.

### 6. Merkle Tree Construction

Constructs Merkle trees from the generated chunk hashes.

The system uses hierarchical hashing to represent the integrity state of evidence content.

This enables efficient comparison of tree structures and helps localize modified regions.

### 7. Multi-Layer Evidence Identification

Generates three levels of integrity identifiers:

| Identifier | Purpose |
|---|---|
| Metadata Root | Represents the metadata integrity state |
| Content Root | Represents the content integrity state |
| Evidence Root | Combines the evidence integrity representation |

An Evidence ID is generated to identify the registered evidence record.

### 8. Protected Evidence Storage

Maintains:

- HMAC-protected JSON records.
- A verified original evidence vault.
- Stored integrity references for subsequent verification.

This helps protect stored records against unauthorized modification, assuming the HMAC key remains secure.

### 9. Evidence Exit and Return Verification

Supports evidence verification when an item exits custody and is subsequently returned.

The returned evidence is compared against the registered reference state to identify changes.

### 10. Tamper Localization

When verification fails, Merkle tree comparison helps identify affected chunks.

Byte-level difference analysis is then used to examine modifications within the identified regions.

### 11. Chain-of-Custody Management

Maintains custody-related records associated with evidence handling and failed verification events.

The system records who held the evidence during relevant verification failures and maintains structured tamper-case information.

### 12. HMAC-Protected Verification Reports

Generates verification reports protected using HMAC.

These reports provide a structured record of integrity checks, detected modifications, and relevant custody information.

## System Architecture

MerkleGuard follows a multi-layer evidence registration and verification pipeline.

```text
             EVIDENCE REGISTRATION
                       |
                       v
            AUTHENTICATION / APPROVAL
                       |
                       v
             BINARY FILE EXTRACTION
                       |
                       v
              METADATA EXTRACTION
                       |
                       v
              SHANNON ENTROPY
                (32 KB Anchors)
                       |
                       v
              ADAPTIVE CHUNKING
                4 / 16 / 32 KB
                       |
                       v
              SHA-256 HASHING
                       |
                       v
                MERKLE TREES
                       |
              +--------+--------+
              |                 |
              v                 v
         METADATA ROOT      CONTENT ROOT
              |                 |
              +--------+--------+
                       |
                       v
                 EVIDENCE ROOT
                       |
                       v
                   EVIDENCE ID
                       |
                       v
          HMAC-PROTECTED JSON STORAGE
                       |
                       v
              VERIFIED ORIGINAL VAULT
                       |
                       v
             EXIT / RETURN VERIFICATION
                       |
                       v
               INTEGRITY COMPARISON
                       |
                +------+------+
                |             |
                v             v
           NO CHANGE      CHANGE DETECTED
                              |
                              v
                       MERKLE LOCALIZATION
                              |
                              v
                        BYTE-LEVEL DIFF
                              |
                              v
                       TAMPER CASE RECORD
                              |
                              v
                       CUSTODY REVIEW
                              |
                              v
                       HMAC-PROTECTED
                       VERIFICATION REPORT
```

## Technical Foundations

MerkleGuard integrates concepts from cryptography, data structures, information theory, and digital evidence management.

| Technique | Application |
|---|---|
| SHA-256 | Cryptographic hashing of evidence chunks |
| Merkle Trees | Hierarchical integrity verification |
| Adaptive Chunking | Dividing evidence into variable-sized regions |
| Shannon Entropy | Characterizing fixed-size anchor regions |
| HMAC | Integrity protection for stored records and reports |
| Binary File Processing | Reading and comparing digital evidence |
| Byte-Level Difference | Identifying modified bytes |
| JSON | Structured evidence and custody record storage |

## Evidence Verification Workflow

The system operates through two major phases.

### Phase 1: Evidence Registration

1. Authenticate and approve the evidence registration process.
2. Extract the binary file and associated metadata.
3. Calculate Shannon entropy over fixed 32-KB anchor regions.
4. Perform adaptive chunking using 4-KB, 16-KB, and 32-KB regions.
5. Generate domain-separated SHA-256 leaf hashes.
6. Construct the corresponding Merkle trees.
7. Generate Metadata Root and Content Root.
8. Derive the Evidence Root and Evidence ID.
9. Store protected records and preserve the verified original.

### Phase 2: Evidence Reverification

1. Process the returned evidence.
2. Recalculate metadata and content integrity references.
3. Compare the new references with the original registered state.
4. Detect integrity mismatches.
5. Use Merkle tree comparison to localize affected chunks.
6. Perform byte-level difference analysis.
7. Record the relevant custody information and tamper case.
8. Generate an HMAC-protected verification report.

## Security Considerations and Limitations

MerkleGuard is an integrity-verification and custody-management prototype.

It is important to distinguish between detecting changes and proving the real-world origin or responsibility for those changes.

- SHA-256 and Merkle trees help detect content modifications.
- Metadata verification helps identify changes to recorded metadata.
- HMAC protects records against modification by parties without access to the secret key.
- The original evidence vault provides a reference for subsequent comparisons.
- Custody records document recorded handling events but do not independently prove physical possession or identify the person responsible for tampering.
- Entropy analysis supports data characterization and chunking; it is not itself proof of tampering.
- HMAC is a symmetric authentication mechanism, not a public-key digital signature.

The security of the system also depends on appropriate protection of cryptographic keys and the original evidence vault.

MerkleGuard should therefore be understood as a prototype demonstrating digital evidence integrity verification rather than a certified forensic chain-of-custody platform.

## Project Significance

MerkleGuard explores how fundamental data structures and cryptographic techniques can be applied to a practical digital security problem.

The project brings together:

- Hierarchical data structures through Merkle trees.
- Cryptographic techniques through SHA-256 and HMAC.
- Information theory through Shannon entropy.
- Adaptive data processing through variable-sized chunking.
- Binary-level analysis through byte-difference detection.
- Structured record management through JSON.

The central objective is to move beyond simple file-integrity checking toward a more detailed system capable of identifying affected regions and maintaining structured evidence-handling records.

## Future Scope

Potential enhancements include:

- Integration with a trusted timestamping mechanism.
- Append-only and externally anchored custody logs.
- Secure cryptographic key management.
- Digital signatures using asymmetric cryptography.
- Support for additional evidence file formats.
- Advanced forensic comparison and visualization.
- Independent validation against established forensic test datasets.
- A graphical dashboard for evidence investigators.

## Author

**Rohith Meka**

B.Tech – Artificial Intelligence & Data Science

GitHub: [@rohithmeka](https://github.com/rohithmeka)

---

**MerkleGuard — Integrity is not just about detecting change. It is about understanding where change occurred and maintaining a record of evidence handling.**
