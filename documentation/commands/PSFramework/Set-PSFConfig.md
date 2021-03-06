---
external help file: PSFramework-help.xml
Module Name: PSFramework
online version:
schema: 2.0.0
---

# Set-PSFConfig

## SYNOPSIS
Sets configuration entries.

## SYNTAX

### FullName (Default)
```
Set-PSFConfig [-FullName] <String> [[-Value] <Object>] [-Description <String>] [-Validation <String>]
 [-Handler <ScriptBlock>] [-Hidden] [-Default] [-Initialize] [-DisableValidation] [-DisableHandler]
 [-EnableException] [<CommonParameters>]
```

### Module
```
Set-PSFConfig [-Name] <String> [[-Module] <String>] [[-Value] <Object>] [-Description <String>]
 [-Validation <String>] [-Handler <ScriptBlock>] [-Hidden] [-Default] [-Initialize] [-DisableValidation]
 [-DisableHandler] [-EnableException] [<CommonParameters>]
```

## DESCRIPTION
This function creates or changes configuration values.
These can be used to provide dynamic configuration information outside the PowerShell variable system.

## EXAMPLES

### EXAMPLE 1
```
Set-PSFConfig -FullName 'MyModule.User' -Value "Friedrich" -Description "The user under which the show must go on."
```

Creates a configuration entry under the module "MyModule" named "User" with the value "Friedrich"

### EXAMPLE 2
```
Set-PSFConfig -Name 'mymodule.User' -Value "Friedrich" -Description "The user under which the show must go on." -Handler $scriptBlock -Initialize -Validation String
```

Creates a configuration entry ...
- Named "mymodule.user"
- With the value "Friedrich"
- It adds a description as noted
- It registers the scriptblock stored in $scriptBlock as handler
- It initializes the script.
This block only executes the first time a it is run like this.
Subsequent calls will be ignored.
- It registers the basic string input type validator
This is the default example for modules using the configuration system.
Note: While the -Handler parameter is optional, it is important to add it at the initial initialize call, if you are planning to add it.
Only then will the system validate previous settings (such as what a user might have placed in his user profile)

### EXAMPLE 3
```
Set-PSFConfig 'Company' 'ConfigLink' 'https://www.example.com/config.xml' -Hidden
```

Creates a configuration entry named "ConfigLink" in the "Company" module with the value 'https://www.example.com/config.xml'.
This entry is hidden from casual discovery using Get-PSFConfig.

### EXAMPLE 4
```
Set-PSFConfig 'Network.Firewall' '10.0.0.2' -Default
```

Creates a configuration entry named "Firewall" in the "Network" module with the value '10.0.0.2'
This is only set, if the setting does not exist yet.
If it does, this command will apply no changes.

## PARAMETERS

### -FullName
The full name of a configuration element.
Must be namespaced \<Module\>.\<Name\>.
The name can have any number of sub-segments, in order to better group configurations thematically.

```yaml
Type: String
Parameter Sets: FullName
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name
Name of the configuration entry.
If an entry of exactly this non-casesensitive name already exists, its value will be overwritten.
Duplicate names across different modules are possible and will be treated separately.
If a name contains namespace notation and no module is set, the first namespace element will be used as module instead of name.
Example:
-Name "Nordwind.Server"
Is Equivalent to
-Name "Server" -Module "Nordwind"

```yaml
Type: String
Parameter Sets: Module
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Module
This allows grouping configuration elements into groups based on the module/component they serve.
If this parameter is not set, the configuration element must have a module name in the name parameter (the first segment will be taken, irrespective of whether that makes sense).

```yaml
Type: String
Parameter Sets: Module
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Value
The value to assign to the named configuration element.

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description
Using this, the configuration setting is given a description, making it easier for a user to comprehend, what a specific setting is for.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Validation
The name of the validation script used for input validation.
These can be used to validate make sure that input is of the proper data type.
New validation scripts can be registered using Register-PSFConfigValidation

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Handler
A scriptblock that is executed when a value is being set.
Is only executed if the validation was successful (assuming there was a validation, of course)

```yaml
Type: ScriptBlock
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Hidden
Setting this parameter hides the configuration from casual discovery.
Configurations with this set will only be returned by Get-Config, if the parameter "-Force" is used.
This should be set for all system settings a user should have no business changing (e.g.
for Infrastructure related settings such as mail server).

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Default
Setting this parameter causes the system to treat this configuration as a default setting.
If the configuration already exists, no changes will be performed.
Useful in scenarios where for some reason it is not practical to automatically set defaults before loading userprofiles.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Initialize
Use this when setting configurations as part of module import.
When initializing a configuration, it will only do a thing if the configuration hasn't already been initialized (So if you load the module multiple times or in multiple runspaces, it won't make a difference)
Also, if there already was a non-initialized setting set for a given configuration, it will then try to set the old value again.
This value will be processed by handlers, if any are set.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableValidation
This parameters disables the input validation - if any - when processing a setting.
Normally this shouldn't be circumvented, but just in case, it can be disabled.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableHandler
This parameter disables the configuration handlers.
Configuration handlers are designed to automatically process input set to a config value, in addition to writing the value.
In many cases, this is used to improve performance, by forking the value location also to a static C#-field, which is then used, rather than searching a Hashtable.
Normally these shouldn't be circumvented, but just in case, it can be disabled.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableException
Replaces user friendly yellow warnings with bloody red exceptions of doom!
Use this if you want the function to throw terminating errors you want to catch.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.
For more information, see about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

## OUTPUTS

## NOTES

## RELATED LINKS
