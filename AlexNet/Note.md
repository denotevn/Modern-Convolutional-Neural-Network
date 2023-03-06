+ Trong lớp đầu tiên của AlexNet, hình dạng cửa sổ tích chập là 11 × 11. Vì hầu hết hình ảnh trong ImageNet cao hơn và rộng hơn mười lần so với hình ảnh MNIST, các đối tượng trong dữ liệu ImageNet có xu hướng chiếm nhiều pixel hơn. Do đó, cần có một cửa sổ tích chập lớn hơn để chụp đối tượng. Hình dạng cửa sổ tích chập trong lớp thứ hai được giảm xuống 5×5, tiếp theo là 3×3. Ngoài ra, sau các lớp tích chập thứ nhất, thứ hai và thứ năm, mạng sẽ thêm các lớp tổng hợp tối đa với hình dạng cửa sổ là 3x3 và sải chân là 2. Ngoài ra, AlexNet có số kênh tích chập nhiều gấp mười lần so với LeNet.
+ Sau lớp tích chập cuối cùng, có hai lớp được kết nối đầy đủ với 4096 đầu ra. Hai lớp khổng lồ được kết nối đầy đủ này tạo ra các tham số mô hình gần 1 GB. Do bộ nhớ hạn chế trong các GPU đời đầu, AlexNet ban đầu đã sử dụng thiết kế luồng dữ liệu kép để mỗi GPU trong số hai GPU của họ chỉ có thể chịu trách nhiệm lưu trữ và tính toán một nửa mô hình của nó. May mắn thay, bộ nhớ GPU hiện tương đối dồi dào, vì vậy ngày nay chúng ta hiếm khi cần chia nhỏ các mô hình trên các GPU (phiên bản mô hình AlexNet của chúng tôi khác với bài báo gốc ở khía cạnh này).
+ AlexNet đã thay đổi chức năng kích hoạt sigmoid thành chức năng kích hoạt ReLU đơn giản hơn: 
   + Chức năng kích hoạt ReLU giúp đào tạo mô hình dễ dàng hơn khi sử dụng các phương thức khởi tạo tham số khác nhau
   + Điều này là do, khi đầu ra của hàm kích hoạt sigmoid rất gần với 0 hoặc 1, thì độ dốc của các vùng này gần như bằng 0, do đó lan truyền ngược không thể tiếp tục cập nhật một số tham số mô hình. Ngược lại, độ dốc của hàm kích hoạt ReLU trong khoảng dương luôn là 1. Do đó, nếu các tham số mô hình không được khởi tạo đúng cách, hàm sigmoid có thể thu được độ dốc gần như bằng 0 trong khoảng dương, do đó mô hình không thể được đào tạo một cách hiệu quả.