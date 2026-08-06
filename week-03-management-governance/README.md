# Week 3 — Management & Governance

## Lab: Pricing, Azure Policy, and Custom RBAC

### 1. Hub-and-spoke cost estimate

Built a minimal hub-and-spoke pricing model for a small business:
- Azure Firewall Standard (1x) — €700/month
- VPN Gateway VpnGw1 (1x) — €130/month
- 2x Ubuntu D2s_v3 VMs with Standard SSD — €160/month
- Public IP (static) — €3/month
- Log Analytics ingestion 10GB/month — €25/month

**Total: approx €1,020/month for a minimal production hub-and-spoke.**

Excel export saved as `hub-spoke-estimate.xlsx`.

**Key takeaway:** Azure Firewall alone is ~70% of the monthly cost. Many small
businesses use NVAs (pfSense on a VM) or NSGs alone to avoid this. When designing,
always check: does the client need Azure Firewall, or would NSGs suffice?

### 2. Azure Policy — tagging enforcement

Created assignment `require-environment-tag` at subscription scope using the
built-in policy "Require a tag on resources", parameterised with tag name `Environment`.

Tested:
- Creating an RG without the tag → BLOCKED at deploy time.
- Creating an RG with `Environment = test` → succeeded.

Policy remains active. All future resources in this subscription must have the tag.

### 3. Custom RBAC role

Created `custom-reader-week3` — cloned the built-in Reader role and scoped it
to only the `rg-rbac-test` resource group.

Assigned the role to myself. Verified I can view resources in that RG but nothing
outside it. The role definition JSON is saved as `custom-role.json`.

---

## Azure Policy vs RBAC — Trade-offs

Both control what happens in Azure, but at different layers. Confusing them is a
common interview mistake.

### RBAC — controls WHO can do WHAT

- Acts at the **identity layer**: "which user/group/service principal can perform which action?"
- Checked before any operation.
- Denies by default (no assignment = no access).
- Cannot enforce configuration state — RBAC can't say "all VMs must have Standard SSD."
- Cannot audit compliance retroactively.

### Azure Policy — controls WHAT can exist

- Acts at the **resource layer**: "which resource configurations are allowed?"
- Evaluates at deploy time (Deny/Modify effects) or continuously (Audit effect).
- Applies regardless of who's making the change — even the subscription owner
  can be blocked by a Deny policy.
- Can enforce state (tag values, allowed SKUs, allowed regions).
- Can audit existing resources for compliance.

### When to use which

| Scenario | Use |
|---|---|
| Prevent a junior engineer from deleting production | RBAC |
| Enforce every resource has an `Environment` tag | Azure Policy |
| Restrict who can create VNets | RBAC |
| Prevent anyone (including admins) from deploying premium SKU VMs | Azure Policy |
| Give a support team read-only access to one RG | RBAC (custom role, scoped) |
| Ensure all storage accounts use HTTPS-only | Azure Policy |

### Common pattern in real enterprises

Use them together:
1. **RBAC** decides who has permission to try.
2. **Azure Policy** decides whether the specific action they're trying is allowed
   under organisational standards.

Example: An engineer with Contributor rights (RBAC) tries to deploy a VM in
West US. Azure Policy blocks it because company standard is North Europe / West Europe only.
The engineer had permission to try, but policy enforced the standard.

### One more trap for interviews

Azure Policy does NOT prevent someone from LOGGING IN. It only prevents actions
against Azure resources. Identity restrictions (MFA, conditional access) are
handled by Entra ID / Conditional Access, not Policy.

---

## Time to complete
~75 minutes including write-up.

## Screenshots
See `./screenshots/` folder.