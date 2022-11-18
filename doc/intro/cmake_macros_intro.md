# Introduction to rs_driver's CMake macros

## 1 CMakeList.txt

A group of macros can be used to determine what and how to compile rs_driver. Please find them in `CMakeLists.txt` of the project.

## 2 What to compile

The targets of compilation includes:
+ Demo Apps, including `demo_online`, `demo_pcap`, etc.
+ Tools, including `rs_driver_viewer`, `rs_driver_pcdsaver`, etc.
+ Test cases

### 2.1 COMPILE_DEMOS

COMPILE_DEMOS determines whether to compile Demo Apps.
+ COMPILE_DEMOS=OFF means No。This is the default.
+ COMPILE_DEMOS=ON means Yes。

```
option(COMPILE_DEMOS "Build rs_driver demos" OFF)
```

### 2.2 COMPILE_TOOLS

COMPILE_TOOLS determines whether to compile tools.
+ COMPILE_TOOLS=OFF. Whether to compile `rs_driver_viewer`/`rs_driver_pcdsaver`, is determined by COMPILE_TOOLS_VIEWER/COMPILE_TOOLS_PCDSAVER. This is the default.
+ COMPILE_TOOLS=ON. Compile both `rs_driver_viewer` and `rs_driver_pcdsaver`, no matter what COMPILE_TOOLS_VIEWER and COMPILE_TOOLS_PCDSAVER are.

```
option(COMPILE_TOOLS "Build rs_driver tools" OFF)
```

### 2.3 COMPILE_TOOLS_VIEWER

COMPILE_TOOLS_VIEWER determines whether to compile `rs_driver_viewer`, in case of COMPILE_TOOLS=OFF.
+ COMPILE_TOOLS_VIEWER=OFF means No. This is the default.
+ COMPILE_TOOLS_VIEWER=ON means Yes.

```
option(COMPILE_TOOL_VIEWER "Build point cloud visualization tool" OFF)
```

### 2.4 COMPILE_TOOLS_PCDSAVER

COMPILE_TOOL_PCDSAVER determines whether to compile `rs_driver_pcdsaver`, in case of COMPILE_TOOLS=OFF.
+ COMPILE_TOOLS_PCDSAVER=OFF means No. This is the default.
+ COMPILE_TOOLS_PCDSAVER=ON means Yes.

```
option(COMPILE_TOOL_PCDSAVER "Build point cloud pcd saver tool" OFF)
```

### 2.5 COMPILE_TESTS

COMPILE_TESTS determines whether to compile test cases.
+ COMPILE_TESTS=OFF means No. This is the default.
+ COMPILE_TESTS=ON means Yes.

```
option(COMPILE_TESTS "Build rs_driver unit tests" OFF)
```



## 3 How to compile

### 3.1 DISABLE_PCAP_PARSE

DISALBE_PCAP_PARSE determines whether to support the PCAP source, that's to say, whether to parse MSOP/DIFOP packets from a PCAP file.
+ DISABLE_PCAP_PARSE=OFF means Yes, This is the default.
+ DISABLE_PCAP_PARSE=ON means No. On embedded Linux, PCAP source is not needed, so enable this macro to avoid porting the `libpcap` library.

```
option(DISABLE_PCAP_PARSE         "Disable PCAP file parse" OFF) 
```

### 3.2 ENABLE_TRANSFORM

ENABLE_TRANSFORM determines whether to support coordinate transformation.
+ ENABLE_TRANSFORM=OFF means No. This is the default.
+ ENABLE_TRANSFORM=ON means Yes, **Enable this costs much CPU resource, so please don't do so in released products.**

```
option(ENABLE_TRANSFORM           "Enable transform functions" OFF)
```

### 3.3 ENABLE_DOUBLE_RCVBUF

ENABLE_DOUBLE_RCVBUF determines whether to double the receiving buffer of the MSOP/DIFOP sockets.

On some platforms (such as embedded Linux, Windows, etc), the default size of the buffer is small. This may cause loss of MSOP/DIFOP packets, so enable this macro to enlarge the buffer.
+ ENABLE_DOUBLE_RCVBUF=OFF means No. This is the default.
+ ENABLE_DOUBLE_RCVBUF=ON means Yes. The current implementation will change this value to `4` times. You can change it if needed.

```
option(ENABLE_DOUBLE_RCVBUF       "Enable double size of RCVBUF" OFF)
```

### 3.4 ENABLE_WAIT_IF_QUEUE_EMPTY

ENABLE_WAIT_IF_QUEUE_EMPTY determines what the handling thread do if the MSOP/DIFOP packet queue is empty.
+ ENABLE_WAIT_IF_QUEUE_EMPTY=OFF means to wait for condition variable. This is the default.
+ ENABLE_WAIT_IF_QUEUE_EMPTY=ON means to call usleep(). This can decrease CPU usage, however increase the delay of point cloud frames.

You should weigh up the pros and cons based on your cases.

```
option(ENABLE_WAIT_IF_QUEUE_EMPTY "Enable waiting for a while in handle thread if the queue is empty" OFF)
```

### 3.5 ENABLE_EPOLL_RECEIVE

ENABLE_EPOLL_RECEIVE determines how to recieve MSOP/DIFOP packets.
+ ENABLE_EPOLL_RECEIVE=OFF means to use select(). This is the default.
+ ENABLE_EPOLL_RECEIVE=ON means to use epoll(). 

```
option(ENABLE_EPOLL_RECEIVE       "Receive packets with epoll() instead of select()" OFF)
```

### 3.6 ENABLE_STAMP_WITH_LOCAL

ENABLE_STAMP_WITH_LOCAL determines whether to convert the timestamp of point cloud to local time.
+ ENABLE_STAMP_WITH_LOCAL=OFF means No, and to use UTC time. This is the default.
+ ENABLE_STAMP_WITH_LOCAL=ON means Yes, and to convert the timestamp to local time。

```
option(ENABLE_STAMP_WITH_LOCAL    "Enable stamp point cloud with local time" OFF)
```

### 3.7 ENABLE_PCL_POINTCLOUD

ENABLE_PCL_POINTCLOUD determines the format of point cloud in the Demo Apps.
+ ENABLE_PCL_POINTCLOUD=OFF means to use the RoboSense defined format. This is the default.
+ ENABLE_PCL_POINTCLOUD=ON means to use the PCL format. The PCL library is needed to enable this.

```
option(ENABLE_PCL_POINTCLOUD      "Enable PCL Point Cloud" OFF)
```


