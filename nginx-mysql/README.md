#Triển khai nginx theo cây thư mục chuẩn của Ansible.

```sh
Bố cục của một chương trình Ansible chuẩn :
```

```sh
Ansible/
|---- files
|
| ---- group_vars
|      |----all
|----- roles
|      |---- web
|      |     |---- handlers
|      |     |     |---- main.yml
|      |     |---- tasks
|      |           |---- main.yml
|      |           templates
       |           |---- default.conf
|      |---- db
|            |---- handlers
|            |     |---- main.yml
|            |---- tasks
|                  |---- main.yml
|
|---- hosts
|
|---- main.yml
```

- Trong cây thư mục chuẩn này chúng ta có thể đơn giản hóa được với các chương trình CM lớn , một số trường hợp có thể nhắc đến như sau :
 <ul>
  <li>Trong một chương trình CM chúng ta có nhiều biến lặp đi, lặp lại với nhau để tránh việc nhầm lẫn cũng như khai báo quá nhiều lần chúng ta chỉ cần khai báo 1 lần trong `group_vars`</li>
  <li>Một trương trình triển khai trên nhiều host thì chúng ta lại phải chạy số lần bằng với số host cần đến cho 1 chương trình, như thế sẽ mất thời gian ngồi đợi hết host này để tiếp tục triển khai trên host khác . Như thế sẽ không đúng với mục tiêu của các chương trình CM khi viết ra (chỉ cần ấn sau đó đi đâu đó làm ly cafe và đợi), với cây thư mục như này chúng ta có thể thực hiện một chương trình như thế chỉ bằng 1 thao tác.</li>
  <li>Tiện lợi trong cấu hình và chỉnh sửa lỗi vì các file cấu hình và các tác vụ được phân chia rõ ràng chứ không gồm trong 1 file.</li>
 </ul>

- Chúng ta cùng xem qua một chút về các file m thư mục và chức năng của nó :
 <ul>
  <li>files : đây là thư mục chứa tất cả các files mà chúng ta cần cho chương trình ansible để chúng ta coppy vào các máy client.</li>
  <li>group_vars/all : đây là file chứa tất cả các biến trong chương trình ansible.</li>
  <li>roles : Đây là thư mục chính chứa các file cấu hình cũng như triển khai mà chương trình ansible cần để thực hiện, có thể gọi đây là file điều khiển trung tâm của chương trình. Ở chương trình triển khai nginx mình triển khai trên 2 host (một host làm web-server một web db) do đó trong đây chúng ta có 2 thư mục là web và db (lời khuyên : đối với các chương trình cần triển khai trên nhiều node thì mỗi node chúng ta nên tạo một thư mục chuyên cho node đó như trong ví dụ này.). Trong 2 thư mục này chúng ta có thể thấy 2 thư mục là handlers và tasks, trong 2 thư mục này lại đều có file main.yml , đây là 2 file mà chúng ta sử dụng cú pháp YAMl để điều khiển chương trình. Với thư mục task trong đây sẽ bao gồm các tác vụ như cài đặt, sửa file cấu hình còn với handlers sẽ thực hiện các tác vụ liên quan đến xử lý như restart service hay stop service.</li>
  <li>hosts : Đây là file cấu hình iventory (host) để máy chủ có thể nhận diện được tác vụ này sẽ được thực hiện trên những hosts nào, file host này sẽ quyết định vị trí file cấu hình mà ansible phải thực hiện.</li>
  <li>main.yml : file triển khai điều khiển, nó sẽ gọi các tác vụ trong roles để thực hiện.</li>
  <li>templates: đây là file được viết bằng jinja2 , nếu như chúng ta có một file cấu hình hệ thống khác với cấu hình mặc định thì chúng ta nên làm 1 file templates và thay thế file cấu hình cũ không nên dùng regex để thay thế như thế sẽ mất rất nhiều thời gian cũng như các rủi ro về lỗi và cú pháp.</li>
 </ul>


- Bây giờ chúng ta thực hiện chạy file playbook main.yml rồi đi pha 1 ly cafe ngồi đợi thôi .

```sh
cd /etc/ansible
ansible-playbook main.yml
```

#Finish !!!