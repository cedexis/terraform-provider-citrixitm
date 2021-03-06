---
layout: "citrixitm"
page_title: "Citrix ITM: citrixitm_dns_app"
sidebar_current: "docs-citrixitm-resource-dns-app"
description: |-
  Manages a Citrix ITM DNS app resource.
---

# citrixitm_dns_app

The `citrixitm_dns_app` resource type is used to create and maintain a custom Citrix ITM DNS app. You can read more about custom DNS apps at [Custom JavaScript Apps](https://docs.citrix.com/en-us/citrix-intelligent-traffic-management/openmix.html#custom-javascript-apps).

## Example Usage

```hcl
resource "citrixitm_dns_app" "my_app" {
  name = "My App"
  description = "An example JavaScript DNS routing app"
  app_data = <<EOF
function init(config) {
  config.requestProvider('edgecast');
}

function onRequest(request, response) {
  response.respond('edgecast', 'www.example.edgecastcdn.net');
  response.setTTL(600);
}
EOF
  fallback_cname = "fallback.example.com"
}
```

## Argument Reference

The following arguments are supported:

* app_data - (Required) A string containing the JavaScript code defining the app's behavior.

* description - (Optional) A description for the app.

* fallback_cname - (Required) The CNAME that the framework should respond with in the event of a problem.

* fallback_ttl - (Optional) The TTL that should be specified when the framework issues a fallback response. The default is 20.

* name - (Required) A descriptive name for the app.

## Attributes Reference

The following attributes are exported:

* cname - The CNAME used to reach the app. This is determined automatically when the app is created.

* version - The version number of the app. This is automatically incremented when the app is updated.

## Import

An existing Citrix ITM DNS app instance may be imported using its app ID, which is found in the Citrix ITM Portal. For example, say you have an existing app with ID 123, and a Terraform configuration like the following:

```hcl
resource "citrixitm_dns_app" "my_app" {
    // settings...
}
```

The following command can be used to create an association between the existing app and the Terraform configuration.

```bash
$ terraform import citrixitm_dns_app.my_app 123
```

At this point the resource configuration may or may not match production state, so you should also run `terraform plan` to see if any changes need to be made in order to reconcile the differences.
