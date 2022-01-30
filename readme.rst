Deployment of an Active/Active BIG-IP synchronized cluster in Azure
###################################################################

Architecture
*****************************************

.. image:: ./_pictures/tierII_access_path.png
   :align: center
   :width: 800
   :alt: Access path for TierI applications

.. image:: ./_pictures/tierII_access_path.png
   :align: center
   :width: 800
   :alt: Access path for TierII applications

.. image:: ./_pictures/routing_view.png
   :align: center
   :width: 800
   :alt: Routing view

.. contents:: Contents
    :local:

Deployment
*****************************************

Azure infra
=========================================

Workflow template ``wf-create_create_edge_security_inbound``

=============================================================   =============================================       =============================================   =============================================   =============================================   =============================================
Job template                                                    objective                                           playbook                                        activity or play targeted in role               inventory                                       credential
=============================================================   =============================================       =============================================   =============================================   =============================================   =============================================
``poc-azure_create_hub_edge_security_inbound``                  Create a resource group and network objects         ``playbooks/poc-azure.yaml``                    ``create_hub_edge_security_inbound``            CMP_inv_CloudBuilderf5                          <Service Principal>
=============================================================   =============================================       =============================================   =============================================   =============================================   =============================================

==============================================  =============================================
Extra variable                                  Description
==============================================  =============================================
``extra_platform_name``                         logical platform name
``extra_platform_tags``                         tags on resources
``extra_location``                              region
``extra_availability_zone``                     list of Availability Zone IDs
``extra_vnet_address_prefixes``                 dataplane SuperNet
``extra_nginx_subnet_address_prefix``           NGINX dataplane subnet
``extra_external_subnet_address_prefix``        BIG-IP dataplane subnet
``extra_management_subnet_address_prefix``      Management subnet
``extra_subnet_mgt_on_premise``                 remote Cross management subnet
``extra_lb_external_vip``                       ILB frontendIP to load balance AWAF VMSS
``extra_vip_address_list_awaf``                 Subnet list to route via AWAF VMSS
==============================================  =============================================

.. code-block:: yaml

    activity: create_hub_edge_security_inbound
    extra_availability_zone:
      - 1
    extra_external_subnet_address_prefix: 10.100.2.0/24
    extra_lb_external_vip: 10.100.2.10
    extra_location: francecentral
    extra_management_subnet_address_prefix: 10.100.0.0/24
    extra_nginx_subnet_address_prefix: 10.100.1.0/24
    extra_platform_name: ingress
    extra_platform_tags: environment=POC platform=ingress project=customer_name
    extra_subnet_mgt_on_premise: 10.0.0.0/24
    extra_vip_address_list_awaf:
      - 10.100.10.0/24
    extra_vnet_address_prefixes: 10.100.0.0/16


