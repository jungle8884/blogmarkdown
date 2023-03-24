---
title: Java-io
categories:
  - 后端
  - Java
tags:
  - IO
author:
  - Jungle
date: 2023-02-09 12:27:55
---

# IO

## 读取文件和写入文件

>它读取源文件的内容，并将其写入到目标文件：

```java
import java.io.*;

public class ReadAndWriteFile {
    public static void main(String[] args) {
        String sourceFileName = "source.txt";
        String targetFileName = "target.txt";
        String line;

        try (BufferedReader br = new BufferedReader(new FileReader(sourceFileName));
             BufferedWriter bw = new BufferedWriter(new FileWriter(targetFileName))) {
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

> 在这个代码中，使用了 `BufferedReader` 类和 `FileReader` 类读取源文件的内容，并使用 `BufferedWriter` 类和 `FileWriter` 类写入目标文件。同样，如果你要读取和写入其他文件，请更改源文件名和目标文件名的字符串变量。



























