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
hevc_handle_packet 这句比较关键
if ((res = av_new_packet(pkt, sizeof(start_sequence) + len)) < 0)
```

