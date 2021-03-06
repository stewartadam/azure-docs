---
title: Run a disaster recovery drill for on-premises machines to Azure with Azure Site Recovery | Microsoft Docs
description: Learn about running disaster recovery drill from on-premises to Azure, with Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/02/2018
ms.author: raynew

---
# Run a disaster recovery drill to Azure

This tutorial shows you how to run a disaster recovery drill for an on-premises machine to Azure,
using a test failover. A drill validates your replication strategy without data loss. In this
tutorial, you learn how to:

> [!div class="checklist"]
> * Set up an isolated network for the test failover
> * Prepare to connect to the Azure VM after failover
> * Run a test failover for a single machine

This is the fourth tutorial in a series. This tutorial assumes that you have already completed the
tasks in the previous tutorials.

1. [Prepare Azure](tutorial-prepare-azure.md)
2. [Prepare on-premises VMware](tutorial-prepare-on-premises-vmware.md)
3. [Set up disaster recovery](tutorial-vmware-to-azure.md)

## Verify VM properties

Before you run a test failover, verify the VM properties, and make sure that the Hyper-V VM[hyper-v-azure-support-matrix.md#replicated-vms], [VMware VM or physical server](vmware-physical-azure-support-matrix.md#replicated-machines) complies with Azure requirements.

1. In **Protected Items**, click **Replicated Items** > VM.
2. In the **Replicated item** pane, there's a summary of VM information, health status, and the
   latest available recovery points. Click **Properties** to view more details.
3. In **Compute and Network**, you can modify the Azure name, resource group, target size,
   [availability set](../virtual-machines/windows/tutorial-availability-sets.md), and managed disk
   settings.
   
      >[!NOTE]
      Failback to on-premises Hyper-V machines from Azure VMs with managed disks isn't currently supported. You should only use the managed disks option for failover if you're planning to migrate on-premises VMs to Azure, without failing them back.
   
4. You can view and modify network settings, including the network/subnet in which the Azure VM
   will be located after failover, and the IP address that will be assigned to it.
5. In **Disks**, you can see information about the operating system and data disks on the VM.

## Run a test failover for a single VM

When you run a test failover, the following happens:

1. A prerequisites check runs to make sure all of the conditions required for failover are in
   place.
2. Failover processes the data, so that an Azure VM can be created. If select the latest recovery
   point, a recovery point is created from the data.
3. An Azure VM is created using the data processed in the previous step.

Run the test failover as follows:

1. In **Settings** > **Replicated Items**, click the VM > **+Test Failover**.
2. Select the **Latest processed** recovery point for this tutorial. This fails over the VM to the latest available point in time. The time stamp is shown. With this option, no time is spent processing data, so it provides a low RTO (recovery time objective).
3. In **Test Failover**, select the target Azure network to which Azure VMs will be connected after
   failover occurs.
4. Click **OK** to begin the failover. You can track progress by clicking on the VM to open its
   properties. Or you can click the **Test Failover** job in vault name > **Settings** > **Jobs** >
   **Site Recovery jobs**.
5. After the failover finishes, the replica Azure VM appears in the Azure portal > **Virtual
   Machines**. Check that the VM is the appropriate size, that it's connected to the right network,
   and that it's running.
6. You should now be able to connect to the replicated VM in Azure.
7. To delete Azure VMs created during the test failover, click **Cleanup test failover** on the
  VM. In **Notes**, record and save any observations associated with the test failover.

In some scenarios, failover requires additional processing that takes around eight to ten minutes
to complete. You might notice longer test failover times for VMware Linux machines, VMware VMs that
don't have the DHCP service enables, and VMware VMs that don't have the following boot drivers:
storvsc, vmbus, storflt, intelide, atapi.

## Next steps

> [!div class="nextstepaction"]
> [Run a failover and failback for on-premises VMware VMs](vmware-azure-tutorial-failover-failback.md).
