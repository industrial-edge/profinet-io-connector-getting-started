# PROFINET IO Connector Getting Started: Documentation

- [PROFINET IO Connector Getting Started: Documentation](#profinet-io-connector-getting-started-documentation)
  - [Configure IED Layer 2 access](#configure-ied-layer-2-access)
  - [Configure PROFINET IO Connector](#configure-profinet-io-connector)
    - [TIA Portal HSP for PN Driver](#tia-portal-hsp-for-pn-driver)
    - [Configure PROFINET IO Connector in TIA Portal](#configure-profinet-io-connector-in-tia-portal)
    - [Configure PROFINET IO Configuration Files](#configure-profinet-io-configuration-files)
    - [Configure PROFINET IO with Binary format](#configure-profinet-io-with-binary-format)
    - [Configure PROFINET IO with JSON format](#configure-profinet-io-with-json-format)
    - [Configure User Credentials for IE Databus](#configure-user-credentials-for-ie-databus)
    - [Configure Application Settings](#configure-application-settings)
    - [Configure Tag Definition](#configure-tag-definition)
    - [Update All Configurations Files from Management (IEM)](#update-all-configurations-files-from-management-iem)
  - [Configure Databus and Data Service](#configure-databus-and-data-service)
    - [Configure Databus](#configure-databus)
    - [Configure Data Service](#configure-data-service)

## Configure IED Layer 2 access

The PROFINET IO Connector requires Layer 2 access within the IED to enable a communication with the PLC.

> [!IMPORTANT]  
> From version 1.3.0-57 of IED firmware (ied-os-1.3.0-57) the Layer 2 can be configured after onboarding of IED. Previous versions support setting Layer 2  **only** during onboarding. Both options are described in the following subsections.

### Configure IED Layer 2 access during onboarding of IED

Open the management system and select "My Edge Devices" on the left side in the bar and click on "+ New Edge Device" on the upper right side.

![Layer_2_configuration_onboarding_1](graphics/Layer_2_configuration_onboarding_1.png)

Configure your Edge Device and click on "Next".

![Layer_2_configuration_onboarding_2](graphics/Layer_2_configuration_onboarding_2.png)

Click on the "+" button at the top right to configure the network interface.

![Layer_2_configuration_onboarding_3](graphics/Layer_2_configuration_onboarding_3.png)

Configure the network interface and the layer 2 access and click on "Add".

![Layer_2_configuration_onboarding_4](graphics/Layer_2_configuration_onboarding_4.png)

Confirm the device configuration with "Next" and with "Create".

![Layer_2_configuration_onboarding_5](graphics/Layer_2_configuration_onboarding_5.png)

![Layer_2_configuration_onboarding_6](graphics/Layer_2_configuration_onboarding_6.png)

### Configure IED Layer 2 access of onboarded IED

In your IED go to the Settings, open the Connectivity tab and click on "LAN Network".

![Layer_2_configuration_after_onboarding_1](graphics/Layer_2_configuration_after_onboarding_1.png)

Go to the edditing window of your IED network adapter by clicking on pencil icon.

![Layer_2_configuration_after_onboarding_2](graphics/Layer_2_configuration_after_onboarding_2.png)

Configure the layer 2 access and click on "Update".

![Layer_2_configuration_after_onboarding_3](graphics/Layer_2_configuration_after_onboarding_3.png)

## Configuration of Databus

First of all, make sure that you have installed
- Databus Configurator application in your IEM Maintenance
- Databus application in your IEM Management

![Databus_apps_1](graphics/Databus_apps_1.png)

![Databus_apps_2](graphics/Databus_apps_2.png)

Then, in your IEM go to the Data Connections and open the Databus. Add a user with following topic: `"ie/#"` and deploy it to IED (or IEVD).

![Databus_configuration_1](graphics/Databus_configuration_1.png)

![Databus_configuration_2](graphics/Databus_configuration_2.png)

![Databus_configuration_4](graphics/Databus_configuration_4.png)

## Configure PROFINET Driver in TIA Portal

In order to manage a communication between PLC and IED, the PROFINET Driver should be configured in TIA Portal.

> [!IMPORTANT]  
> If the TIA Portal V16 or older is used, the PNDriver V2.2 is not included automatically in there. Therefore, the HSP (Hardware Support Package) has to be installed. You can download the needed HSP 0307 from the Siemens support pages [↗ID 72341852](https://support.industry.siemens.com/cs/ww/en/view/72341852).

![TIA_Portal_HSP_PN_IO_Driver](graphics/TIA_Portal_HSP_PN_IO_Driver.png)

By double clicking open "Devices & Network" at the top of the left side. Then, select the PROFINET Driver from the catalog, drag it and drop it into the "Network view" field.

![TIA_Portal_PN_IO_Driver_configuration_1](graphics/TIA_Portal_PN_IO_Driver_configuration_1.png)

Next, you have to add Linux native communication interface to the PROFINET Driver. Drag in and drop into the PROFINET interface of the PROFINET Driver in "Device view" field.

![TIA_Portal_PN_IO_Driver_configuration_2](graphics/TIA_Portal_PN_IO_Driver_configuration_2.png)

The project contains now a PC station with prepared PROFINET Driver. Switch to the Network View and connect the PLC with the PROFINET Driver. Ensure that IP address of PROFINET Driver is inside IED subnet and outside of L2 subnet. Then, click on the PLC properties. Go to "PROFINET interface [X1]" and inside Operation mode, select IO device. Also, assign PROFINET driver to IO controller.

![TIA_Portal_PN_IO_Driver_configuration_3](graphics/TIA_Portal_PN_IO_Driver_configuration_3.png)

Click on "I-device communication" in the PLC properties, add a new transfer area and name it. Inside newly created transfer area, define the address type (inputs, outputs), starting address and the length of area. In our case, output variables from PLC are inputs to PROFINET Driver. This way, selected PLC memory becomes available for access through PROFINET network.

![TIA_Portal_PN_IO_Driver_configuration_4](graphics/TIA_Portal_PN_IO_Driver_configuration_4.png)

Check the transfer area from the I-Device communication.

![TIA_Portal_PN_IO_Driver_configuration_5](graphics/TIA_Portal_PN_IO_Driver_configuration_5.png)

The output variables for the PROFINET IO controller should be defined in the tag table. Open Default tag table of PLC. Create output variable `SignalsOut` with custom data type `typeSignals`. In Address field, enter the starting address of tag. Pay attention that the transfer area has enough bytes to store all variables.

![TIA_Portal_PN_IO_Driver_configuration_6](graphics/TIA_Portal_PN_IO_Driver_configuration_6.png)

Next, this tag needs to be populated. So, in PLC program, assign the respective values to the output variables (e.g. in OB1). Here, everything from `"GDB".signals` is copied to `SignalsOut`.

![TIA_Portal_PN_IO_Driver_configuration_7](graphics/TIA_Portal_PN_IO_Driver_configuration_7.png)

Finally, compile the PROFINET Driver to create the XML configuration file. This file you have to provide later during the configuration of the PROFINET IO Connector application. You can get to the file storage by clicking to green arrow.

![TIA_Portal_PN_IO_Driver_compilation](graphics/TIA_Portal_PN_IO_Driver_compilation.png)

When all of the steps above are done, you can download the project to the PLC. Be aware that only physical PLC can be used. The PROFINET doesn't work on simulated PLC.

## Configure PROFINET IO Connector

There are two ways how to configure the PROFINET IO Connector:

1. [Directly in IED](Configuration_IED.md) - using Common Configurator application (newest and preferable way)
2. [In IEM](Configuration_IEM.md) - using PROFINET IO Configuration Files (older and more complicated way)
