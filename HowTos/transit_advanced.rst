.. meta::
  :description: Multi-Cloud Transit Network Advanced
  :keywords: Transit VPC, Transit hub, AWS Global Transit Network, Encrypted Peering, Transitive Peering, AWS VPC Peering, VPN


================================================================
Transit Advanced Config
================================================================

.. Note::

 The advanced configuration applies to each Aviatrix Transit Gateway. To edit a Transit Gateway's advanced configuration, go to Multi-Cloud Transit > Advanced Config. 

Transit Gateway
---------------------------------------

To edit the advanced configuration for any Transit Gateway, click on this dropdown menu and select the Transit Gateway to edit.

Local AS Number
------------------------

This option changes the Aviatrix Transit Gateway ASN number before you setup Aviatrix Transit Gateway connection configurations. 

Connected Transit
------------------------

By default, Aviatrix Spoke VPCs/VNets do not have routing established to communicate 
with each other via Transit. They are completely segmented. 

If you would like to build a full mesh network where Spoke VPCs/VNets communicate with each other via Transit GW, you can achieve that by enabling Connected Transit mode. All connections are encrypted. 

.. Note::

  For a Spoke VPC/VNet in a multi-cloud transit to communicate with a Spoke VPC in TGW Orchestrator, connected Transit must be enabled on the Aviatrix Transit Gateway that connects both sides.

For software version 4.1 and later, click Multi-Cloud Transit on the left sidebar > Advanced Config > Edit Gateway tab. Select the Transit Gateway for which you want to enable the Connected Transit.

Note all Spokes should be either in HA mode or non HA mode. A mixed deployment where some Spokes have 
HA enabled while others don't work in a normal environment, but does not work
when a failover happens on a HA enabled Spoke. 

Advertise Transit VPC/VNet Network CIDR(s)
--------------------------------------

This field is only applicable to Transit GW established by `Transit Network workflow <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html>`_.

By default, Aviatrix Transit GW does not advertise Transit VPC/VNet `CIDR <https://www.aviatrix.com/learning/glossary/cidr.php>`_.

When this feature is enabled, Aviatrix Transit GW advertises the Transit VPC/VNet CIDR to VGW. The Controller programs the 3 RFC1918 routes in the AWS route table to point to the Transit GW. It also programs the learned routes from VGW into the AWS route table. 

If you deploy instances in the Transit VPC/VNet, enabling "Advertise Transit VPC CIDR(s) mode allows the instance to communicate both to Spoke VPCs and the on-prem network, assuming the Spoke VPCs are in the RFC1918 range. 

To enable this option in software version prior to 4.1, click Site2Cloud on the left navigation bar, select the connection established by `Step 3 <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html#connect-the-transit-gw-to-aws-vgw>`_, click to edit.
Scroll down to Advertise Transit VPC Network CIDR(s) to enable.

For software version 4.1 and later, click Multi-Cloud Transit on the left sidebar > Advanced Config > Edit Gateway tab. Select the Transit Gateway you want and scroll down to enable the Advertise Transit VPC Network CIDR(s) option. 

BGP ECMP
----------------

This option is to enable Equal Cost Multi Path (ECMP) routing for the next hop. For the Aviatrix Transit Gateway next hop routing decision
process, refer to `ActiveMesh 2.0 next hop. <https://docs.aviatrix.com/HowTos/activemesh_faq.html#what-is-activemesh-2-0>`_.

Click the Slide Bar to enable BGP ECMP. 

Site2Cloud RX Balancing
----------------------------
.. Note::

  This option is only available for Aviatrix Transit Gateways deployed in AWS on C5 and C5n instance types (except for c5.large and c5n.large). 

The Site2Cloud RX Balancing option can increase forwarding throughput on Aviatrix Transit gateways for BGP-over-GRE `External Device <https://docs.aviatrix.com/HowTos/transitgw_external.html>`_ traffic (a.k.a. Site2Cloud or S2C GRE tunnels), in these situations:

* On certain topologies that require high throughput, with External Devices that limit the number of GRE tunnels. 
* Where maintaining a high number of GRE tunnels increases operational burden.

If enabled, this option ensures that the Aviatrix Transit Gateway(s) are configured to maximize RX capacity and distribute ingress GRE tunnel load to all available vCPUs. This is mainly an alternative to `building a large number of GRE tunnels <https://docs.aviatrix.com/HowTos/transit_gateway_external_device_bgp_over_gre_high_performance_workflow.html>`_, but a greater number of tunnels will be needed if the External Device imposes per-tunnel rate limits. A brief (sub-second) period of packet loss may affect the gateway when this setting is enabled or disabled.

To maximize the forwarding throughput increase enabled by this setting, consider the following:

* The number of vCPUs provisioned for the Aviatrix Transit Gateway(s) should be significantly higher than the number of GRE tunnels (for example, four GRE tunnels to a 16 vCPUs c5n.4xlarge instance).
* High Performance Encryption (HPE) should be enabled between Aviatrix Transit Gateways.
* BGP ECMP should be enabled, to ensure load balancing of return traffic over multiple tunnels.

Active-Standby
-------------------

This option is to provide the flexibility on Aviatrix Transit Gateways and Aviatrix BGP Spoke Gateways to connect to on-prem with only one active tunnel and the other one as backup. In addition, this Active-Standby Mode supports ActiveMesh 2.0 only.

The use case is a deployment scenario where on-prem device such as firewall does not support asymmetric routing on two tunnels. When Active-Standby mode is enabled, it applies to both BGP and Static Remote Route Based External Device Connections and for each connection, only one tunnel is active in forwarding traffic at any given time. 

This feature can only be applied to non-HA remote devices in the `External Device section <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html#external-device>`_.

Click Active-Standby mode to set it to **Enabled**. 

Preemptive or Non-Preemptive Mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you enable Active-Standby mode, you can also select the **Preemptive** or **Non-preemptive** radio buttons to determine the network's behavior when the primary gateway goes down and the network switches to the standby gateway.

* In Preemptive mode, when the primary gateway for a connection is back up, the network automatically switches back to using that primary gateway.
* In Non-preemptive mode, the network continues to use the standby gateway even after the primary gateway is up again, until you initiate a manual switchover using the Switchover button.

.. Note::

If you enable Preemptive mode, the Switchover button is grayed out and unclickable because in Preemptive mode, there is no need for a manual switchover back to the primary gateway.

Multi-Tier Transit
-----------------------
Use the Multi-Cloud Transit Gateway option to implement a hierarchical transit gateway architecture that permits packets to traverse more than 2 Aviatrix transit gateways. In previous releases, full-mesh transit peering was required. You can now connect the two CSPs or regions through one peered connection. You must use ActiveMesh 2.0 to use multi-tier transit gateways, but full-mesh transit peering is not required.

Guidelines

* You can use Multi-Cloud Transit Gateway option with or without HPE.
* Inter and intra-region peering are both supported.
* Inter-CSP HPE over Internet is supported between AWS and Azure.
* AWS TGW peering is not supported.

Gateway Manual BGP Advertised Network List
---------------------------------------------------------

This field is only applicable to Transit GW established by `Transit Network workflow <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html>`_.

By default, Aviatrix Transit GW advertises individual Spoke VPC/VNet CIDRs to VGW. You can 
override that by manually entering the intended CIDR list to advertise to VGW. 

This feature is critical to limit the total number of routes carried by VGW (maximum is 100). 

To enable this option in software version prior to 4.1, on the left sidebar, select the connection established by `this step <https://docs.aviatrix.com/HowTos/transitvpc_workflow.html#connect-the-transit-gw-to-aws-vgw>`_, and click to edit.
Scroll down to Manual BGP Advertised Network List to set it to **Enabled**.

For software version 4.1 and later, click Multi-Cloud Transit on the left sidebar > Advanced Config > Edit Gateway tab. Select the Transit Gateway you want to enable this feature on, scroll down to the Manual BGP Advertised Network List, and enter the summarized CIDRs that you want to advertise.

To disable the option, leave the field blank and click **Change**.

Connection Manual BGP Advertised Network List
-------------------------------------------------------------

Manual Advertise Routes per BGP Connection expands the existing gateway based manual advertising routes feature to apply it to each BGP connection. One use case is to have better route advertising control for each remote BGP peer.

To enable this option on software version 6.3, 

#. Click Multi-Cloud Transit on the left sidebar > Advanced Config > Edit Transit tab.
#. Select the Transit Gateway. 
#. Find the Connection Manual BGP Advertised Network List panel, select the connection name and fill the CIDRs to advertise in the Advertised Network List field.

To disable the option, leave the field blank and click **Change**.


Preserve AS Path
-----------------

This field is applicable to both Gateway Manual BGP Advertised Network List and Connection Manual BGP Advertised Network List. When disabled, behavior defaults to the AS path being stripped during BGP route advertisements from transit or spoke gateways to neighbors. When enabled, AS Path is preserved. Gateways will not advertise manual BGP advertised CIDRs if the CIDRs are no longer in the best route DB.

To enable AS path preservation for a transit gateway:

#. Click Multi-Cloud Transit > Advanced Config on the left sidebar.
#. On the Edit Transit tab, in the Transit Gateway list select the transit gateway on which to enable this feature.
#. Scroll down to Preserve AS Path and set the toggle switch to Enabled.

Similarly, to enable AS Path preservation for a spoke gateway, follow the same procedure as above but select the spoke gateway to enable this feature on from the Edit Spoke tab. Scroll down to Preserve AS Path and set the toggle switch to Enabled.


Gateway AS Path Prepend
-------------------------------------------

You can insert BGP AS_PATH on the Aviatrix Transit Gateway to customize the BGP AP_PATH field when it advertises to VGW or peer devices. For example, 
enter 65458, 65478 in the input field, these ASN will appear to the remote end. 

This configuration applies to all BGP peers of the Aviatrix Transit Gateway. 

If you don't configure this field, Transit Gateway only advertises its own ASN.

Connection AS Path Prepend
-------------------------------------

Customize AS Path Prepend by specifying AS PATH for each BGP connection. 
This feature applies to any dynamic connection and Transit Gateway peering connections on a selected Aviatrix Transit Gateway. 

BGP Polling Time (seconds)
------------------------------------

Aviatrix Transit Gateways report its BGP routes to the Controller periodically. By default, the periodic timer is 50 seconds. 
This polling time affects BGP route change convergence time. 

This option changes the default polling time. The range is 10 seconds to 50 seconds.

BGP Hold Time (seconds)
----------------------------------
Use the BGP Hold Time option to manually set the BGP holding time for your Aviatrix transit gateway. The hold time specifies how long a router waits for incoming BGP messages before it assumes the neighbor is dead. 

The Aviatrix Transit Gateway hold time is bound to the Aviatrix keep alive message time, which is always 1/3 of the hold time. By default, the Hold Time is 180 seconds, and the Keep Alive time is 60 seconds. The supported Hold Time range is 12 to 180 seconds. If the remote site has a shorter hold time, the shorter hold time is used for the gateway.

Refresh BGP Advertised Routes
---------------------------------------

This option reset BGP connection to the remote BGP peers. 

Use this option to enable new features such as Segmentation based BGP CIDR Advertisements where on-prem receives BGP advertisement
for networks on-prem has connection policy or in the same Security Domain. 

AWS TGW Edge Segmentation
---------------------------------------

Refer to `TGW Edge Segmentation <https://docs.aviatrix.com/HowTos/tgw_faq.html#what-is-edge-segmentation>`_ for details. 

TGW Edge Segmentation can be enabled at given time. Select a connection to enable or disable. 

Summarize CIDR(s) to AWS TGW
------------------------------------------

* Enable this setting to limit routes propagated to TGW to only 3 RFC1918 CIDRs and specific non-RFC1918 CIDRs. Limiting routes saves route propagation time.
* Leave this setting disabled (the default setting) to maintain better segmentation behavior without improving performance.

.. |Test| image:: transitvpc_workflow_media/SRMC.png
   :width: 5.55625in
   :height: 3.26548in

.. |TVPC2| image:: transitvpc_workflow_media/TVPC2.png
   :scale: 60%

.. |HAVPC| image:: transitvpc_workflow_media/HAVPC.png
   :scale: 60%

.. |VGW| image:: transitvpc_workflow_media/connectVGW.png
   :scale: 50%

.. |launchSpokeGW| image:: transitvpc_workflow_media/launchSpokeGW.png
   :scale: 50%

.. |AttachSpokeGW| image:: transitvpc_workflow_media/AttachSpokeGW.png
   :scale: 50%

.. |SpokeVPC| image:: transitvpc_workflow_media/SpokeVPC.png
   :scale: 50%

.. |transit_to_onprem| image:: transitvpc_workflow_media/transit_to_onprem.png
   :scale: 40%

.. |azure_native_transit2| image:: transitvpc_workflow_media/azure_native_transit2.png
   :scale: 30%

.. |transit_approval| image:: transitvpc_workflow_media/transit_approval.png
   :scale: 30%

.. disqus::
