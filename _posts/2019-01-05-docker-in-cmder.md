---
layout: post
title: "Khởi động docker trong Cmder"
tag: [windows, cmder, docker]
---
# Cmder là gì?

[Link của em nó đây](http://cmder.net/)

Nếu bạn là web developer, và nếu bạn đã từng dùng MacOS hoặc Linux, thì hẳn là sẽ quen với terminal (bash). Một trong những điểm trừ của Windows là không có terminal, mà chỉ có một thứ gần giống như terminal: cmd. Mà cái này thì ai dùng rồi sẽ biết, vừa xấu vừa cùi bắp.

Vì một lí do nào đó, môi trường làm việc của bạn là Windows, thì hẳn là bạn sẽ muốn có một tool giống như terminal thay vì dùng cmd. Cmder cung cấp cho bạn một tool gần gần giống như vậy, có thể dùng 1 số command như ở bash của Linux (cd, ls v.v thậm chí là ssh), tự động khởi động git, transparent background, thay đổi background & foreground color, font chữ v.v

# Docker trong Windows

Gần đây, nhiều dự án mình làm việc đều sử dụng docker để dựng môi trường local. Side project của mình cũng vậy. Tuy nhiên, khi cài đặt trên Macbook thì dễ, nhưng máy bàn mình dùng Windows, và cài đặt trên Windows thì hơi phiền phức hơn xíu. [Docker for Windows](https://docs.docker.com/docker-for-windows/) chỉ sử dụng được với Windows 10 Pro, còn máy mình dùng là Windows 10 Home nên phải dùng [Docker Toolbox](https://docs.docker.com/toolbox/overview/).

Khi cài xong Docker Toolbox, muốn khởi động Docker, bạn sẽ phải dùng app Docker Quickstart Terminal, tức là Docker tạo riêng cho bạn 1 terminal để chạy nó, còn mở cmd hoặc Cmder bình thường thì bạn sẽ không dùng được docker trong đó.
![Docker terminal](/assets/images/docker_terminal.png)

# Setup để khởi động docker cùng với Cmder

Nhu cầu của mình là làm sao để bật Cmder lên và sử dụng được docker trong đó. Cách làm như sau:

* Vô *Setup tasks...* để tạo một loại session mới cho Cmder
![Select setup](/assets/images/setup_task.png)
* Setup như hình:
![Setup detail](/assets/images/setup.png)
  * ô icon:
```
/icon "C:\Program Files\Docker Toolbox\docker-quickstart-terminal.ico"
```
  * ô commands
```
cmd /c ""%ConEmuDir%\..\git-for-windows\bin\bash" --login -i "C:\Program Files\Docker Toolbox\start.sh" -new_console:d:"C:\Program Files\Docker Toolbox""
```
  * đường dẫn tới bash bạn có thể copy từ task {bash::bash} có sẵn
  * thay thế đường dẫn tới Docker Toolbox bằng đường dẫn trong máy của bạn
* Sau khi setup thì bạn sẽ có 1 loại session mới như hình sau, khi mở session loại này thì docker sẽ được khởi động sẵn cho bạn
![Docker Cmder session](/assets/images/docker_session.png)