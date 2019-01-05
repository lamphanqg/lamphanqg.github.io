---
layout: post
title: "Sử dụng concat trong content_tag của Rails"
tags: [rails]
---
# Hàm `concat` là gì?
Nói đơn giản thì `concat` là hàm để nối và print string ra view trong Rails.
Thay vì bạn dùng `+`
~~~ erb
<%= "a" + "b" %>
~~~
thì sẽ thay bằng `concat`
~~~ erb
<%
  concat "a"
  concat "b"
%>
~~~
Tham khảo source code của `concat` ở đây: [https://apidock.com/rails/ActionView/Helpers/TextHelper/concat](https://apidock.com/rails/ActionView/Helpers/TextHelper/concat)

# Tại sao phải dùng `concat`?
Nếu là đơn giản như trên thì dùng `+` cho khỏe, `concat` làm qué gì!

À thì thật ra là dùng `concat` nhìn cho đẹp thôi à :laughing:

Khi bạn viết helper để in HTML ra, sẽ gặp những trường hợp helper rất nhiều phần, khi đó thay vì `+` thì dùng `concat` nhìn sẽ gọn gàng ngăn nắp hơn.

Ví dụ:
~~~ ruby
def hello_world
  content_tag(:p, class: 'hello') do
    content_tag(:span, 'HELLO') + ' ' + content_tag(:span, 'WORLD') + '.' +
    content_tag(:span, 'How are you?')
  end
end
~~~
có thể viết lại thế này:
~~~ ruby
def hello_world
  content_tag(:p, class: 'hello') do
    concat content_tag(:span, 'HELLO')
    concat ' '
    concat content_tag(:span, 'WORLD')
    concat '.'
    concat content_tag(:span, 'How are you?')
  end
end
~~~
nhìn ngay thẳng hơn hẳn mà đúng ko? :sunglasses:

# Lưu ý khi dùng `concat`
`concat` chỉ dùng bên trong 1 block thôi, nếu dùng bên ngoài nó sẽ nối string của tất cả output buffer, nghĩa là toàn bộ HTML chuẩn bị được in ra từ đầu tới chỗ đó luôn.

