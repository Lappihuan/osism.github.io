---
sidebar_label: OSISM 8
---

# OSISM 8

Instructions for the upgrade can be found in the [Upgrade Guide](../guides/upgrade-guide/manager).

The release notes build on each other. When upgrading from 7.0.1 to 8.0.0, you should
therefore not only read and take into account the release notes for 8.0.0 but also the
previous release notes. The same applies to an update from, for example, 8.0.0 to 8.0.2.
The release notes for 8.0.1 must then also be taken into account.

| Release                  | Release Date        |
|:-------------------------|:--------------------|
| [8.1.0](#810-20250116)   | 16. January 2025    |
| [8.0.2](#802-20241006)   | 6. October 2024     |
| [8.0.1](#801-20240924)   | 24. September 2024  |
| [8.0.0](#800-20240911)   | 11. September 2024  |

## Next

Release date: ???

* The `osism.services.virtualbmc` role was removed. Tenks, a virtual bare metal cluster management,
  will be used in the future. The related virtualbmc container images was also removed.

* Ansible 11 is supported by all OSISM collections and will be used from now on.

* New `osism.services.dnsmasq` role was added.

* New `osism.services.httpd` role was added.

## 8.1.0 (20250116)

Release date: 17. January 2025

* The Ceph service images have not been rebuilt. No upgrade of Ceph is required.

* All OpenStack service images have been rebuilt. An upgrade of OpenStack
  services is recommended.

* The service images for Kuryr and Zun will be not longer included in the next stable release.

* The infrastructure service images (MariaDB, RabbitMQ, ..) have been rebuilt. An upgrade is recommended.

* The network service images (OVN, OVS) have been rebuilt. An upgrade is recommended.

* The monitoring service images (Prometheus & all Prometheus exporters) have been rebuilt. An upgrade is recommended.

* The logging service images (OpenSearch, Fluentd) have been rebuilt. An upgrade is recommended.

* The `osism.commons.clevis` role was removed.

* The `osism.services.tang` role was removed.

* The `osism.services.metering` role was removed.

* New `osism.services.wazuh_agent` role was added.

* New `osism.services.sshd` role was added.

* New `osism.services.teleport` role was added.

## 8.0.2 (20241006)

Release date: 6. October 2024

* The Ceph service images have not been rebuilt. No upgrade of Ceph is required.

* The OpenStack service images for Ironic have been rebuilt.
  Upgrades of the Ironic service is recommended.

  * The Ironic have been rebuilt because of a critical security
    issues. Further details can be found in security advisory
    [OSSA-2024-004: Ironic fails to verify checksums of supplied image_source URLs when configured to convert images to raw for streaming](https://security.openstack.org/ossa/OSSA-2024-004.html).
    This upgrade is important.

* Other OpenStack service images have not been rebuilt. No upgrade of other OpenStack
  services is required.

* The infrastructure service images (MariaDB, RabbitMQ, ..) have not been rebuilt. No upgrade is required.

* The network service images (OVN, OVS) have not been rebuilt. No upgrade is required.

* The monitoring service images (Prometheus & all Prometheus exporters) have not been rebuilt. No upgrade is required.

* The logging service images (OpenSearch, Fluentd) have not been rebuilt. No upgrade is required.

* The generic inventory group is now used as the default for the FRR service.

* In the inventory reconciler, the handling of inventory groups from the configuration repoistory
  and the Netbox has been improved. Groups that exist both in the configuration repository and in
  the Netbox are now merged and do not overwrite each other.

* With the `proxy_enable` parameter of `osism.commons.proxy` it is possible to disable the proxy
  configuration. This makes it easier to configure global proxy parameters
  (via environment or group_vars) and to skip them via host_vars only for
  specific hosts.

* The `osism.commons.repository` role now supports the newer DEB822 format and is now also
  usuable on Ubuntu 24.04 based ARM nodes.

* With the `nexus_force_init` parameter of `osism.services.nexus` it is possible to force the
  initialisation of the Nexus service.

* A temporary workaround for the installation of python3-docker on Ubuntu 24.04 has been added
  to `osism.services.docker`. This will be removed later.

* With the `docker_storage_containerd_snapshotter` parameter of `osism.services.docker` it is
  possible to enable the [experimental container-snapshotter feature](https://docs.docker.com/engine/storage/containerd/).
  Only use this on new nodes.

* With the `frr_enable_bfdd` parameter of `osism.services.frr` it is possible to enable the BFD
  daemon.

* All `frr_uplinks__*` parameters will now be merged with the `frr_uplinks`
  parameter of `osism.services.frr`. This way it is possible to add host specific `frr_uplinks` via
  the host vars.

* The integration of the [OpenStack Resource Manager](https://github.com/osism/openstack-resource-manager)
  was started in the OSISM CLI.

  * `osism manage compute list`
  * `osism manage compute list HOSTNAME`
  * `osism manage server list`
  * `osism manage volume list`
  * `osism manage compute evacuate HOSTNAME`
  * `osism manage compute disable HOSTNAME`
  * `osism manage compute enable HOSTNAME`
  * `osism manage compute start HOSTNAME`
  * `osism manage compute stop HOSTNAME`

## 8.0.1 (20240924)

Release date: 24. September 2024

* The Ceph service images have not been rebuilt. No upgrade of Ceph is required.

* The OpenStack service images have not been rebuilt. No upgrade of OpenStack is required.

* The infrastructure service images (MariaDB, RabbitMQ, ..) have not been rebuilt. No upgrade is required.

* The network service images (OVN, OVS) have not been rebuilt. No upgrade is required.

* The monitoring service images (Prometheus & all Prometheus exporters) have not been rebuilt. No upgrade is required.

* The logging service images (OpenSearch, Fluentd) have not been rebuilt. No upgrade is required.

* The Prometheus OpenStack Exporter is no longer enabled by default as there have been frequent
  OOM problems and the exporter generates a very high load on the OpenStack APIs. If the exporter
  is still to be used, the `enable_prometheus_openstack_exporter` parameter can be used for this.

  ```yaml title="environments/kolla/configuration.yml"
  enable_prometheus_openstack_exporter: "yes"
  ```

* An error when upgrading the Postres database of the Netbox service has been fixed. Upgrades
  of the Postgres database are now done automatically.

* The Scaphandre service now accesses the host PIDs by default in order to better assign the
  consumption data to the running processes. If this is not wanted, it can be switched off using
  the `scaphandre_share_pids_with_host` parameter.

  ```yaml title="environments/configuration.yml"
  scaphandre_share_pids_with_host: false
  ```

* Both Ceph play (`osism apply ceph`)  and the validation of Ceph OSD services
  (`osism validate ceph-osds`) have been fixed for non-HCI environments.  

* Inventory groups in `inventory/20-roles` now also overwrite groups in all other files
  (with exception of `inventory/99-ovewrite`). The same applies to groups that are set
  via labels in Netbox. This makes it possible, for example, to define the load balancers
  via the `loadbalancer` group directly via `inventory/20-roles` or a Netbox label.

## 8.0.0 (20240911)

Release date: 11. September 2024

* In this release, OpenStack has been upgraded from OpenStack 2023.2 (Bobcat) to OpenStack
  2024.1 (Caracal). All OpenStack images and images of the associated infrastructure services
  (MariaDB, RabbitMQ, ..) have been rebuilt. These must be upgraded as required.

* The Ceph service images have not been rebuilt. No upgrade of Ceph is required.

* New manager features.

  * The configuration is now synchronised with `osism sync configuration`.
    The `osism configuration sync` command will be removed in the future.

  * The inventory is now synchronised with `osism sync inventory`.
    The `osism reconciler sync` command will be removed in the future.

  * Netbox is now synchronised with `osism sync netbox`.
    The `osism netbox sync` command will be removed in the future.

  * Ironic is now synchronised with `osism sync ironic`.
    The `osism netbox sync ironic` command will be removed in the future.

### Upgrade notes

* The following secrets must be added in `environments/kolla/secrets.yml`:

  ```yaml
  prometheus_skyline_password:  # generate with: pwgen 32 1
  ```

* The `designate-sink` service of OpenStack Designate is now disabled by default.
  The following must be added in `environments/kolla/configuration.yml` to keep
  the `designate-sink` service enabled.

  ```yaml
  designate_enable_notifications_sink: "yes"
  ```

* The file containing the custom local settings of Horizon has been renamed from
  `custom_local_settings` to `_9999-custom-settings.py`.

### Other & References

OpenStack 2024.1 press announcement: https://www.openstack.org/software/openstack-caracal

OpenStack 2024.1 release notes: https://releases.openstack.org/caracal/index.html

Release notes for each OpenStack service:

* Barbican: https://docs.openstack.org/releasenotes/barbican/2024.1.html
* Ceilometer: https://docs.openstack.org/releasenotes/ceilometer/2024.1.html
* Cinder: https://docs.openstack.org/releasenotes/cinder/2024.1.html
* Designate: https://docs.openstack.org/releasenotes/designate/2024.1.html
* Glance: https://docs.openstack.org/releasenotes/glance/2024.1.html
* Heat: https://docs.openstack.org/releasenotes/heat/2024.1.html
* Horizon: https://docs.openstack.org/releasenotes/horizon/2024.1.html
* Ironic: https://docs.openstack.org/releasenotes/ironic/2024.1.html
* Keystone: https://docs.openstack.org/releasenotes/keystone/2024.1.html
* Manila: https://docs.openstack.org/releasenotes/manila/2024.1.html
* Neutron: https://docs.openstack.org/releasenotes/neutron/2024.1.html
* Nova: https://docs.openstack.org/releasenotes/nova/2024.1.html
* Octavia: https://docs.openstack.org/releasenotes/octavia/2024.1.html
* Placement: https://docs.openstack.org/releasenotes/placement/2024.1.html
* Skyline: https://docs.openstack.org/releasenotes/skyline-apiserver/2024.1.html, https://docs.openstack.org/releasenotes/skyline-console/2024.1.html
