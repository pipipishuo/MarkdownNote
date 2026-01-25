测试链接

```
magnet:?xt=urn:btih:0B8E840A5C37008E875A99ACA3306D21A3A1E97A&x._t-v1=603367087271842522
```

load_torrent_file这是第一个看到的函数  将文件进行解析 保存到TorrentDescriptor

AddNewTorrentDialog  这个窗口能够说明TorrentDescriptor都有啥

获取torrent信息  addnewtorrentdialog.cpp

```
m_ui->contentTreeView->setContentHandler(m_contentAdaptor.get());
```

TorrentInfo 应该藏着我需要的东西

```
 const lt::file_storage &fileStorage = m_nativeInfo->orig_files();
```

