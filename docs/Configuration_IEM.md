 - [TIA Portal HSP for PN Driver](#tia-portal-hsp-for-pn-driver)
    - [Configure PROFINET IO Configuration Files](#configure-profinet-io-configuration-files)
    - [Configure PROFINET IO with Binary format](#configure-profinet-io-with-binary-format)
    - [Configure PROFINET IO with JSON format](#configure-profinet-io-with-json-format)
    - [Configure User Credentials for IE Databus](#configure-user-credentials-for-ie-databus)
    - [Configure Application Settings](#configure-application-settings)
    - [Configure Tag Definition](#configure-tag-definition)
    - [Update All Configurations Files from Management (IEM)](#update-all-configurations-files-from-management-iem)
  - [Configure Databus and Data Service](#configure-databus-and-data-service)
    
    - [Configure Data Service](#configure-data-service)
   
## Prepare configuration files




## Update PROFINET IO Connector configuration in IEM



To read data from the PLC and provide the data, we will use PROFINET IO Connector to establish connection a with the PLC via PROFINET.

The PROFINET IO Connector sends the data to the Databus, where the Data Service app can collect what is needed.

In order to build this infrastructure, these parts must be configured properly:

- TIA Portal HSP for PN Driver
- Databus
- Data Service
- PROFINET IO Connector

Note: When installing the PROFINET IO connector, no configuration files need to be selected. These are configured later [Configure PROFINET IO Configuration Files](#configure-profinet-io-configuration-files) and uploaded to the app.

The PROFINET IO Connector app can be adjusted according to the project's needs. Configuration files enable to configure the behavior of the app.

An example of the required PROFINET IO Connector files can be downloaded [here](https://support.industry.siemens.com/cs/us/en/view/109793251) under "Appendix":

- User credentials for IE Databus (pn_hs_adpt_credentials.xml)
- Application settings (pn_hs_adpt_appconfig.xml)
- Profinet configuration (e.g. gerated from TIA Portal)
- optional Tag Definition file (pn_hs_adpt_tagdefs.json)


### Configure PROFINET IO Configuration Files

You have a choice in the PROFINET IO Connector between binary and JSON format. You must use the downloaded example configuration files for the respective format.

The PROFINET IO Connector application requires four configuration files that are available in the JSON or Binary folder:

- User credentials for IE Databus (pn_hs_adpt_credentials.xml)
- Application settings (pn_hs_adpt_appconfig.xml)
- Profinet configuration (e.g. generated from TIA Portal)
- optional Tag Definition File (pn_hs_adpt_tagdefs.json)

![PROFINET_IO_Configurations_Files](graphics/PROFINET_IO_Configurations_Files.PNG)

### Configure PROFINET IO with Binary format

The binary format is designed for higher performance.

For the binary format, the respective binary files must be uploaded in the Profinet IO Connector.

![PROFINET_IO_Configurations_Binary_File](graphics/PROFINET_IO_Configurations_Binary_File.PNG)

### Configure PROFINET IO with JSON format

The JSON format is for easier handling on the client side.

For the JSON format, the respective JSON files must be uploaded in the Profinet IO Connector.

![PROFINET_IO_Configurations_JSON_File](graphics/PROFINET_IO_Configurations_JSON_File.PNG)

### Configure User Credentials for IE Databus

The XML file pn_hs_adpt_appconfig.xml shows user and password for the IE Databus (MQTT broker). This configuration must match the IE Databus configuration.

![PROFINET_IO_Configuration_user_cred_file](graphics/PROFINET_IO_Configuration_user_cred_file.PNG)

### Configure Application Settings

The XML file pn_hs_adpt_appconfig.xml contains several parameters for the PROFINET IO Connector app. Every line has a comment. You can adjust the parameters according your needs, e.g.

- name of the PN config file, created by TIA Portal
- name of the optional tag defintion file
- MQTT topic names for published and subsribed topics
- Cycle time for reading the PN IO data
- Oversampling factor (how many PN IO cycles are transferred with one MQTT message)

![PROFINET_IO_Configuration_appconfig_file](graphics/PROFINET_IO_Configuration_appconfig_file.PNG)

### Configure Tag Definition

The XML file pn_hs_adpt_tagdefs.json shows the configured Tags. This configuration must match the tag table configuration.

![Tag_Definition_file](graphics/Tag_Definition_file.PNG)

Properties for tag definition

![PROFINET_IO_Configuration_properties_tag_definition](graphics/PROFINET_IO_Configuration_properties_tag_definition.PNG)

The supported data types for tag definitions are as follows:

- SimpleTagTypeBool
- SimpleTagTypeByte
- SimpleTagTypeWord
- SimpleTagTypeDWord
- SimpleTagTypeLWord
- SimpleTagTypeSInt
- SimpleTagTypeInt
- SimpleTagTypeDInt
- SimpleTagTypeLInt
- SimpleTagTypeUSInt
- SimpleTagTypeUInt
- SimpleTagTypeUDInt
- SimpleTagTypeULInt
- SimpleTagTypeReal
- SimpleTagTypeLReal

### Update All Configurations Files from Management (IEM)

You can download all or selected config files with the Management (IEM).
IEM >> My Installed Apps >> PROFINET IO Connector >> Update Configuration

Hint: When you change any config file, you have to restart the app (e.g. via the Web UI of the IEM) to activate the changed configuration!

![PROFINET_IO_Configurations_Files_upload](graphics/PROFINET_IO_Configurations_Files_upload.PNG)

## Configure Databus and Data Service

### Configure Databus

In your IEM open the Databus and launch the configurator.

Add a user with this topic:
`"ie/d/b/simatic/v1/pnhs1/dp/r"`
`"ie/m/j/simatic/v1/pnhs1/dp/r"`

![ie_databus_user](graphics/IE_Databus_User.PNG)

![ie_databus](graphics/IE_Databus.PNG)

Deploy the configuration.

### Configure Data Service

Open the Data Service in the IED.

Click on adapters and chosse the PROFINET IO Connector:

![Data_Service_Adapter](graphics/Data_Service_Adapters_Profinet_IO_Connctor.PNG)

Take the settings what you have used in the Databus:

![Data_Service_PROFINET_IO_Connector](graphics/Data_Service_PROFINET_IO_Connector.PNG)

Activate the adapter for PROFINET IO Connector:

![Data_Service_Adapter](graphics/Data_Service_Adapters.PNG)

Click on "Assets & Connectivity" at the top of the left-hand page and create your first variables.

Select the PROFINET IO Connector in "Choose an Adapter" and all "profinetxadriver" in "choose a tag":

![Data_Service_Variables](graphics/Data_Service_Data_Service_Variable.PNG)

Click on "Aspects" and select variables if you want to display the variables e.g. in Performance Insight.

![Data_Service_Aspects](graphics/Data_Service_Data_Service_Aspects.PNG)

