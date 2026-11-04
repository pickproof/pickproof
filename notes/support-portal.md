# Support portal · build notes

**In development · November 2026**

Getting IT help should be easy. I’m working on a portal that brings support requests, replies, and updates into one place, with customer and administration experiences.

These notes follow the decisions behind the project: making requests understandable, keeping access appropriate, and checking that changes behave as intended.

## A look inside

![Customer dashboard with support categories](../assets/portal/dashboard.webp)

*Desktop customer dashboard. Plain-language categories help someone start without having to diagnose the problem first.*

<table>
<tr>
<td width="35%" valign="top">
<img src="../assets/portal/mobile-request.webp" alt="Mobile support request category selection" width="260" />
</td>
<td valign="top">
<h3>A smaller first step</h3>
<p>The request flow starts with a familiar question: what do you need help with? Categories lead into a guided sequence.</p>
<p>The design work includes mobile layouts, readable choices, and keeping the next action clear.</p>
</td>
</tr>
</table>

![Administration overview with synthetic queue counts](../assets/portal/admin-overview.webp)

*Administration overview showing request states and navigation.*

**About these images:** actual application renders from automated visual fixtures using synthetic identities and counts, captured November 3, 2026. They illustrate development UI; visible navigation does not establish that every feature is released or fully accepted.

## Build note 01 · Access should follow current membership

An organization request has an original creator, but that person’s relationship with the organization can change. Testing an earlier implementation showed that ownership alone could preserve access after membership was removed.

**Creating a record and remaining authorized to access it are separate questions.**

| Context | Intended behavior |
| --- | --- |
| Personal request | Preserve the original owner’s personal access. |
| Organization request, original creator | Require active membership in an active organization. |
| Organization manager | Appropriate organization reads do not automatically grant owner-only actions. |
| Membership removed | Earlier ownership must not preserve organization access. |

```mermaid
flowchart TD
    R["Original creator requests access"] --> C{"Request context"}
    C --> P["Personal request"]
    C --> O["Organization request"]
    P --> K["Personal ownership applies"]
    O --> M{"Active membership and organization?"}
    M -->|Yes| A["Ownership path may allow access"]
    M -->|No| D["Ownership path denies access"]
```

*Conceptual original-creator access path. Other role and session checks also apply.*

### What changed and how it was checked

The correction makes original-creator access to organization requests depend on current membership and organization status. Personal ownership remains separate, as does the distinction between viewing a request and performing owner-only actions.

The reviewed TEST records describe synthetic-account checks in which an already-issued session lost organization-request access after membership removal. Related checks covered legitimate personal access, manager read-only scope, inactive organizations, and denied actions.

Those results belong to dated TEST versions. They do not establish that every workflow or production release is ready. The project remains in development.

### What I’m taking forward

- Test denied behavior alongside successful behavior.
- Enforce permissions beneath the interface.
- Test relationships changing during an existing session.
- Keep evidence tied to a version and environment.
- Distinguish an implemented correction from a verified release.

## Build note 02 · Make the first step easier

Someone asking for help may know that their Wi-Fi keeps dropping without knowing whether the cause is a device, router, or internet connection. The request flow starts with that person’s experience.

| Design choice | Reason |
| --- | --- |
| Familiar categories | Let someone describe the problem without technical vocabulary. |
| A guided sequence | Break the request into smaller decisions instead of one long form. |
| A “Something Else” option | Give uncertain users a way forward. |
| Separate customer and administration views | Give each person a workspace suited to the task they need to perform. |
| Mobile layouts and light/dark themes | Review the experience across different screen sizes and appearances. |

The mobile image above captures a useful review question: a category can appear selected while the footer still asks the user to choose an issue. That visual inconsistency deserves investigation. A screenshot helps identify the question; it does not establish the cause or prove the interaction works.

**The lesson:** review the whole state of a screen—selection, instructions, and next action together.

## Build note 03 · A visible warning needs an accessible connection

The administration reply form includes a warning about public replies. Seeing a warning near a field does not automatically mean assistive technology will associate it with that field.

A small correction gave the warning a unique identifier and connected it to the form control through `aria-describedby`. The project record documents checking the association in the hosted form, alongside an owner report that a screen reader announced it.

That is a bounded check: the screen-reader product and version were not recorded, and it is not a complete accessibility audit.

**The lesson:** visual placement and programmatic relationships both matter. Small markup changes can affect whether someone receives important context before acting.

## Skills this project is developing

- **Product thinking:** turning support needs into categories, steps, and role-specific workflows.
- **Security reasoning:** translating ownership, membership, and session changes into access rules.
- **Troubleshooting:** separating a visible symptom from its underlying cause.
- **Verification:** understanding the difference between a fixture capture, an automated test, and a real-session check.
- **Documentation:** explaining the decision, its evidence, and what still needs work.

## How I work on this project

I direct the requirements and review the experience, using AI assistance for implementation, investigation, and testing. These notes describe the project decisions and learning.

## Coming next

- **Request walkthrough:** follow a synthetic request through its lifecycle.
- **Verification in practice:** a closer look at a failed check, the correction, and the retest.
- **More field notes:** practical troubleshooting and completed security labs.

<sub>Application source remains private. This public showcase contains selected development images and learning notes. Visual source: b2a6e406578eee00ecc6a9a4afb9734bc92a701f. Permission results refer to earlier dated TEST checkpoints, not a fresh acceptance run performed for this write-up.</sub>

[← Back to profile](https://github.com/pickproof)
