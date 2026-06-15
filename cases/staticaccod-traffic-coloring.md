# Case Study: Static Accounting Failure in a Traffic Coloring Environment

## Context

A production issue was reported by the deployment team.

In a specific deployment scenario, subscriber identification information was expected to be associated with intercepted network traffic and appear in exported records consumed by downstream systems.

Although traffic processing appeared to function correctly, subscriber identification data was missing from the final output.

The objective of the investigation was to determine where information was being lost and why the observed behavior contradicted the expected system model.

---

## Business Function

The investigated platform was responsible for processing network traffic and associating it with subscriber identities.

The resulting records were used for downstream analysis and reporting.

For the platform to produce meaningful results, traffic information and subscriber identification data had to be successfully combined during processing.

Loss of subscriber identification information at any stage resulted in incomplete exported records despite otherwise successful traffic processing.

---

## Feature Under Investigation

The investigated feature provided subscriber identification for users with static network addressing.

The feature relied on two cooperating mechanisms:

* generation of synthetic subscriber accounting information;
* traffic classification based on network attributes.

Subscriber accounting information was generated independently and later consumed during traffic classification and processing.

Traffic classification mechanisms relied on subscriber accounting information to associate classified traffic with subscriber identities.

The expected information flow was:

Subscriber Accounting Information
→ Traffic Classification
→ Subscriber Association
→ Exported Record

Correct operation therefore depended on successful propagation of subscriber accounting information across multiple interacting services.

---

## Initial System Model

At the beginning of the investigation, the following assumptions were made:

1. Subscriber accounting information was generated correctly.
2. Required services were operational.
3. Subscriber information was transferred successfully between components.
4. Traffic classification mechanisms consumed subscriber information during processing.
5. Successful traffic processing should result in correctly populated exported records.

Under this model, normal service operation should have produced the expected business outcome.

---

## Key Data Entity

The key information entity investigated during this analysis was subscriber identification data.

The investigation focused on:

* where this information was generated;
* how it was propagated;
* which services consumed it;
* how its availability affected downstream processing.

The failure ultimately proved to be not a traffic processing problem but an information propagation problem.

---

## Contradiction

Testing produced results that contradicted the original assumptions.

The following observations were made:

* all relevant services started successfully;
* no critical errors were reported in logs;
* traffic classification appeared operational;
* exported records were generated;
* subscriber identification data was absent from the final result.

The infrastructure appeared healthy while the business requirement was not satisfied.

This contradiction indicated that the original model was incomplete.

---

## Investigation Strategy

Instead of focusing on individual components, the investigation was organized around validating assumptions embedded in the system model.

### Documentation Analysis

The first step was reconstructing expected system behavior from available sources.

The review revealed that:

* information was distributed across multiple documents;
* component interactions were described inconsistently;
* several dependencies were not documented explicitly.

### Deployment Verification

Deployment-related causes were excluded through:

* installation verification;
* configuration validation;
* service availability checks;
* startup verification.

No deployment issues were identified.

### Data Flow Analysis

The investigation then shifted from component status to information movement.

The following questions were examined:

* Where is subscriber information created?
* How is it transferred?
* Which component consumes it?
* At what point can it become unavailable?
* How does its availability affect downstream processing?

### Dependency Analysis

Attention was then focused on relationships between services rather than individual service health.

The investigation revealed that successful processing depended on a startup sequence that was not represented in the original model.

---

## Revised System Model

The analysis demonstrated that system behavior depended not only on configuration correctness but also on service initialization order.

A previously undocumented dependency influenced the availability of subscriber accounting information during processing.

As a result, infrastructure health alone was insufficient to guarantee correct business behavior.

The original model was therefore revised to include service dependency relationships and initialization constraints.

---

## Root Cause

The issue was ultimately traced to an undocumented dependency between interacting services.

The dependency affected the availability of subscriber accounting information and was not enforced by the deployment configuration.

As a result, traffic processing and classification appeared operational while subscriber identification could not be completed successfully.

---

## Impact on Testing Strategy

This investigation changed the approach used for similar features.

Previously, testing focused primarily on:

* configuration validation;
* service health verification;
* output validation.

Following the investigation, additional activities became mandatory:

1. Explicit validation of architectural assumptions.
2. Dependency analysis between interacting services.
3. Data flow reconstruction across the system.
4. Verification of initialization constraints.
5. Continuous refinement of the system model based on observed behavior.

---

## Testing Methodology Takeaway

One of the most important lessons from this investigation was that failures in distributed systems often emerge from incorrect assumptions rather than from visibly failing components.

The investigation process evolved into a reusable methodology:

1. Build an initial system model.
2. Identify contradictions between observations and expectations.
3. Collect evidence from documentation, logs and system behavior.
4. Revise the model.
5. Adapt the testing strategy accordingly.

The goal is not merely to verify functionality but to continuously improve the accuracy of the model used to understand the system itself.
