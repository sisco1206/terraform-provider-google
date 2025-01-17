---
subcategory: "Compute Engine"
page_title: "Google: google_compute_project_metadata"
description: |-
  Manages common instance metadata
---

# google\_compute\_project\_metadata

Authoritatively manages metadata common to all instances for a project in GCE. For more information see
[the official documentation](https://cloud.google.com/compute/docs/storing-retrieving-metadata)
and
[API](https://cloud.google.com/compute/docs/reference/latest/projects/setCommonInstanceMetadata).

~> **Note:**  This resource manages all project-level metadata including project-level ssh keys.
Keys unset in config but set on the server will be removed. If you want to manage only single
key/value pairs within the project metadata rather than the entire set, then use
[google_compute_project_metadata_item](compute_project_metadata_item.html).

## Example Usage

```hcl
resource "google_compute_project_metadata" "default" {
  metadata = {
    foo  = "bar"
    fizz = "buzz"
    "13" = "42"
  }
}
```

## Example Usage - Adding an SSH Key 

```hcl
/*
A key set in project metadata is propagated to every instance in the project.
This resource configuration is prone to causing frequent diffs as Google adds SSH Keys when the SSH Button is pressed in the console.
It is better to use OS Login instead.
*/
resource "google_compute_project_metadata" "my_ssh_key" {
  metadata = {
    ssh-keys = <<EOF
      dev:ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILg6UtHDNyMNAh0GjaytsJdrUxjtLy3APXqZfNZhvCeT dev
      foo:ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILg6UtHDNyMNAh0GjaytsJdrUxjtLy3APXqZfNZhvCeT bar
    EOF
  }
}
```

## Argument Reference

The following arguments are supported:

* `metadata` - (Required) A series of key value pairs.

- - -

* `project` - (Optional) The ID of the project in which the resource belongs. If it
    is not provided, the provider project is used.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `{{project}}`

## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 4 minutes (also used for update).
- `delete` - Default is 4 minutes.

## Import

This resource can be imported using the project ID:

`terraform import google_compute_project_metadata.foo my-project-id`
