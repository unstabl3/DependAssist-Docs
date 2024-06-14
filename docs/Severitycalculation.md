# Severity Calculation

DependAssist uses a comprehensive severity calculation matrix to determine the priority of issues based on various factors. This page explains how the severity is calculated and what each matrix component means.

## Components of Severity Calculation

The severity of an issue is determined based on the following components:

1. **CVSS Score**
2. **EPSS Score**
3. **KEV Status**

### 1. CVSS Score

The Common Vulnerability Scoring System (CVSS) score is a numerical representation of the severity of a vulnerability. It ranges from 0.0 to 10.0 and is categorized as follows:

- **Low**: 0.1 - 3.9
- **Medium**: 4.0 - 6.9
- **High**: 7.0 - 8.9
- **Critical**: 9.0 - 10.0

### 2. EPSS Score

The Exploit Prediction Scoring System (EPSS) score predicts the likelihood of a vulnerability being exploited. It ranges from 0.0 to 1.0, with higher values indicating a greater probability of exploitation.

### 3. KEV Status

The Known Exploited Vulnerabilities (KEV) status indicates whether a vulnerability is known to be actively exploited. It is a binary value:

- **True**: The vulnerability is in the KEV database.
- **False**: The vulnerability is not in the KEV database.

## Severity Calculation Matrix

DependAssist uses a matrix to determine the final internal priority based on the CVSS score, EPSS score, and KEV status. The matrix is as follows:

### Matrix Details

| CVSS Severity     | EPSS Probability Score    | Is in KEV Database? | Final Internal Priority |
|-------------------|---------------------------|---------------------|-------------------------|
| Low               | ANY                       | ANY                 | P4                      |
| Medium            | < 0.088                   | No                  | P4                      |
| Medium            | >= 0.088                  | No                  | P3                      |
| Medium            | ANY                       | Yes                 | P3                      |
| High              | < 0.088                   | No                  | P4                      |
| High              | >= 0.088                  | No                  | P3                      |
| High              | ANY                       | Yes                 | P3                      |
| Critical          | < 0.088                   | No                  | P3                      |
| Critical          | >= 0.088                  | No                  | P2                      |
| Critical          | ANY                       | Yes                 | P2                      |

### Detailed Explanation

1. **Low CVSS Severity**:
     - Regardless of the EPSS score and KEV status, issues with a low CVSS severity are assigned a priority of **P4**.

2. **Medium CVSS Severity**:
     - If the EPSS score is less than 0.088 and the issue is not in the KEV database, it is assigned a priority of **P4**.
     - If the EPSS score is 0.088 or above and the issue is not in the KEV database, it is assigned a priority of **P3**.
     - If the issue is in the KEV database (regardless of EPSS score), it is assigned a priority of **P3**.

3. **High CVSS Severity**:
     - If the EPSS score is less than 0.088 and the issue is not in the KEV database, it is assigned a priority of **P4**.
     - If the EPSS score is 0.088 or above and the issue is not in the KEV database, it is assigned a priority of **P3**.
     - If the issue is in the KEV database (regardless of EPSS score), it is assigned a priority of **P3**.

4. **Critical CVSS Severity**:
     - If the EPSS score is less than 0.088 and the issue is not in the KEV database, it is assigned a priority of **P3**.
     - If the EPSS score is 0.088 or above and the issue is not in the KEV database, it is assigned a priority of **P2**.
     - If the issue is in the KEV database (regardless of EPSS score), it is assigned a priority of **P2**.

### Special Case

- If the CVSS score is `None`, the issue is assigned a priority of **P4**.

## Example Calculation

Here is an example of how the severity calculation works:

1. **Vulnerability A**:
     - **CVSS Score**: 7.5 (High)
     - **EPSS Score**: 0.95
     - **KEV Status**: True

   **Calculation**:
     - High CVSS severity.
     - EPSS score is 0.95 (>= 0.088).
     - KEV status is True.

   **Final Priority**: **P3**

2. **Vulnerability B**:
     - **CVSS Score**: 9.8 (Critical)
     - **EPSS Score**: 0.05
     - **KEV Status**: False

   **Calculation**:
     - Critical CVSS severity.
     - EPSS score is 0.05 (< 0.088).
     - KEV status is False.

   **Final Priority**: **P3**

3. **Vulnerability C**:
     - **CVSS Score**: None
     - **EPSS Score**: 0.10
     - **KEV Status**: False

   **Calculation**:
     - CVSS score is None.

   **Final Priority**: **P4**

By using this matrix, DependAssist ensures that vulnerabilities are prioritized accurately based on their potential impact and the likelihood of exploitation.
