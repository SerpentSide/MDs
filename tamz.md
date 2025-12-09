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
- Shared preferences
    - Stored in key-value pair
    - `getSharedPreferences()`
    - `getPreferences()`
    - `putString()`, `getBoolean()`...
    - `.commit` to save
- Internal storage
    - Private storage on device memory
    - uses FileOutputStream
- External storage
    -  Non-private for large datasets
- SQLite
- Network connection
- Content provider
#### Transactions
- ACID
    - Atomicity - Performed all or nothing
    - Consistency - When completed, data must be consistent
    - Isolation - Modifications must be isolated between transactions
    - Durability - After completion, effects are permanently in place. Persists even with sysfail
## Game development
- Delta time - Time sinde the last fram update
- Game loop - Root of the game graph. Children get called once per frame
- Android game development kit
    - C/C++ core, perofrmance tuning, high-performance audio
- Android game development extension
    - For VS Code development
- Android GPU Inspector
    - Graphic tool, tracking and analysis of individual frames
#### Inputs
- Polling
    - `while(!button.pressed){}`
- Event-based handling
    - `button.setOnClickListener(){}`
- Events
    - Touch down / drag / up
    - Key down / up
    - Accelerometer
#### Rendering
- 2D
    - Canvas (CPU)
    - SurfaceView (CPU)
    - OpenGL/Vulcan (GPU)
    - Jetpack Compose (GPU)
- 3D
    - OpenGL/Vulcan (GPU)
- onDraw() - Calls drawing
- invalidate() - Calls redraw
#### Image
| PNG | JPG |
| --- | --- |
| Dosent loose data at compresion | Loose data at compresion |
| Compresion ration ~2.7:1 | Compresion ration 10:1 |
| Higer size | Lower size |
- WebP
    - Better. lower size, loosless compresion
#### Android 2D Graphics
- Drawable
    - **`android.graphics.drawable`**
        - ShapeDrawable
        - BitmapDrawable
        - PictureDrawable
        - LayerDrawable
        - NinePatchDrawable
            - Stretchable bitmap
            - PNG that includes extra 1-pixel-wide border
            - Extension .9.png
    - **`android.view.animaiton`**
#### Audio & Video
- Built-in encoderrs/decoders
- Core media formats
## Network connectivity
- Data sync
- Fetching resources
- Real-time updates
- Multi-device, peer sharing
- Cloud
#### Types
- Network
- Bluetooth
- NFC (Near field communication)
- USB
---
- Must set permisions
    - `<uses-permission android:name="android.permission.INTERNET" />`
    - `<uses-permission
android:name="android.permission.ACCESS_NETWORK_STATE" />`
- ***Network reachabilty ≠ Actual internet access***
- Conectivity Manager
    - Gives state of network
    - Notify changes
- Network info
    - Describe network interface status
#### Sockets
- `Socket client = new Socket("hostname", portNumber);`
- **UDP**
    - Packet for real time streaming
    - Dosent matter if damaged packet could be corrected or retransmitted
    - Conectionless
- **STUN**
    - Internet standarts-track suite of methods
    - Used in NAT Traversal
    - Allows operation through NAT (Network address translator)
#### HTTP & REST APIs
- HTTP methods
    - GET, POST, PUT, DELETE, PATCH
    - Request / Response ->Status codes (2xx, 4xx, 5xx)
    - Headers, query parameters, path parameters, body (JSON, XML, binary)
#### Asy patterns
- *Bad*
#### Http clients
- OkHttp
    - Connection pooling, transparent GZIP compression, HTTP/2 multiplexing.
- Retrofit
    - Defines connection as interface
    - Generate implementation
- Ktor client
    - Multiplatform
- Volley
    - Automatic scheduling of network requests
    - Multiple concurrent network connections
#### APIs Patterns
- REST API
- WebSocket
    - Persistent full-duplex communication over single TCP
- Server sent event (SSE)
    - One way chanel: server -> client
- GraphQL
- gRPC
    - Uses protocol bufers
#### BaaS (Backend as a Service)
- Firebase
    - Google
- AWS
    - Amazon Web Service
- Supabase
- Google cloud
#### Data conversion
- Big Endian
    - \>ARMv3
    - More natural
- Little Endian
    - \<ARMv3, x86
    - Easier to place values
#### Security
- Use TLS / HTTPS, avoid clear-text
- Validate certificates
- Auth tokens (JWT, OAuth2)
- Dont leak data *duh*
## Connectivity
#### USB
- UsbManager
    - Entry point for managing
- UsbDevice
    - Represent USB device
- UsbInterface
    - Functional group within device (e.g. printer interface)
- UsbEndpoint
    - Communication port (IN/OUT)
- UsbDeviceConnection
    - Chanel with transfers
        - Control
        - Bulk
        - Interrupt
        - Isochronous
#### WiFi
- Throttling
    - Each foreground app can scan four times in 2 minutes
    - All background apps combined can scan once per 30 minutes
- Wifi direct
    - Connection between device via wifi without the need for acces point
#### Bluetooth (Modrý zub🤡)
| Class | Range |
| --- | --- |
| 1 | 100m |
| 2 | 10m |
| 3 | 1m |
- Permisions
    - BLUETOOTH - To connect
    - BLUETOOTH_ADMIN - To discover and pair
- BLE (Bluetooth Low Energy)
    - Short data bursts
    - Low energy footprint
    - For smartwatches, fit trackers, medical devices
    - GATT Client - Android app
    - GATT Server - Device, peripheral
#### Printing protocols
- ESC/POS
    - For small thermal receipt printers
    - Series of ESCs for text, QR codes...
- ZPL/CPCL (Zebra)
    - mobile label printers
- PCL/PostScript
    - Common office printers
- IPP/LPR/RAW (port 9100)
    - Network protocols
## Barcodes, NFC, RFID
#### AIDC (Automatic Identification and Data Capture)
- Barcode / QR code
    - UPC, EAN, Code39
    - QR up to 2953 bytes or 7089 numeric
- RFID (Radio-Frquency identification)
- NFC (Near field communication)
    - Subset of RFID
    - NDEF Message
    - 1 to many Records
    - HCE (Host card emulation) - Emulating contacless pay card
        - NFC Application Protocol Data Unit (APDU)
## Locations and Sensors
***GPS ≠ GNSS***
- GNSS - Global navigation satelite system
    - Global umbrela term
    - GPS - U.S. Global positioning system
    - Galileo
    - GLONASS
    - BeiDou
    - QZSS
- Multi-constellation GNSS since API 24+
    - Availability
    - Accuracy
    - Time To First Fix
#### Augmentation and Correction Systems
- SBAS (Satelite base augmentation system)
- RTK (Real time kinematic)
    - GPS correction
    - Centimeters accuracy
    - Used in drones, autonomous driving
- PPP (Precise point positioning)
#### Location
- Fused location provider
    - Google location API
    - Permisions
        - ACCESS_COARSE_LOCATION
        - ACCESS_FINE_LOCATION
        - ACCESS_BACKGROUND_LOCATION
- GPS Module
    - Uses trilateration
- WiFi Module
- Combine with BLE or UWB (Ultra wide modulation)
- Sensors
    - `Android.hardware`
    - Motion, enviromental, position
    - Continuous, on-change, one-shot
    - Accelerometer, gyroscope, camera, microphone...
- Update Interval
    - `setInterval()`
    - `setPriority`
        - PRIORITY_BALANCED_POWER_ACCURACY
        - PRIORITY_HIGH_ACCURACY
        - PRIORITY_LOW_POWER
        - PRIORITY_NO_POWER
#### Activity
- Activity Recognition API
    - Detects walking, runnig, still, in-vehicle, cycling
    - Low power
- Awareness API
    - Combines location, time, headphones, weather, activity
    - Contextual triggers -> `when outdoor then tracker`
#### Maps
- Google maps
    - Jetpack Compose Maps SDK
- Alternatives
    - Mapbox
    - OSM Droid
    - MapTiler SDK
- WMS (Web map service)
    - Server returns raster or vectors
    - Response -> Rendered map image (PNG/JPEG)
#### Sensor hub  --- **THE** Hub
\*wink\*
- Cluster of conected sensor chips
## Cryptography
- Study of matemathical techniques related to aspects of information security 🤓