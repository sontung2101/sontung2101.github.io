Tóm tắt về Design Pattern
Singleton Pattern
Là một Design Pattern để đảm bảo rằng một class chỉ có duy nhất một instance và cung cấp một cách để truy cập tới instance này.
Thường được sử dụng để tạo ra các đối tượng có quan hệ 1-1 với một tài nguyên hoặc cấu hình nào đó.
Trong Python, Singleton Pattern có thể được hiện thực bằng cách sử dụng một biến class để lưu trữ instance, và ghi đè phương thức __new__ để kiểm tra và trả về instance đã được tạo nếu có.
Factory Method Pattern
Là một Design Pattern cho phép tạo ra các đối tượng mà không cần chỉ định trực tiếp lớp đối tượng cụ thể.
Thường được sử dụng khi ta không biết trước chính xác loại đối tượng cần tạo ra cho đến khi chạy thời gian.
Trong Python, Factory Method Pattern có thể được hiện thực bằng cách sử dụng một hàm tạo instance để trả về đối tượng cụ thể. Trong FastAPI, Factory Pattern được sử dụng để tạo ra các đối tượng Dependency Injection.
Chúng ta đã thảo luận về cách hiện thực và sử dụng hai Design Pattern trên và cũng đã cung cấp cho nhau một số ví dụ về cách sử dụng chúng trong thực tế.
