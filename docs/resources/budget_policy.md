---
subcategory: "Billing"
---
# databricks_budget_policy Resource
Administrators can use budget policies to ensure that the correct tags appear automatically on serverless resources without depending on users to attach tags manually, allowing for customized cost reporting and chargebacks.

Budget policies consist of tags that are applied to any serverless compute activity incurred by a user assigned to the policy.

The tags are logged in your billing records, allowing you to attribute serverless usage to specific budgets.

-> **Note** This resource can only be used with an account-level provider!

## Example Usage
```hcl
resource "databricks_budget_policy" "this" {
  policy_name = "my-budget-policy"
  custom_tags = [{
    key = "mykey"
    value = "myvalue"
  }]
}
```

## Arguments
The following arguments are supported:
* `binding_workspace_ids` (list of integer, optional) - List of workspaces that this budget policy will be exclusively bound to.
  An empty binding implies that this budget policy is open to any workspace in the account
* `custom_tags` (list of CustomPolicyTag, optional) - A list of tags defined by the customer. At most 20 entries are allowed per policy
* `policy_name` (string, optional) - The name of the policy.
  - Must be unique among active policies.
  - Can contain only characters from the ISO 8859-1 (latin1) set.
  - Can't start with reserved keywords such as `databricks:default-policy`

### CustomPolicyTag
* `key` (string, required) - The key of the tag.
  - Must be unique among all custom tags of the same policy
  - Cannot be “budget-policy-name”, “budget-policy-id” or "budget-policy-resolution-result" -
  these tags are preserved
* `value` (string, optional) - The value of the tag

## Attributes
In addition to the above arguments, the following attributes are exported:
* `policy_id` (string) - The Id of the policy. This field is generated by Databricks and globally unique

## Import
As of Terraform v1.5, resources can be imported through configuration.
```hcl
import {
  id = policy_id
  to = databricks_budget_policy.this
}
```

If you are using an older version of Terraform, import the resource using the `terraform import` command as follows:
```sh
terraform import databricks_budget_policy policy_id
```