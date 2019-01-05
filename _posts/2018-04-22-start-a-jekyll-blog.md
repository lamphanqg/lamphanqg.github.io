---
layout: post
title: "Tạo blog với Jekyll và Github Pages"
tags: [jekyll, blog, githubpages]
---
# Cài đặt Jekyll

Cơ bản thì cài Jekyll khá là dễ. Theo hướng dẫn dưới đây là xong:

[https://jekyllrb.com/docs/installation/](https://jekyllrb.com/docs/installation/)

Cá nhân mình thì khuyến khích các bạn cài ruby bằng rbenv nếu dùng Mac hoặc Ubuntu, và dùng ruby installer nếu là máy Windows (mặc dù mình dùng Win10 nhưng không thích Bash của nó lắm, dùng cứ cảm thấy bê đê kiểu gì ấy :unamused:)

Lưu ý là bạn không cần phải cài gem `github-pages` như trong hướng dẫn của Windows. Thật ra thì mình đã thử, và cài không được do vướng conflict về version.

```
Bundler could not find compatible versions for gem "jekyll":
  In snapshot (Gemfile.lock):
    jekyll (= 3.8.0)

  In Gemfile:
    github-pages x64-mingw32 was resolved to 4, which depends on
      jekyll (= 1.1.2) x64-mingw32

    minima (~> 2.0) x64-mingw32 was resolved to 2.5.0, which depends on
      jekyll (~> 3.5) x64-mingw32

Running `bundle update` will rebuild your snapshot from scratch, using only
the gems in your Gemfile, which may resolve the conflict.
```

Mình cũng đã thử set version mới nhất của github-pages `gem "github-pages", "182"` nhưng cũng vô ích, vì trong dependencies của gem nó set cứng `"jekyll" => "3.7.3"` trong khi jekyll mình cài là `3.8.0`. Tóm lại là thôi khỏi cài, dù sao cũng không cần thiết lắm. Không có gem này bạn vẫn chạy server trên local vù vù, vẫn build ngon lành.

# Upload blog lên Github Pages
Cơ bản thì cứ theo hướng dẫn trong này:

[https://pages.github.com/](https://pages.github.com/)

Bạn có 2 lựa chọn:
1. site cho user/organization: username của bạn sẽ trở thành subdomain của github.io => site của bạn sẽ có dạng `<username>.github.io`
2. site cho project: từ cái site cho user/organization ở trên, bạn có thể chia ra nhiều project site, mỗi project 1 repo riêng, với URL của site sẽ là `<username>.github.io/<repo name>`

Sau khi tạo xong repo như hướng dẫn, thì bạn có 2 lựa chọn để up site lên:
1. `jekyll build` site của bạn dưới local, xong rùi commit folder `_site` lên repo trên github
2. Commit thẳng folder blog của bạn lên github luôn, khỏi build biếc gì hết. Khi lên github, do github mặc định chạy gem `github-pages` trên đó, nó sẽ đọc file `_config.yml` của bạn và tự động gom gem, build site cho bạn trên đó luôn, không cần làm gì thêm cả. Commit, push xong ra uống cà phê 10' sau vô là có con blog ngon lành cành đào đã được activate.

Lưu ý là do site của bạn được build bằng gem `github-pages`, nên có thể version của 1 số theme hoặc plugin bạn dùng dưới local sẽ không giống trên github, dẫn đến sai lệch về layout hoặc chức năng (nhưng cũng không nhiều đâu). Bạn có thể tham khảo version của theme và plugin mà thằng `github-pages` dùng ở đây:

[https://pages.github.com/versions/](https://pages.github.com/versions/)

# Kết
Rùi, vậy là xong cái blog. Lười quá nên không viết dài, theo tôn chỉ là cái gì mò được dễ dàng thì để các đồng đạo tự mò cho lên tay, cái gì tìm hiểu mất thời gian, dễ gây rối loạn thì mới ghi ra thui. Good luck have fun :sunglasses: