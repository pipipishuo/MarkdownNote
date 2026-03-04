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


绑定专门网卡时用这个
```
udp://@226.0.0.1:7051 -localaddr 172.20.112.1 -fflags nobuffer -flags low_delay -analyzeduration 1000000 -max_delay 500000 -infbuf
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



# ffplay时间控制

## decoder_decode_frame

首先是这个if  基本只要不中断  就会进if  if里面的那个do while  是从解码器中获取帧数据，只要不是again或者能获取到就一直死循环的获取

如果是again 就从Decoder的队列中拿一个pkt，必须是serial一样的  这么看 serial是为了分清是flush过的吗 因为只有开始和fluah   serial才会增加

将拿到的pkt发送给d->avctx

总的来说 这就是个拿到有效帧的函数  也就这一个作用  当然也做了serial不一样时的逻辑处理（这个可能对如果飞机断电的情况起到一定的参考价值）

# read_thread

拿到帧 放进去

# video_thread

拿到帧后 计算好时间



# video_refresh

更新视频



| windex | size | queue |
| ------ | ---- | ----- |
| 1      | 1    | 1     |
| 2      | 2    | 1,1   |
| 0      | 3    | 1,1,1 |



| rindex | size | queue |
| ------ | ---- | ----- |
| 1      | 2    | 0,1,1 |
| 2      | 1    | 0,0,1 |
|        |      |       |



# 数据记录日志

0207的实时传输就明显能看出ffplay的不卡  地面站的卡 所以很有必要迁移过来



# 迁移日志

时间控制迁移在笔记本上C:\Users\LANX\Documents\ffplayimmigrant这里

基本迁移在C:\Users\LANX\Documents\untitled3这里



# 错误代码查询

搜EAGAIN的定义



