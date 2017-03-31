Developer Specific Configs
==========================


TODO: If you have applied this package to a project with a web.config you need to copy the contents of the current web.config into a new file called web.template.config



The intention of Developer Specific Configs is to allow individual developers to have their own set of config settings 
during development without affecting the shared app or web.config.  The principle is that a project will have a default 
configuration in the web.template.config or app.config.  
The developer will have a set of transforms in a file that conforms to one of the following naming convention:
 - app.[MACHINE_NAME].config
 - app.[USER_NAME].config
 - web.[MACHINE_NAME].config
 - web.[USER_NAME].config

the transform is applied to the default configuration as part of the build process.

For more information on transforms see: http://msdn.microsoft.com/en-us/library/dd465326.aspx

Example transform file content:
===============================

<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <!-- See http://msdn.microsoft.com/en-us/library/dd465326.aspx for transformation syntax -->
  <!-- You can test transforms at https://webconfigtransformationtester.apphb.com/ -->

  <connectionStrings>
    <add name="DefaultConnection"
         connectionString="Server=localhost;Database=MyOwnDb;Trusted_Connection=True;Application Name=MyApplicaiton"
         providerName="System.Data.SqlClient"
         xdt:Transform="SetAttributes"
         xdt:Locator="Match(name)" />
  </connectionStrings>

  <appSettings>
    <add key="foo" value="bar"
         xdt:Locator="Match(key)"
         xdt:Transform="SetAttributes"/>
  </appSettings>
</configuration>
