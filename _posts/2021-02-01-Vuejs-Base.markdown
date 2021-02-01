---
layout: post
title: "Welcome to VueJS!"
date: 2021-02-01 10:00:18 +0700
image: vuejs.jpg
tags: [vuejs, vuejs base]
categories: vuejs
---
- <strong>Bài viết là tổng hợp những kiến thức căn bản về Vuejs</strong>
- SPA : sử dụng vuejs tốc độ load trang sẽ nhanh hơn rất nhiều so với bình thường sử dụng server side do ko cần load lại dữ liệu ko cần thiết, chỉ load lại dữ liệu mới ,đối với những dữ liệu ít thay đổi sẽ được giữ lại
- VSCode , NodeJS(cài nodejs sẽ includes cả npm(Node package manager:quản lý những mã nguồn gói hỗ trợ riêng của nodejs ở bên ngoài , sử dụng ở npm trong vuejs) ),Git
- VueInstance (thực thể Vue , đối tượng Vue)
{% highlight ruby %}<a v-bind:href ="url">{{title}}</a> {% endhighlight %}v-bind => khi muốn dùng dữ liệu dàng buộc vào các thuộc tính của HTML thì dùng
- toán tử 3 ngôi đơn giản {% highlight ruby %}ok ? 'yes':'no'{% endhighlight %}
- Numberformat Javascript :
{% highlight ruby %}
   const number = 123456.789;  
   console.log(new Intl.NumberFormat('de-DE', { style: 'currency', currency: 'VND' }).format(number));
{% endhighlight %}
- Event v-on :
{% highlight ruby %}
   <button v-on:click = "handleClick">Click me </button>
    methods:{
              handleClick(event){
              console.log(this)
              this.count +=1
                }
            }
{% endhighlight %}
<span style="color:red">NOTE</span><strong>: khi tạo sự kiên cần chuyền 1 biến mặc định vào là event , đối tượng this sẽ được lấy đối tượng vuejs hiện tại </strong>

- Event Modifiers:
     + v-on:submit.prevent : sẽ ko chuyển trang dựa vào action trong form mà ở lại trang hiện tại
     + v-on:mousemove.stop : sẽ ko cập nhật giá trị ở cha mà chỉ cập nhật ở đối tượng con . đối tượng trực tiếp hover
- Key Modifiers:
     + v-on.keyup.enter="submit" : nhấn key enter sẽ submit
     + Javascript keycode : lây số kí tự
- Computed và methods tương đối giống nhau đều là viết hàm nhưng Computed ko cần dấu () còn methods thì ngược lại,Computed tính toán dựa trên đối tượng,dữ liệu có sẵn của vuejs
- "v-model": dùng để ràng buộc dữ liệu 2 chiều giữa data và input
   + vd : v-model ="name"
- ràng buộc class bằng Vuejs:
   + isActive : false
   + iseEror : true
- v-if="a" v-else="b"
- Vòng lặp :
    + v-for = "item in items"
         {{item.name}}
- v-html='' :render dữ liệu dưới dạng HTML ,

-------------------------------<strong>CLI(command line interface)</strong>--------------------- <br>
Vuejs template webpack
-hãy chắc chắn rằng đã cài nodejs(include npm)
{% highlight ruby %}
$ npm install -g vue-cli
$ vue init webpack my-project (vue init webpack-simple helloVuejs) : nên sử dụng simple
$ cd my-project
$ npm install (cài node_modules)
$ npm run dev
{% endhighlight %}
----------- <strong>ví dụ</strong> --------------<br>
To get started:
{% highlight ruby %}
    cd HelloVuejs
    npm install
    npm run dev
{% endhighlight %}
----------------------------------<br>
- Cấu trúc project,tìm hiểu về webpack
    + package.json : là nơi khai báo cái thư viện , modul ở ngoài mà cần sử dụng cho project
    + Development : là môi trường cho lập trình viên code , test Computed
    + Production : là môi trường cho người dùng .
    + babel : là một trình biên dịch của Javascript (ES6) , biên dịch cú pháp sang các mã nguồn khác nhau
    + Webpack : là công cụ đóng gói code, đóng gói mã nguồn
    + npm run build
    +làm việc chính trong file src
    + cài Vetur trong editor code

- component : là các thành phần của 1 page được chia ra .
- Props : Những data truyền từ component cha vào component con , cú pháp giống thuốc tính
     + sử dụng v-bind: để ràng buộc thuộc tính
<br>
<strong>Thay đổi dữ liệu từ component Cha , tránh việc thay đổi dữ liệu trực tiếp tại component con,đối với for if trong vuejs ko nên sử dụng cùng lúc . vì sẽ chạy vòng for trước . nên ta sẽ dùng hàm if trước lấy dữ liệu clearn rồi mới for ra .</strong> <br>
-----------<strong>Props Down - Event Up</strong>-------------------<br/>
- Props Down : chuyền dữ liệu từ cha vào con -> thằng con chỉ được sài thôi , không được trực tiếp thay đổi .
- Event Up : Truyền thông điệp (truyền sự kiện) thông báo cho component cha biết là nó muốn thay đổi dữ liệu -> Nhiệm vụ của Component cha là nhận được thông điệp và thay đổi data
====> <strong>chỉ thằng nào giữ dữ liệu mới được xóa dữ liệu<strong><br>

--------------<strong>referen trong vue</strong>-------------<br>
- hiểu đơn giản là thay vì custom ô input mặc định . ta tạo 1 ô input mới dễ custom hơn sau đó sử dụng refs của vuejs để gọi đến nút input ban đầu
- ví dụ :
       this.$refs.fileInputAvatar.click()<br>

-------<strong>slot trong vuejs</strong>------------<br>

- gửi html qua component
- ví dụ:
{% highlight ruby %}
<component>HTML</component> => son : <slot></slot> ==> son : HTML
{% endhighlight %}

-------------------<strong>lifecycle Diagram</strong>--------------------<br>

- <strong>Một số tài liệu thao khảo thêm</strong> <br>

Tutorial : [zendVN]

GitHub : [Sample]

[zendVN]: https://youtu.be/q1CNKjPPfdc
[Sample]:   https://github.com/pytutorial/html_samples/tree/master/vuejs
