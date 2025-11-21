# Empyrion Scripting

Script collections for Empyrion Scripting mod.

## ⚠️ Important Notes for Script Creation

### LCD Naming Convention
- **Script LCD**: Must be named `Script:YourScriptName` (with `Script:` prefix)
- **Output LCD**: Must be named exactly `YourScriptName` (without prefix)
- Example: Script LCD `Script:ItemSorter` → Output LCD `ItemSorter`

### SaveGame vs In-Game Scripts
- **SaveGame scripts**: Place `.hbs` files in `[Savegame]\Mods\EmpyrionScripting\Scripts\`
  - LCD name = script filename (without `.hbs`)
  - Runs automatically on server
- **In-Game scripts**: Code written directly in LCD
  - Script LCD: `Script:ScriptName`
  - Output LCD: `ScriptName`

### Performance & Optimization
- **Disable scripts when not needed**: Toggle Script LCD on/off to save server/PC performance
- Use TX Signal from switch/sensor to control Script LCD activation
- Scripts with `move` or `fill` operations should be disabled when not actively sorting

### Syntax Rules
- **Every `{{#command}}` must be closed with `{{/command}}`**
- Commands can be nested, but must be closed in reverse order
- Use `{{~#command}}` (tilde) to output on same line without newline
- Use `{{~command}}` to suppress whitespace

### Structure Requirements
- Structures must be **added to a group** and have a **unique name**
- Scripts work on all structure types (BA, CV, HV, SV) if properly named

### Wildcards
- Use `*` in device names: `'Container*'` matches all containers starting with "Container"
- Useful for multiple devices: `'Box-*'` matches `Box-1`, `Box-2`, etc.

### Logging Output
- Use `Script:[+N]Name` format to show last N lines of output
- Example: `Script:[+6]ReFuel` shows last 6 lines (useful for move/fill operations)

---

## Script Collections

- [olliewheeler/empyrionscripts](https://github.com/olliewheeler/empyrionscripts)
- [sri-arjuna/Empyrion-Scripts](https://github.com/sri-arjuna/Empyrion-Scripts)
- [GitHub-TC/EmpyrionScripting-Collection](https://github.com/GitHub-TC/EmpyrionScripting-Collection)

---

## Syntax Reference

### Handlebars Block Helpers

#### Data Access
- `{{#items E.S 'ContainerName'}}` - Access items in a container
- `{{#devices E.S 'DeviceName*'}}` - Access devices by name (supports wildcards)
- `{{#devicesoftype E.S 'Type1,Type2'}}` - Access devices by type
- `{{#each}}` - Iterate over arrays/objects
- `{{#itemlist E.S.Items 'IDList'}}` - Check structure items against ID list

#### Conditional Logic
- `{{#if condition}}` - Conditional block
- `{{#unless condition}}` - Negative conditional block
- `{{#test value operator 'target'}}` - Test conditions (eq, ne, gt, ge, lt, le, in)
- `{{#ok variable}}` - Check if variable exists and is not empty
- `{{else}}` - Alternative block

#### Item Operations
- `{{#move item E.S 'Destination' amount}}` - Move items between containers
- `{{#fill tank E.S 'Destination' amount}}` - Fill tanks
- `{{#additems container itemid count}}` - Add items to container

#### Variables
- `{{#set 'VariableName' value}}` - Set variable
- `{{#use expression}}` - Use expression result

#### String Operations
- `{{#split string separator}}` - Split string into array
- `{{#concat var1 var2}}` - Concatenate strings/variables
- `{{#lookup array index}}` - Lookup value in array by index

#### Calculations
- `{{#calc value1 operator value2}}` - Calculate (+, -, *, /, %)
- `{{#math variable operator value}}` - Math operations on variables

#### Configuration
- `{{#configbyid Id}}` - Get item configuration by ID
- `{{#configbyname 'Name'}}` - Get item configuration by name
- `{{configattr Id 'AttributeName'}}` - Get specific attribute
- `{{configid value}}` - Get configuration ID

#### Display Control
- `{{#scroll lines speed}}` - Scrollable content block
- `{{#settext device 'text'}}` - Set text on device (single line)
- `{{#settextblock device}}` - Set text block on device (multi-line)
- `{{#gettext device}}` - Get text from device

#### File Operations (SaveGame scripts)
- `{{#readfile path}}` - Read file
- `{{#writefile path content}}` - Write file
- `{{#fileexists path}}` - Check if file exists

#### Database
- `{{#db 'QueryName'}}` - Execute SQL query

#### Block Operations
- `{{#block E.S x y z}}` - Access block at coordinates
- `{{#islocked E.S device}}` - Check if device is locked

### Functions

#### Localization
- `{{i18n Id}}` - Get localized item name
- `{{i18n Id 'Language'}}` - Get localized name in specific language

#### Date/Time
- `{{datetime}}` - Current date/time
- `{{datetime 'format'}}` - Formatted date/time

#### Formatting
- `{{format value 'formatString'}}` - Format numbers/values
- `{{bar current min max length}}` - Progress bar
- `{{bar current min max length 'filled' 'empty'}}` - Custom progress bar

### HTML/CSS Tags

#### Text Formatting
- `<color=#hex>` - Text color
- `<size=value>` - Font size
- `<align=left|center|right>` - Text alignment
- `<pos=value>` - Horizontal position
- `<line-height=value>` - Line height
- `<bgcolor=#hex>` - Background color
- `<fontsize=value>` - Font size

### Variables

#### Entity Variables
- `E.S` - Structure
- `E.Id` - Entity ID
- `E.Name` - Entity name
- `E.Pos` - Position
- `E.Forward` - Forward vector
- `E.BelongsTo` - Owner
- `E.DockedTo` - Docked entity
- `E.IsLocal` - Is local
- `E.IsPoi` - Is POI
- `E.IsProxy` - Is proxy
- `E.Faction` - Faction information

#### Item Variables
- `Id` - Item ID
- `ItemId` - Item ID (alternative)
- `Name` - Item name
- `Count` - Item count
- `Source` - Source container name
- `Destination` - Destination container name
- `SourceE.Name` - Source entity name
- `DestinationE.Name` - Destination entity name

#### Device Variables
- `CustomName` - Device custom name
- `Type` - Device type
- `Active` - Device active state

#### Context Variables
- `@root` - Root context
- `@index` - Current index in loop
- `@key` - Current key in object
- `@first` - First item in loop
- `@last` - Last item in loop

#### Configuration Variables
- `Values` - Configuration values
- `EcfValues` - ECF configuration values
- `Attr` - Attributes array
- `Childs` - Child elements
- `AddOns` - Additional properties

#### Data Variables
- `@root.Data.VariableName` - Access stored variable
- `@root.E.S` - Access structure from root

### Preset ID Lists

- `{{Ids.Ore}}` - Ore items
- `{{Ids.Ingot}}` - Ingot items
- `{{Ids.BlockL}}` - Large blocks
- `{{Ids.BlockS}}` - Small blocks
- `{{Ids.Medic}}` - Medical items
- `{{Ids.Food}}` - Food items
- `{{Ids.Ingredient}}` - Ingredient items
- `{{Ids.Sprout}}` - Sprout items
- `{{Ids.Tools}}` - Tool items
- `{{Ids.ArmorMod}}` - Armor mod items

### Special Syntax

#### Tilde Operator
- `{{~#command}}` - Output on same line (no newline)
- `{{~command}}` - Suppress whitespace

#### Wildcards
- `'DeviceName*'` - Match devices starting with name
- `'*DeviceName'` - Match devices ending with name
- `'*DeviceName*'` - Match devices containing name

#### Operators (for #test)
- `eq` - Equal
- `ne` - Not equal
- `gt` - Greater than
- `ge` - Greater or equal
- `lt` - Less than
- `le` - Less or equal
- `in` - In list/range

#### ID Ranges
- `'100-200'` - Range of IDs
- `'100,200,300'` - List of IDs
- `'100-200,300,400-500'` - Combined ranges and lists
