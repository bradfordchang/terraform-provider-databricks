---
subcategory: "Unity Catalog"
---
# databricks_metastore_assignment (Resource)

A single [databricks_metastore](docs/resources/metastore.md) can be shared across Databricks workspaces, and each linked workspace has a consistent view of the data and a single set of access policies. You can only create a single metastore for each region in which your organization operates.

## Example Usage

```hcl
resource "databricks_metastore" "this" {
  name          = "primary"
  storage_root  = "s3://${aws_s3_bucket.metastore.id}/metastore"
  owner         = "uc admins"
  region        = "us-east-1"
  force_destroy = true
}

resource "databricks_metastore_assignment" "this" {
  metastore_id = databricks_metastore.this.id
  workspace_id = local.workspace_id
}
```

## Import

This resource can be imported by combination of workspace id and metastore id:

```bash
terraform import databricks_metastore_assignment.this '<workspace_id>|<metastore_id>'
```

## Argument Reference

The following arguments are required:

* `metastore_id` - Unique identifier of the parent Metastore
* `workspace_id` - id of the workspace for the assignment
* `default_catalog_name` - (Optional) Default catalog used for this assignment, default to `hive_metastore`

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - ID of this metastore assignment in form of `<metastore_id>|<metastore_id>`.
