---
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_mariadb_database"
sidebar_current: "docs-azurerm-resource-database-mariadb-database"
description: |-
  Manages a MariaDB Database within a MariaDB Server.
---

# azurerm_mariadb_database

Manages a MariaDB Database within a MariaDB Server

## Example Usage

```hcl
resource "azurerm_resource_group" "example" {
  name     = "tfex-mariadb-database-RG"
  location = "westeurope"
}

resource "azurerm_mariadb_server" "example" {
  name                = "mariadb-svr"
  location            = "${azurerm_resource_group.example.location}"
  resource_group_name = "${azurerm_resource_group.example.name}"

  sku {
    name     = "B_Gen5_2"
    capacity = 2
    tier     = "Basic"
    family   = "Gen5"
  }

  storage_profile {
    storage_mb            = 51200
    backup_retention_days = 7
    geo_redundant_backup  = "Disabled"
  }

  administrator_login          = "acctestun"
  administrator_login_password = "H@Sh1CoR3!"
  version                      = "10.2"
  ssl_enforcement              = "Enabled"
}

resource "azurerm_mariadb_database" "example" {
  name                = "mariadb_database"
  resource_group_name = "${azurerm_resource_group.example.name}"
  server_name         = "${azurerm_mariadb_server.example.name}"
  charset             = "utf8"
  collation           = "utf8_general_ci"
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) Specifies the name of the MariaDB Database, which needs [to be a valid MariaDB identifier](https://mariadb.com/kb/en/library/identifier-names/). Changing this forces a
    new resource to be created.

* `server_name` - (Required) Specifies the name of the MariaDB Server. Changing this forces a new resource to be created.

* `resource_group_name` - (Required) The name of the resource group in which the MariaDB Server exists. Changing this forces a new resource to be created.

* `charset` - (Required) Specifies the Charset for the MariaDB Database, which needs [to be a valid MariaDB Charset](https://mariadb.com/kb/en/library/setting-character-sets-and-collations). Changing this forces a new resource to be created.

* `collation` - (Required) Specifies the Collation for the MariaDB Database, which needs [to be a valid MariaDB Collation](https://mariadb.com/kb/en/library/setting-character-sets-and-collations). Changing this forces a new resource to be created.

## Attributes Reference

The following attributes are exported:

* `id` - The ID of the MariaDB Database.

## Import

MariaDB Database's can be imported using the `resource id`, e.g.

```shell
terraform import azurerm_mariadb_database.database1 /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mygroup1/providers/Microsoft.DBforMariaDB/servers/server1/databases/database1
```
