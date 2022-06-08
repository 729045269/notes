# php读取目录下面的文件列表，并按照时间排序

```
//获取最新的备份文件目录
        $path = "/wwwroot/backup";
        if (!is_dir($path)) {
            return false;
        }
        $handle = opendir($path);
        $files = [];

        if ($handle) {
            while (($file = readdir($handle)) !== false) {
                if ($file != "." && $file != "..") {
                    $files[] = ["name" => $file, "time" => date("Y-m-d H:i:s", filemtime($path . '/' . $file))];
                }
            }
        }
        closedir($handle);
        if ($files) {
            $fileTime = array_column($files, 'time');
            //按时间降序
            array_multisort($fileTime, SORT_DESC, SORT_STRING, $files);
            $lastFile = $files[0];//最新的文件
        }
        var_dump($files);
        var_dump(lastFile);
        
```

