---
subcategory: "Database"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_postgresql_server"
description: |-
  Manages a PostgreSQL Server.
---

# azurerm_postgresql_server

Manages a PostgreSQL Server.

## Example Usage

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_postgresql_server" "example" {
  name                = "example-psqlserver"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  administrator_login          = "psqladminun"
  administrator_login_password = "H@Sh1CoR3!"

  sku_name   = "GP_Gen5_4"
  version    = "9.6"
  storage_mb = 640000

  backup_retention_days        = 7
  geo_redundant_backup_enabled = true
  auto_grow_enabled            = true

  infrastructure_encryption_enabled = true
  public_network_access_enabled     = false
  ssl_enforcement_enabled           = true
  ssl_minimal_tls_version_enforced  = "TLS1_2"
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) Specifies the name of the PostgreSQL Server. Changing this forces a new resource to be created.

* `resource_group_name` - (Required) The name of the resource group in which to create the PostgreSQL Server. Changing this forces a new resource to be created.

* `location` - (Required) Specifies the supported Azure location where the resource exists. Changing this forces a new resource to be created.

* `sku_name` - (Required) Specifies the SKU Name for this PostgreSQL Server. The name of the SKU, follows the `tier` + `family` + `cores` pattern (e.g. `B_Gen4_1`, `GP_Gen5_8`). For more information see the [product documentation](https://docs.microsoft.com/en-us/rest/api/postgresql/servers/create#sku).

* `version` - (Required) Specifies the version of PostgreSQL to use. Valid values are `9.5`, `9.6`, `10`, `10.0`, and `11`. Changing this forces a new resource to be created.

* `administrator_login` - (Optional) The Administrator Login for the PostgreSQL Server. Required when `create_mode` is `Default`. Changing this forces a new resource to be created.

* `administrator_login_password` - (Optional) The Password associated with the `administrator_login` for the PostgreSQL Server. Required when `create_mode` is `Default`.

* `auto_grow_enabled` - (Optional) Enable/Disable auto-growing of the storage. Storage auto-grow prevents your server from running out of storage and becoming read-only. If storage auto grow is enabled, the storage automatically grows without impacting the workload. The default value if not explicitly specified is `true`.

* `backup_retention_days` - (Optional) Backup retention days for the server, supported values are between `7` and `35` days.

* `create_mode` - (Optional) The creation mode. Can be used to restore or replicate existing servers. Possible values are `Default`, `Replica`, `GeoRestore`, and `PointInTimeRestore`. Defaults to `Default.`

* `creation_source_server_id` - (Optional) For creation modes other then default the source server ID to use.

* `geo_redundant_backup_enabled` - (Optional) Turn Geo-redundant server backups on/off. This allows you to choose between locally redundant or geo-redundant backup storage in the General Purpose and Memory Optimized tiers. When the backups are stored in geo-redundant backup storage, they are not only stored within the region in which your server is hosted, but are also replicated to a paired data center. This provides better protection and ability to restore your server in a different region in the event of a disaster. This is not support for the Basic tier.

* `infrastructure_encryption_enabled` - (Optional) Whether or not infrastructure is encrypted for this server. Defaults to `false`. Changing this forces a new resource to be created.

~> **Note:** This property is currently still in development by Microsoft. It is suggested to leave as `false` as not doing so can lead to unclear error messages.

* `public_network_access_enabled` - (Optional) Whether or not public network access is allowed for this server. Defaults to `true`.

* `restore_point_in_time` - (Optional) When `create_mode` is `PointInTimeRestore` the point in time to restore from `creation_source_server_id`. 

* `ssl_enforcement_enabled` - (Optional) Specifies if SSL should be enforced on connections. Possible values are `true` and `false`.

* `ssl_minimal_tls_version_enforced` - (Optional) The mimimun TLS version to support on the sever. Possible values are `TLSEnforcementDisabled`, `TLS1_0`, `TLS1_1`, and `TLS1_2`. Defaults to `TLSEnforcementDisabled`.
 
* `storage_mb` - (Optional) Max storage allowed for a server. Possible values are between `5120` MB(5GB) and `1048576` MB(1TB) for the Basic SKU and between `5120` MB(5GB) and `4194304` MB(4TB) for General Purpose/Memory Optimized SKUs. For more information see the [product documentation](https://docs.microsoft.com/en-us/rest/api/postgresql/servers/create#StorageProfile).

* `tags` - (Optional) A mapping of tags to assign to the resource.  

## Attributes Reference

The following attributes are exported:

* `id` - The ID of the PostgreSQL Server.

* `fqdn` - The FQDN of the PostgreSQL Server.

## Timeouts

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:

* `create` - (Defaults to 60 minutes) Used when creating the PostgreSQL Server.
* `update` - (Defaults to 60 minutes) Used when updating the PostgreSQL Server.
* `read` - (Defaults to 5 minutes) Used when retrieving the PostgreSQL Server.
* `delete` - (Defaults to 60 minutes) Used when deleting the PostgreSQL Server.

## Import

PostgreSQL Server's can be imported using the `resource id`, e.g.

```shell
terraform import azurerm_postgresql_server.server1 /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mygroup1/providers/Microsoft.DBforPostgreSQL/servers/server1
```
