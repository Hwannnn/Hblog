---
layout: post
title: nio StandardOpenOption
published: True
categories: 
- java
tags:
- java
- Files
- nio
- StandardOpenOption
---


신입 프로젝트를 하면서 Apache Commons의 FileUtils를 사용하지 않고, 자바 8에서 기본으로 제공해주는 nio Files와 Paths를 이용해봤다. 프로젝트를 진행하던 중에 파일을 덮어쓰고 싶은 부분있어서 StandardOpenOption 옵션이 적용되는 부분을 찾아봤다.





## Files.write()

* Files.write를 하기위해 사용할 수 있는 메소드는 다음과 같이 3가지가 있다.
* 파라미터 중 OpenOption... options를 보면 여러 개 옵션을 줄 수도 있는 것을 볼 수 있다.

![](/assets/post_images/java/nio_document_write.PNG)



## StandardOPenOption -> .CREATE vs .CREATE_NEW

### StandardOpenOption.CREATE

* Files.write(path, byte, StandardOpenOption.CREATE)
* 파일이 존재하지 않으면 새로 쓰고, 존재하면 기존의 파일내용 뒤에 추가된다.




### StandardOpenOption.CREATE_NEW

* Files.write(path, byte, StandardOpenOption.CREATE_NEW)
* 파일이 존재하지 않으면 새로 쓰고, 존재하면 IOException 상속받는 java.nio.file.FileAlreadyExistsException이 발생 한다.




### 파일이 존재하지 않으면 새로 쓰고, 존재하면 덮어쓰려면?

* Files.write(path, byte)
* 이처럼 StandardOpenOption 옵션을 주지 않으면, 아래 그림과 같이 CREATE, TRUNCATE_EXISTING 옵션이 주어지도록 되어있다.
  * TRUNCATE_EXISTING : 파일을 0바이트로 잘라낸다. 존재하는 파일을 지운다

![](/assets/post_images/java/nio_newOutStream.PNG)

