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
