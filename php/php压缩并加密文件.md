# php压缩并加密文件

PHP7.2以下不支持加密

```
 //加密文件
 $password = "123456";
 $zipArc = new \ZipArchive();
 $path = "./readme.txt";
 if ($zipArc->open('./test.zip', \ZipArchive::CREATE | \ZipArchive::OVERWRITE == true)) {
     if (!$zipArc->setPassword($password)) {
        throw new RuntimeException('备份数据库设置压缩密码错误');
     }
     //往压缩包里面添加文件， addFile("要加密的文件路径和文件名","加密后的文件路径和文件名")
     $zipArc->addFile($path, "/readme.txt");
     //加密文件 此处的文件名和路径是压缩包内的
     if (!$zipArc->setEncryptionName("/readme.txt", \ZipArchive::EM_AES_256)) {//此处组合才是加密的核心关键
     	 throw new RuntimeException('加密失败');
     }
     $zipArc->close();
 }
```

