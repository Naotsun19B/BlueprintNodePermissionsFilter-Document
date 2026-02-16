![UE Version](https://img.shields.io/badge/Unreal%20Engine-5.4--5.7-0e1128?logo=unrealengine&logoColor=white)
[![License: Fab Standard License (Fab EULA)](https://img.shields.io/badge/License-Fab%20Standard%20License%20%28Fab%20EULA%29-blue)](https://www.fab.com/eula)
[![X (formerly Twitter) Follow](https://img.shields.io/twitter/follow/Naotsun_UE?style=social)](https://twitter.com/Naotsun_UE)

# BlueprintNodePermissionsFilter

<img width="1873" height="101" alt="Image" src="https://github.com/user-attachments/assets/adca207b-eed4-4c33-b7a1-f1a3f5c314f7" />

<!--ts-->
* [Description](#description)
* [Requirements](#requirements)
* [Installation](#installation)
* [Quick Start](#quick-start)
* [Features and Usage](#features-and-usage)
    * [Group System](#group-system)
        * [Administrator Group](#administrator-group)
        * [Unknown Group](#unknown-group)
        * [Custom Groups](#custom-groups)
        * [Group Hierarchy](#group-hierarchy)
        * [Edit Scope](#edit-scope)
    * [Filter System](#filter-system)
        * [Filter State](#filter-state)
        * [Supported Node Types](#supported-node-types)
    * [How to Use the UI](#how-to-use-the-ui)
        * [Group Tree View](#group-tree-view)
        * [Filter Tree View](#filter-tree-view)
        * [Group Selector Dialog](#group-selector-dialog)
    * [Shortcut Keys](#shortcut-keys)
* [Options](#options)
    * [Group Settings](#group-settings)
    * [Per-Group Settings](#per-group-settings)
    * [Filter Settings](#filter-settings)
    * [Frame-Distributed Task Manager Settings](#frame-distributed-task-manager-settings)
    * [Personal Settings](#personal-settings)
* [License](#license)
* [Author](#author)
* [History](#history)
<!--te-->

## Description

This plugin adds the ability to manage which nodes are shown in the Blueprint Editor Action Menu (right-click context menu) **per group**, such as by job role.  
For example, you can hide high-cost engine-standard nodes from team members and enforce the use of your own lightweight nodes instead.

<img width="2542" height="971" alt="Image" src="https://github.com/user-attachments/assets/8161d2b3-5d51-4f3b-99b6-192b8d7ba45b" />
<img width="2540" height="1042" alt="Image" src="https://github.com/user-attachments/assets/cf81f67c-ec24-42a5-8689-9a81981436b0" />
<img width="1211" height="523" alt="Image" src="https://github.com/user-attachments/assets/fb11e2b7-6f29-480d-af0b-d7cb754374bb" />

## Requirements

Target versions: UE 5.4 – 5.7  
Supported platforms: Windows, Linux

## Installation

This is a paid plugin distributed via Fab only.  
The Fab product page will be enabled after the review is completed.  

1. Install the plugin from [Fab](https://www.fab.com/en-US/listings/74869306-565b-4051-a590-fd2d81ab2386) (once enabled).  
2. If the feature is not available after installing the plugin, enable it in `Edit > Plugins` and restart the editor.  

## Quick Start

After installation is complete, you can check the features of the plugin by following the steps below.  

1. Open any project in Unreal Editor.  
2. Go to `Edit > Editor Preferences > Blueprint Node Permissions Filter > Group Settings`.  
3. Create any group in the group tree view.  
4. Change your affiliation from the Affiliated Group to the group you created.  
5. Go to `Edit > Editor Preferences > Blueprint Node Permissions Filter > [Created Group Name]`.  
6. Set the filter state of any node to `Deny`.  
7. Open any Blueprint graph and right-click to open the action menu.  
8. Confirm that the nodes belonging to the denied scope are hidden from the action menu.  

## Features and Usage

### Group System

This plugin manages Blueprint node visibility permissions **per group**.  
Groups have a hierarchical structure, allowing you to provide different node sets depending on team roles (programmer, designer, etc.).

#### Administrator Group

The Administrator Group is the root group built into the plugin. It has the following characteristics:

- It is at the top of the hierarchy and has no parent group.
- All nodes are visible and no filtering is applied.
- It has edit permissions for all groups (`AllGroups`).

#### Unknown Group

The Unknown Group is a placeholder group temporarily assigned, such as on first launch or when the group previously selected by the user has been deleted. It has the following characteristics:

- All nodes are filtered out, so no nodes appear in the Blueprint Editor Action Menu.
- Its edit scope is `ReadOnly`, so you cannot change filter settings or group structure.
- It cannot have child groups.

While the Unknown Group is selected, the Group Selector Dialog is shown, prompting the user to reselect a valid group.

#### Custom Groups

Groups created by users. Each custom group has the following information:

| **Property**            | **Description**                                                                 |
|-------------------------|---------------------------------------------------------------------------------|
| Group Name              | A unique identifier name for the group                                          |
| Group Display Name      | The display name shown in the UI                                                |
| Group Description       | A description of the group                                                      |
| Edit Scope              | The edit permission level for this group                                         |
| Is Filtered Per Filter  | A map of allow/deny settings per filter GUID                                    |

Custom group settings are saved as INI files under:  
`[Project]/Config/BlueprintNodePermissionsFilter/`  
Child groups are stored in subdirectories.

#### Group Hierarchy

Groups are managed as a tree structure with parent-child relationships.  
You can freely place custom groups under the Administrator Group (root).  
Child groups inherit the parent group’s filter settings. By setting a filter state to `Inherit`, the child will carry over the parent’s settings as-is.

#### Edit Scope

Each group has an Edit Scope that controls who can edit filter settings:

| **Edit Scope**        | **Description**                                                             |
|------------------------|-----------------------------------------------------------------------------|
| ReadOnly               | Filter settings cannot be edited                                            |
| SelectedGroupOnly      | Only this group’s filter settings can be edited                             |
| ChildGroupsOnly        | This group and its child groups’ filter settings can be edited              |
| AllGroups              | All groups’ filter settings and hierarchy structure can be edited           |

### Filter System

The Filter System controls whether nodes are shown/hidden in the Blueprint Editor Action Menu.

#### Filter State

Each filter can be set to one of three states:

| **Filter State** | **Description**                                         |
|------------------|---------------------------------------------------------|
| Allow            | Show the node (allow)                                   |
| Deny             | Hide the node (deny)                                    |
| Inherit          | Inherit the parent group’s filter setting               |

#### Supported Node Types

This plugin provides filters for the following Blueprint node types:

| **Filter Name**                | **Target Nodes**                              |
|-------------------------------|-----------------------------------------------|
| Function                      | Function call nodes                            |
| Event                         | Event / Event Dispatcher nodes                 |
| Bound Event                   | Bound Event nodes                               |
| Delegate                      | Delegate nodes                                  |
| Variable                      | Variable Get/Set nodes                          |
| Struct                        | Struct nodes                                     |
| Instanced Struct              | Instanced Struct nodes                           |
| Enum                          | Enum nodes                                       |
| Dynamic Cast                  | Type cast nodes                                  |
| Input Event                   | Input Event nodes                                |
| Input Key                     | Input Key nodes                                  |
| Input Axis Key                | Input Axis Key nodes                             |
| Input Debug Key               | Debug Input nodes                                |
| Get Input Axis Key Value      | Get Input Axis Key Value nodes                   |
| Get Subsystem                 | Subsystem access nodes                           |

For nodes that do not match the above, a default fallback filter is applied.  
Because the fallback filter determines uniqueness by function name, it is recommended to implement additional filters for custom nodes that do not fall into the categories above.

### How to Use the UI

#### Group Tree View

<img width="2542" height="971" alt="Image" src="https://github.com/user-attachments/assets/8161d2b3-5d51-4f3b-99b6-192b8d7ba45b" />

The Group Tree View is the UI for displaying and managing the group hierarchy. It consists of the following columns:

| **Column**        | **Description**                                 |
|-------------------|-------------------------------------------------|
| Group Name         | Group name (editable)                           |
| Edit Scope         | The group’s edit permission level               |
| Allowed Filters    | Number of nodes allowed to be shown in the group|

The following operations are available from the context menu:

- Add a new sub-group
- Copy / move a group
- Delete a group
- Edit group settings (open Editor Preferences)

You can also reorder the hierarchy via drag and drop.

#### Filter Tree View

<img width="2540" height="1042" alt="Image" src="https://github.com/user-attachments/assets/cf81f67c-ec24-42a5-8689-9a81981436b0" />

The Filter Tree View displays and manages the list of filters for the currently selected group. It consists of the following columns:

| **Column**      | **Description**                                            |
|-----------------|------------------------------------------------------------|
| Display Name    | Filter name (node name)                                    |
| Filter State    | Filter state (Allow / Deny / Inherit buttons)              |

You can choose one of the following two display/grouping modes:

| **Grouping Mode**      | **Description**                                            |
|------------------------|------------------------------------------------------------|
| By Category             | Group hierarchically by category path                      |
| By Action Owner         | Group by the action owner (asset/class)                    |

From the context menu, you can switch grouping modes and batch-change the state of selected filters.

#### Group Selector Dialog

<img width="934" height="270" alt="Image" src="https://github.com/user-attachments/assets/f4c7446e-a17c-4b72-8397-8a6a483e0c07" />

If the user’s affiliated group is unknown when the editor starts, the Group Selector Dialog is shown.  
Here, the user selects their group.  
This selection is saved as a per-user personal setting.  
You can also disable showing this dialog on startup from Editor Preferences.  
You can configure it to not prompt re-selection at startup, but in that case, you can later choose a group from `Affiliated Group` in the group settings.

### Shortcut Keys

<img width="1300" height="357" alt="Image" src="https://github.com/user-attachments/assets/a96dccc6-0334-4db5-8048-0b705980f7be" />

The plugin provides the following shortcut commands.  
They can be executed via context menus or keyboard shortcuts:

| **Command**                           | **Description**                                      |
|--------------------------------------|------------------------------------------------------|
| Add New Group                         | Create a new sub-group                               |
| Open Group Settings                   | Open group settings in Editor Preferences            |
| Change To Grouping By Category        | Group the filter list by category                    |
| Change To Grouping By Action Owner    | Group the filter list by action owner                |
| Change Filter State To Allow          | Set selected filters to Allow                        |
| Change Filter State To Deny           | Set selected filters to Deny                         |
| Change Filter State To Inherit        | Set selected filters to Inherit                      |

## Options

The following settings can be configured from Editor Preferences.

#### Group Settings

<img width="2542" height="971" alt="Image" src="https://github.com/user-attachments/assets/8161d2b3-5d51-4f3b-99b6-192b8d7ba45b" />

These are project-wide settings related to group management.

| **Category** | **Item**           | **Description**                                                                                                                        |
|-------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Shared      | Groups             | Shows a tree view of groups selectable in this project. Only groups within the affiliated group’s edit scope can be edited.          |
| Personal    | Affiliated Group   | Select the group you are currently affiliated with.                                                                                   |

The settings for created groups are added under the `Blueprint Node Permissions Filter` section in Editor Preferences.

#### Per-Group Settings

<img width="2540" height="1042" alt="Image" src="https://github.com/user-attachments/assets/cf81f67c-ec24-42a5-8689-9a81981436b0" />

From each custom group’s context menu, choose `Open Group Settings` to open that group’s settings in Editor Preferences.

| **Category**   | **Item**                 | **Description**                                                                 |
|----------------|--------------------------|---------------------------------------------------------------------------------|
| Profile        | Group Name               | Unique identifier. Also used for the INI filename and directory name.          |
|                | Group Display Name       | Display name shown in the tree view. If unset, Group Name is used.            |
|                | Group Description        | Group description text.                                                        |
|                | Edit Scope               | Edit permission level for this group.                                           |
| Permissions    | Is Filtered Per Filter   | Set each node’s show/hide/inherit state in the filter tree view.               |

### Filter Settings

<img width="1311" height="355" alt="Image" src="https://github.com/user-attachments/assets/433ec331-a115-438d-a02a-3dde966fc19e" />

Default settings applied to the overall filter system.

| **Item**                          | **Description**                                                                                                  |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| Node Spawner Classes To Exclude   | List of node spawner classes excluded from the filter system. Nodes from these classes are not filtered.        |
| Node Classes To Exclude           | List of node classes excluded from the filter system. Nodes of these classes are not filtered.                  |
| Action Owner Classes To Exclude   | List of action owner classes excluded from the filter system. Nodes belonging to these asset types are not filtered. |

### Frame-Distributed Task Manager Settings

<img width="1116" height="461" alt="Image" src="https://github.com/user-attachments/assets/a022a612-6229-46cc-93e7-18def0b40ada" />

Tuning settings for the frame-distributed task scheduler.  
The frame-distributed task manager manages the total processing time per frame (global budget) and allocates time to registered tasks based on their priority (weight).  
These settings are shared across all projects.

Within the plugin, the following processes run as frame-distributed tasks:

| **Task**                           | **Description**                                                                                                      |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Apply Selected Group Filter States | Applies the selected group’s filter states to all nodes when the affiliated group changes. Progress is shown via editor notifications. |
| Count Num Of Allowed Filters       | Counts the number of nodes allowed in the group and reflects it in the Group Tree View’s `Allowed Filters` column.   |
| Construct Filter Tree View Nodes   | Builds tree nodes for the Filter Tree View, generating a tree based on grouping mode (category/action owner).        |

| **Category**              | **Item**                         | **Description**                                                                 |
|--------------------------|----------------------------------|---------------------------------------------------------------------------------|
| Global Budget Per Frame  | Should Be Adjusted Automatically | Whether to automatically adjust the per-frame global budget (seconds) based on editor load. |
|                          | Manual Value                     | Manual per-frame global budget (seconds), used when auto-adjust is disabled.    |
|                          | Minimum Limit                    | Lower limit for auto-adjusted per-frame global budget (seconds).                |
|                          | Maximum Limit                    | Upper limit for auto-adjusted per-frame global budget (seconds).                |
| Limitation               | Max Rounds Per Frame             | Maximum number of rounds executed per frame. Remaining budget is redistributed each round based on priority and aging compensation. |
| Aging                    | Weight Factor                    | Degree of assistance for low-priority tasks. Higher values allocate more frame budget to waiting tasks. |
|                          | Max Boost                        | Upper limit for the aging-based priority boost multiplier.                      |
| Automatic Adjustment     | Task Budget Ratio                | Ratio of task budget to editor frame time (e.g., 0.03 = 3%), used when auto-adjust is enabled. |
| Other                    | Frame Follow Alpha               | Exponential moving average factor used for frame tracking.                      |

### Personal Settings

Per-user personal settings.  
These settings do not appear in the Editor Preferences panel and are automatically managed by the plugin’s internal processing.  
Values are saved in `Editor.ini` under `Saved/Config/`.

| **Item**                                    | **Description**                                                   |
|---------------------------------------------|-------------------------------------------------------------------|
| Selected Group Guid                          | GUID of the currently selected group                              |
| Disable Group Selector Dialog When Startup   | Whether to disable the group selector dialog at editor startup    |

## License

This repository contains documentation only. It does not include the plugin’s source code or binaries.  
The plugin itself is distributed on Fab and provided under the [Fab Standard License (Fab EULA)](https://www.fab.com/en-US/eula).  
Unless otherwise noted, the copyright of the documents in this repository belongs to © 2025–2026 Naotsun. Unauthorized reproduction is prohibited.

## Author

[Naotsun](https://twitter.com/Naotsun_UE)

## History

Coming soon