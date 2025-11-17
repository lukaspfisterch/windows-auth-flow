# User Execution Shell (UES)

![UES Schema](images/ues-schema.png "User Execution Shell – visual schema")

**Definition**  
The *User Execution Shell* (UES) is the active operational layer created when a user signs in.  
It is not a login screen or a token store — it is the **runtime boundary** within which identity, policy, and execution coexist coherently.

---

## 1. Core Idea

Every user action after sign-in occurs inside a persistent shell that holds:
- the authenticated identity (PRT / WAM state),
- device and session bindings,
- policy context (CA, compliance, conditional state),
- and the live process surface (apps, WebView2, scripts, brokers).

When the shell stays intact, authentication flows are stable.  
When the shell fractures (profile reset, token drift, VDI rehydrate), authentication decays even if credentials are still valid.

---

## 2. Relation to the Authentication Flow

| Layer | Function within UES |
|-------|----------------------|
| Device / Logon | Instantiates the shell; establishes base trust |
| PRT / WAM | Maintain identity continuity inside the shell |
| MSAL / OneAuth | Issue delegated tokens to child processes |
| CA / Policy | Evaluate outbound actions leaving the shell |

The authentication flow is therefore the *breathing mechanism* of the UES — it keeps the shell alive.

---

## 3. Why It Matters

- **Stability:** a healthy UES equals predictable sign-in and token renewal behavior.  
- **Observability:** each signal (PRT age, WAM error, CA result) belongs to one shell instance.  
- **Governance:** higher layers (InfraCommand, ShadowState) can control lifecycle transitions safely.

---

## 4. Scope of This Repository

This repository focuses on the authentication subsystem *inside* the UES.  
Broader aspects — state diffing, telemetry, controlled teardown — belong to adjacent modules that treat the UES as a measurable entity.
