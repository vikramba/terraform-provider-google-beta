---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    Type: MMv1     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "Artifact Registry"
layout: "google"
page_title: "Google: google_artifact_registry_repository_iam"
sidebar_current: "docs-google-artifact-registry-repository-iam"
description: |-
  Collection of resources to manage IAM policy for Artifact Registry Repository
---

# IAM policy for Artifact Registry Repository
Three different resources help you manage your IAM policy for Artifact Registry Repository. Each of these resources serves a different use case:

* `google_artifact_registry_repository_iam_policy`: Authoritative. Sets the IAM policy for the repository and replaces any existing policy already attached.
* `google_artifact_registry_repository_iam_binding`: Authoritative for a given role. Updates the IAM policy to grant a role to a list of members. Other roles within the IAM policy for the repository are preserved.
* `google_artifact_registry_repository_iam_member`: Non-authoritative. Updates the IAM policy to grant a role to a new member. Other members for the role for the repository are preserved.

~> **Note:** `google_artifact_registry_repository_iam_policy` **cannot** be used in conjunction with `google_artifact_registry_repository_iam_binding` and `google_artifact_registry_repository_iam_member` or they will fight over what your policy should be.

~> **Note:** `google_artifact_registry_repository_iam_binding` resources **can be** used in conjunction with `google_artifact_registry_repository_iam_member` resources **only if** they do not grant privilege to the same role.

~> **Note:** `google_artifact_registry_repository_iam_member` resources should depend on the associated `google_artifact_registry_repository`. If it does not have an implicit dependency, the dependency should be set explicitly using [`depends_on`](https://www.terraform.io/language/meta-arguments/depends_on). Otherwise, it may create race condition.


~> **Warning:** This resource is in beta, and should be used with the terraform-provider-google-beta provider.
See [Provider Versions](https://terraform.io/docs/providers/google/guides/provider_versions.html) for more details on beta resources.


## google\_artifact\_registry\_repository\_iam\_policy

```hcl
data "google_iam_policy" "admin" {
  provider = google-beta
  binding {
    role = "roles/viewer"
    members = [
      "user:jane@example.com",
    ]
  }
}

resource "google_artifact_registry_repository_iam_policy" "policy" {
  provider = google-beta
  project = google_artifact_registry_repository.my-repo.project
  location = google_artifact_registry_repository.my-repo.location
  repository = google_artifact_registry_repository.my-repo.name
  policy_data = data.google_iam_policy.admin.policy_data
}
```

## google\_artifact\_registry\_repository\_iam\_binding

```hcl
resource "google_artifact_registry_repository_iam_binding" "binding" {
  provider = google-beta
  project = google_artifact_registry_repository.my-repo.project
  location = google_artifact_registry_repository.my-repo.location
  repository = google_artifact_registry_repository.my-repo.name
  role = "roles/viewer"
  members = [
    "user:jane@example.com",
  ]
}
```

## google\_artifact\_registry\_repository\_iam\_member

```hcl
resource "google_artifact_registry_repository_iam_member" "member" {
  provider = google-beta
  project = google_artifact_registry_repository.my-repo.project
  location = google_artifact_registry_repository.my-repo.location
  repository = google_artifact_registry_repository.my-repo.name
  role = "roles/viewer"
  member = "user:jane@example.com"
}
```

## Argument Reference

The following arguments are supported:

* `repository` - (Required) Used to find the parent resource to bind the IAM policy to
* `location` - (Required) The name of the location this repository is located in.
 Used to find the parent resource to bind the IAM policy to

* `project` - (Optional) The ID of the project in which the resource belongs.
    If it is not provided, the project will be parsed from the identifier of the parent resource. If no project is provided in the parent identifier and no project is specified, the provider project is used.

* `member/members` - (Required) Identities that will be granted the privilege in `role`.
  Each entry can have one of the following values:
  * **allUsers**: A special identifier that represents anyone who is on the internet; with or without a Google account.
  * **allAuthenticatedUsers**: A special identifier that represents anyone who is authenticated with a Google account or a service account.
  * **user:{emailid}**: An email address that represents a specific Google account. For example, alice@gmail.com or joe@example.com.
  * **serviceAccount:{emailid}**: An email address that represents a service account. For example, my-other-app@appspot.gserviceaccount.com.
  * **group:{emailid}**: An email address that represents a Google group. For example, admins@example.com.
  * **domain:{domain}**: A G Suite domain (primary, instead of alias) name that represents all the users of that domain. For example, google.com or example.com.
  * **projectOwner:projectid**: Owners of the given project. For example, "projectOwner:my-example-project"
  * **projectEditor:projectid**: Editors of the given project. For example, "projectEditor:my-example-project"
  * **projectViewer:projectid**: Viewers of the given project. For example, "projectViewer:my-example-project"

* `role` - (Required) The role that should be applied. Only one
    `google_artifact_registry_repository_iam_binding` can be used per role. Note that custom roles must be of the format
    `[projects|organizations]/{parent-name}/roles/{role-name}`.

* `policy_data` - (Required only by `google_artifact_registry_repository_iam_policy`) The policy data generated by
  a `google_iam_policy` data source.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are
exported:

* `etag` - (Computed) The etag of the IAM policy.

## Import

For all import syntaxes, the "resource in question" can take any of the following forms:

* projects/{{project}}/locations/{{location}}/repositories/{{repository}}
* {{project}}/{{location}}/{{repository}}
* {{location}}/{{repository}}
* {{repository}}

Any variables not passed in the import command will be taken from the provider configuration.

Artifact Registry repository IAM resources can be imported using the resource identifiers, role, and member.

IAM member imports use space-delimited identifiers: the resource in question, the role, and the member identity, e.g.
```
$ terraform import google_artifact_registry_repository_iam_member.editor "projects/{{project}}/locations/{{location}}/repositories/{{repository}} roles/viewer user:jane@example.com"
```

IAM binding imports use space-delimited identifiers: the resource in question and the role, e.g.
```
$ terraform import google_artifact_registry_repository_iam_binding.editor "projects/{{project}}/locations/{{location}}/repositories/{{repository}} roles/viewer"
```

IAM policy imports use the identifier of the resource in question, e.g.
```
$ terraform import google_artifact_registry_repository_iam_policy.editor projects/{{project}}/locations/{{location}}/repositories/{{repository}}
```

-> **Custom Roles**: If you're importing a IAM resource with a custom role, make sure to use the
 full name of the custom role, e.g. `[projects/my-project|organizations/my-org]/roles/my-custom-role`.

## User Project Overrides

This resource supports [User Project Overrides](https://www.terraform.io/docs/providers/google/guides/provider_reference.html#user_project_override).
