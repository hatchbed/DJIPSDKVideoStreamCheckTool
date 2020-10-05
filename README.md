# DJIPSDKVideoStreamCheckTool

## Introduction
The PSDK video stream check tool is used to check whether the specified video stream conforms to the PSDK video stream standard. The tool supports UDP transmission or a file as input and outputs the results for each individual frame. 

Please refer to the [DJI developer site](https://developer.dji.com/document/65de35b8-78ff-4199-b3ab-ce57ea95301b) for details of the PSDK video steam encoding requirements.

## Build Steps
1. Clone the DJIPSDKVideoStreamCheckTool
```
$ git clone https://github.com/hatchbed/DJIPSDKVideoStreamCheckTool.git
```
2. Move to the project directory
``` 
$ cd DJIPSDKVideoStreamCheckTool/project
```
2. Create a build dirctory and move to that folder
```
$ mkdir build; cd build
```
3. Run cmake (NOTE: cmake version must be version 3.14 or higher).  
```
$ cmake ..
```
4. Compile the tool
```
$ make
```

## Usage

#### Tool Usage Options 
To see the usage options run `./stream_check_tool -h`. Example: 

```
dji@manifold2:~/Documents/DJIPSDKVideoStreamCheckTool/project/build$ ./stream_check_tool -h

stream_check_tool usage:
./stream_check_tool <-u [-p <port>] [-t <type>] |-f [-i <filename>] [-t <type>] > [-h] [-v]

-t type: Stream type, can be any value of "DJI-H264" or "Custom-H264"

sample:
case 1: video stream received by UDP transmission channel as input, and please ensure every frame end with AUD (0x00 0x00 0x00 0x01 0x09 0x10)
   stream_check_tool -u -p udp_port -t stream_type
       udp_port is the UDP port which send video stream to, and the default value is 45654

case 2: video file as input
       stream_check_tool -f -i filename -t stream_type
```

#### Video File as Input 
Example of checking a local video file that was encoded to the DJI-H264 requirements:

```
dji@manifold2:~/Documents/DJIPSDKVideoStreamCheckTool/project/build$ ./stream_check_tool -f -i test.h264 -t DJI-H264

[Passed] 0.stream size(reference 7.3.2.1.1)
[Passed] 1.profile_idc (reference 7.3.2.1.1)
[Passed] 2.level_idc (reference 7.3.2.1.1)
[Passed] 3.YUV420 chroma_format_idc (reference 7.3.2.1.1)
[Passed] 4.chroma and luma only allow 8 bit (reference 7.3.2.1.1)
[Passed] 5.seq_scaling_matrix_presenst_flag (reference 7.3.2.1.1 and 7.3.2.2)
[Passed] 6.frame_mbs_only_flag (reference 7.3.2.1.1)
[Passed] 7.slice_type (reference 7.3.3)
[Passed] 8.num_slice_groups_minus1 (reference 7.3.2.2)
[Passed] 9.max_num_ref_frames (reference 7.3.2.1.1)
[Passed] 10.video resolution (reference 7.3.2.1.1)

[Passed] Currently, all frames has passed the above standards test.
```

#### UDP Transmission as Input
Below is an example of checking a video stream that is encoded to the DJI-H264 requirements.  Ensure that the destination address of the video stream is 127.0.0.1. 

```
dji@manifold2:~/Documents/DJIPSDKVideoStreamCheckTool/project/build$ ./stream_check_tool -u -p 23003 -t DJI-H264
udp reader mode, port 23003

[Passed] 0.stream size(reference 7.3.2.1.1)
[Passed] 1.profile_idc (reference 7.3.2.1.1)
[Passed] 2.level_idc (reference 7.3.2.1.1)
[Passed] 3.YUV420 chroma_format_idc (reference 7.3.2.1.1)
[Passed] 4.chroma and luma only allow 8 bit (reference 7.3.2.1.1)
[Passed] 5.seq_scaling_matrix_presenst_flag (reference 7.3.2.1.1 and 7.3.2.2)
[Passed] 6.frame_mbs_only_flag (reference 7.3.2.1.1)
[Passed] 7.slice_type (reference 7.3.3)
[Passed] 8.num_slice_groups_minus1 (reference 7.3.2.2)
[Passed] 9.max_num_ref_frames (reference 7.3.2.1.1)
[Passed] 10.video resolution (reference 7.3.2.1.1)

[Passed] Currently, all frames has passed the above standards test.
```
