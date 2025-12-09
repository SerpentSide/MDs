## TAMZ II
#### Android platform libraries
- System C
- Graphics
- Surface manager
- Audio manager
#### Android aplication framefwork
- Package
- Window
- View system
- Resource
- Activity
- Content
#### Art
- Replaced Davig since 5.0
#### Android bootup
- Boot ROM
- Bootloader - Memories, verification, security
- Kernel - Interupt controllers, cahces and scheduling
- Init process - Mounts file system, launch processes
## Application Internals
#### Types of apps
- Foreground
    - Visible on screen
- Background
    - Restricted since 8.0
- Widget & shortcuts
    - Entry points
- Multi-window & multi-resume
    - Active side by side (tablets, multiscreens)
#### App components
- Activity
    - Primary for user interaction
    - Presentation layer
    - Each activity start stops the previous and save in stack
    - Responsible for saving its own state
    - onCreate - Must have, calls when creating
    - onPause - System calls when user leaves activity
    - onStart, onResume, onRestart, onStop, onDestroy
- Service
    - Code running in background
    - Independent of any activity
- Content provider
    - Store and share data
    - Accesble by multiple apps
- Broadcast reciever
    - Responds to system-wide broadcast
    - Subsriber in publish/subscribe pattern (asy observer)
- Widget
    - Visual components
    - Notifications
#### Navigation
- Task
    - Collection of activities
- Backstack
- Suspending & resuming activites
#### Priorities
- All apps remain running until the system needs memory
- Killing apps based on priority
- With same priority, kills process with longest lower priority history
- Active process
    - Foreground
- Visible process
    - Visible, not active
- Started service
    - Should continue without visible interface
- Background process
    - Without running service
- Empty process
---
On config change, Android kills Activity and restarts it. Automaticly saves the state Views with UID
#### Intents
- Describing action (e.g. Send a message)
- Intent object - Bundle of information
- Passive data structure, holding abstract description of operation to be performed
- Operations, events
- Sends asy messages
- Communicate between activities, components...
- Explicit
    - Names the component 
- Implicit
    - Asks sytem to perform service without telling which class should do this service
    - Android tries to find Activity that mathes, this is called **Intent resolution**
        - Based on the **Intent** and **Intent filtres**
        - Intent filters specified in AndroidManifest.xml
            - Action
            - Data
            - Category  
            `Intent inte = new Intent();`  
            `inte.setAction(Intent.ACTION_SEND);`  
            `inte.setType("plain/text");`  
            `startActivity(inte);`
#### Intents objects
- Name (opt)
- Action
    - String naming the action
- Data
    - Uri of data and MIME type 
- Category
- Extras
    - Key-values pairs
- Flags
#### App manifest
- Defines structure, Intent filters and permissions
- uses-sdk define min and max SDK
- uses-configuration specify supported devices
- uses-feature specify hardware features and min verision of OpenGL
- supports-screens specify which screens u can and cant use
    - smallScreens
    - normalScreens
    - largeScreens
    - anyDensity
- You must include all activities
- **Launch methods**
    - standart - new Activity launched and added to backstack
    - singleTop - if Activity instance exists on top, Android routes to the instance instead of creating new
    - singleTask - new Activity is created in new task, if task already exists, routes to the task instead
    - singleInstance - same as singleTask, except the Activity is always single and only member of its task
#### Resources
- **Non-source** code entity
- Layout
- Strings
- Images
- Menus
- Animations
- Located in `/res`
- **R class** - automaticaly generated. Cotains references for each resource
- `package.resource_type.resource_name`
- Code: `R.string.hello`
- XML: `@string/hello`
- Default
    - Should be used regardles of config
- Alternative
    - Designed for specific config
#### Assets
- To store **any** kind of data
## Basic UI
- View
    - any view have UID
- ViewGroup - View that holds other views
#### Jetpack compose
- Composable functions
- Faster iteration
- Eliminates boilerplate (XML + findViewById)
- Easier testing
- @Composable - tells the compiler this is gonna be UI
#### Callbacks
- onClick()
- onLongClick()
- onFocusChang()
- onKey()
- onTouch()
- ...
#### States
- Local state - Hold by one composable
- Hoisted state - Moved into shared parent component
#### Layout
- Container - Layout Manager
- Constrain
    - Created manualy in code or XML
- Linear
    - Horizontaly/Verticaly
- Relative
    - Lays widgets based on relations
- Table 
    - Grid base on specification
- Scrollview
- Nesting composable
    - Column, row, box, spacer...
#### Fragments
- Behavior or a portion of UI
- Life cycle
    - Resumed - Visible
    - Paused - Another activity has focus
    - Stopped - Not visible
- States
    - onAttach
    - onCreate
    - onCreateView
    - onActivityCreated
    - onDestroyView
    - onDestroy
    - onDetach
#### Menus
- Options
- Contextual
- Contextual action bar
- Popup menu
## Data storage