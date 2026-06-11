# Case Study: Revising a System Model During Investigation of a Distributed System Failure

## Context

A production issue was reported by the deployment team.

Subscriber identification information was expected to be propagated through several interacting services and appear in exported records used by downstream systems.

Although traffic processing appeared to function correctly, subscriber identification data was missing from the final output.

The issue occurred only in a specific deployment scenario involving multiple interacting components.

The objective of the investigation was to determine where information was being lost and why the observed behavior contradicted the expected system model.

---

## Initial System Model

At the beginning of the investigation, the following assumptions were made:

1. Subscriber identification information was generated correctly.
2. Required services were operational.
3. Data was transferred successfully between components.
4. Traffic processing mechanisms consumed identification data during processing.
5. Successful processing should result in correctly populated output records.

Under this model, normal service operation should have produced the expected business outcome.

---

## Contradiction

Testing produced results that contradicted the original assumptions.

The following observations were made:

* all relevant services started successfully;
* no critical errors were reported in logs;
* processing pipelines appeared operational;
* output records were generated;
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

* Where is the information created?
* How is it transferred?
* Which component consumes it?
* At what point can it be lost or become unavailable?

### Dependency Analysis

Attention was then focused on relationships between services rather than individual service health.

The investigation revealed that successful processing depended on a startup sequence that was not represented in the original model.

---

## Revised System Model

The analysis demonstrated that system behavior depended not only on configuration correctness but also on service initialization order.

A previously undocumented dependency influenced information availability during processing.

As a result, infrastructure health alone was insufficient to guarantee correct business behavior.

The original model was therefore revised to include service dependency relationships and initialization constraints.

---

## Root Cause

The issue was ultimately traced to an undocumented dependency between interacting services.

The dependency affected information availability during processing and was not enforced by the deployment configuration.

As a result, all components appeared operational while the expected business outcome could not be achieved.

---

## Impact on Testing Strategy

This investigation changed the approach used for similar features.

Previously, testing focused primarily on:

* configuration validation;
* service health verification;
* output validation.

Following the investigation, additional activities became mandatory:

1. Explicit validation of architectural assumptions.
2. Dependency analysis between interacting components.
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
