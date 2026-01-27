read_thread  这个函数是用来读的

测试参数rtsp://admin:lanxiaohk12@192.168.1.64:554/Stream1/Channels/1  -fflags nobuffer -flags low_delay

decoder_decode_frame



这个延迟就比较低了

```
ffplayd.exe rtsp://admin:admin@192.168.8.64:554/ -rtsp_transport tcp -fflags nobuffer -flags low_delay -analyzeduration 1000000
```



```
ffplayd.exe rtsp://admin:admin@192.168.8.64:554/ -rtsp_transport tcp -fflags nobuffer -flags low_delay -analyzeduration 1000000 -max_delay 500000 -reorder_queue_size 4 -infbuf
```



```
video_thread

upload_texture这个应该是显示了  但没啥意义  sdl本身就能显示yuv图像
```



```
rtsp://admin:1a2s3d4f5g@192.168.8.64:554/ -rtsp_transport tcp -fflags nobuffer -flags low_delay -analyzeduration 1000000 -max_delay 500000 -reorder_queue_size 4 -infbuf
```



写入应该从这入手

```
write_packet

process_input

init_output_stream_wrapper
//这个是获取rtsp包的
get_input_packet
```



```
stream_component_open
```



```
hevc_handle_packet 这句比较关键  说明了packet的size为啥跟packet->buf的size不一样
if ((res = av_new_packet(pkt, sizeof(start_sequence) + len)) < 0)
```



这句代码 包含了时间基是从哪确定的信息：ic->streams[pkt->stream_index]->time_base

```
pkt_in_play_range = duration == AV_NOPTS_VALUE ||
                (pkt_ts - (stream_start_time != AV_NOPTS_VALUE ? stream_start_time : 0)) *
                av_q2d(ic->streams[pkt->stream_index]->time_base) -
                (double)(start_time != AV_NOPTS_VALUE ? start_time : 0) / 1000000
                <= ((double)duration / 1000000);
```



保留的命令行参数

```
rtsp://admin:1a2s3d4f5g@192.168.8.64:554/ -rtsp_transport tcp -fflags nobuffer -flags low_delay -analyzeduration 1000000 -max_delay 500000 -reorder_queue_size 4 -infbuf
```



读取组播时的方法

handle_packets



mpegts_push_data

pes->pts = ff_parse_pes_pts(r);



1378行 ret = new_pes_packet(pes, ts->pkt);

# 查找inputformat

av_probe_input_format3

# 从这定的dts



```
pkt->dts = select_from_pts_buffer(st, sti->pts_buffer, pkt->dts);
```



# 关键变量

mov_default_parse_table

ff_raw_read_partial_packet

udp_read

# 读mp4的方法

mov_read_packet



# 几个关键的box



## stco

|                |                                                    |
| -------------- | -------------------------------------------------- |
| 理解           | chunk偏移哦哦哦  就是说这个chunk是在文件的哪个位置 |
| 章节           | 8.7.5 Chunk Offset Box                             |
| ffmpeg对应方法 | mov_read_stco                                      |



## stss

|                |                        |
| -------------- | ---------------------- |
| 理解           | 记载关键字索引         |
| 章节           | 8.7.5 Chunk Offset Box |
| ffmpeg对应方法 | mov_read_stss          |



## stsd

|                |                                |
| -------------- | ------------------------------ |
| 理解           | 记载样本表的具体细节信息  比如 |
| 章节           | 8.7.5 Chunk Offset Box         |
| ffmpeg对应方法 | mov_read_stsd                  |

