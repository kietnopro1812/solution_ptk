## **Lời giải bài toán "Trò chơi"**

#### **Ý tưởng 1: Dùng 2 vòng lặp (Brute-force)**

*   **Cách làm:**
    1.  Đọc và lưu N số vào một mảng `a`.
    2.  Khởi tạo biến `max_freq = 0` (tần suất lớn nhất) và `lucky_number`.
    3.  Dùng một vòng lặp ngoài (`i` từ 1 đến N) để xét từng phần tử `a[i]`.
    4.  Bên trong, dùng một vòng lặp trong (`j` từ 1 đến N) để đếm xem `a[i]` xuất hiện bao nhiêu lần trong mảng. Gọi là `current_freq`.
    5.  So sánh `current_freq` với `max_freq`:
        *   Nếu `current_freq > max_freq`, cập nhật `max_freq = current_freq` và `lucky_number = a[i]`.
        *   Nếu `current_freq == max_freq`, cập nhật `lucky_number = max(lucky_number, a[i])` để đảm bảo chọn số lớn hơn.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N²)**, do có hai vòng lặp lồng nhau.
    *   **Không gian:** **O(N)** để lưu mảng ban đầu.
    *   **Đánh giá:** Với N ≤ 10000, N² ≈ 10⁸ phép tính. Thuật toán này có nguy cơ cao bị **TLE (Time Limit Exceeded)** và không phải là giải pháp tốt.

---

#### **Ý tưởng 2: Sắp xếp rồi đếm**

*   **Cách làm:**
    1.  Đọc và lưu N số vào một mảng hoặc vector.
    2.  Sử dụng thuật toán sắp xếp (ví dụ `sort`) để sắp xếp mảng theo thứ tự tăng dần. Sau khi sắp xếp, các số giống nhau sẽ nằm cạnh nhau.
    3.  Duyệt qua mảng đã sắp xếp một lần. Trong khi duyệt, đếm số lần xuất hiện của mỗi nhóm số liên tiếp giống nhau.
    4.  Trong quá trình duyệt, duy trì biến `max_freq` và `lucky_number` và cập nhật chúng tương tự như ý tưởng 1 mỗi khi kết thúc một nhóm số.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N log N)**, chủ yếu do bước sắp xếp.
    *   **Không gian:** **O(N)** để lưu mảng.
    *   **Đánh giá:** Với N ≤ 10000, thuật toán này đủ nhanh để **qua được full test**. Đây là một cách tiếp cận khá phổ biến và hiệu quả nếu không sử dụng các cấu trúc dữ liệu đặc biệt.

---

#### **Ý tưởng 3: Dùng Map/Hash Map để đếm tần suất (Tối ưu)**

*   **Cách làm:**
    1.  Sử dụng một cấu trúc dữ liệu `map` hoặc `unordered_map` (hash map) để lưu tần suất. `key` là số trong dãy, `value` là số lần xuất hiện.
    2.  Duyệt qua N số của input. Với mỗi số `x` đọc vào, tăng giá trị của `map[x]` lên 1.
    3.  Sau khi đã có bảng tần suất, duyệt qua tất cả các cặp `(key, value)` trong map.
    4.  Tìm cặp có `value` (tần suất) lớn nhất. Nếu có nhiều cặp có cùng tần suất lớn nhất, chọn cặp có `key` (giá trị số) lớn nhất.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N)** (với `unordered_map`) hoặc **O(N log N)** (với `map`). Cả hai đều rất nhanh.
    *   **Không gian (Bộ nhớ):** **O(K)**, với K là số lượng các số nguyên *duy nhất* trong dãy (K ≤ N).
    *   **Đánh giá:** Đây là thuật toán **tối ưu và tiêu chuẩn** cho các bài toán đếm tần suất. Nó hiệu quả cả về thời gian và không gian.

---

### **Bảng so sánh các ý tưởng**

| Tiêu chí | Ý tưởng 1: Dùng 2 vòng lặp | Ý tưởng 2: Sắp xếp rồi đếm | Ý tưởng 3: Dùng Map (Tối ưu) |
| :--- | :--- | :--- | :--- |
| **Ưu điểm** | - Dễ hình dung nhất.<br>- Không cần cấu trúc dữ liệu phức tạp. | - Hướng tiếp cận tự nhiên.<br>- Code tương đối đơn giản. | - **Tối ưu nhất** về thời gian.<br>- Logic rõ ràng: đếm riêng, tìm max riêng.<br>- Code ngắn gọn. |
| **Nhược điểm** | - **Rất chậm**, dễ bị TLE. | - Chậm hơn một chút so với dùng hash map.<br>- Thay đổi thứ tự mảng gốc. | - Cần kiến thức về `map`/`unordered_map`.<br>- Tốn bộ nhớ hơn O(1). |
| **Độ phức tạp thời gian** | **O(N²)** | **O(N log N)** | **O(N)** hoặc **O(N log N)** |
| **Độ phức tạp không gian**| **O(N)** | **O(N)** | **O(K)** (K ≤ N) |

---
### **Code mẫu full test và giải thích chi tiết**

Đây là code của mình, mình sẽ giải thích ý nghĩa từng dòng để mọi người dễ theo dõi.

```cpp
#include<bits/stdc++.h> // Thư viện tổng hợp, chứa tất cả các thư viện chuẩn cần thiết cho bài này.

// Các macro định nghĩa sẵn để viết code ngắn gọn hơn.
#define name "BAI2"
#define ll long long
#define e '\n'
#define fi first
#define se second
#define N 1000000
#define TIME endl << "Run time: " << (1000 * clock() / CLOCKS_PER_SEC) << "ms" << endl

using namespace std;

// Khai báo một hash map để lưu tần suất. Key là số, value là số lần xuất hiện của nó.
// Unordered_map giúp truy cập và cập nhật phần tử với tốc độ trung bình O(1).
unordered_map<int, int> mp;

// Biến pair để lưu kết quả: {số may mắn, tần suất}.
// Khởi tạo tần suất là -N, một số âm rất nhỏ để đảm bảo cặp (số, tần suất) đầu tiên tìm được sẽ luôn được chọn làm đáp án tạm thời.
pair<int, int> ans = {0, -N};

// Biến n lưu số lượng phần tử.
int n;

int main() {
	// Các dòng lệnh này giúp tối ưu tốc độ nhập/xuất trong C++, rất quan trọng trong thi đấu.
	ios_base::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	
	// Mở file .INP để đọc dữ liệu và file .OUT để ghi kết quả.
	freopen(name".INP", "r", stdin);
	freopen(name".OUT", "w", stdout);
	
	// Đọc số lượng phần tử N từ input.
	cin >> n;
	
	// Bắt đầu vòng lặp để đọc N số và đếm tần suất của chúng.
	for(int i = 1; i <= n; i++) {
		int x; cin >> x; // Đọc một số nguyên x từ input.
		mp[x]++;         // Tăng tần suất của số x lên 1. Nếu x chưa có trong map, nó sẽ được tự động tạo với giá trị 1.
	}
	
	// Sau khi có bảng tần suất, duyệt qua tất cả các cặp (số, tần suất) trong map để tìm ra đáp án.
	// 'auto x' ở đây sẽ là một pair<int, int>.
	for(auto x : mp) {
		// Nếu tần suất của số hiện tại (x.second) lớn hơn tần suất lớn nhất đã tìm thấy (ans.second)...
		if(x.second > ans.second) {
            ans = x; // ...thì cập nhật ans bằng cặp (số, tần suất) hiện tại.
        }
		// Ngược lại, nếu tần suất bằng nhau...
		else if(x.second == ans.second && x.first < x.second) {
            // ...code sẽ kiểm tra một điều kiện đặc biệt: nếu giá trị của số (x.first) nhỏ hơn tần suất của chính nó (x.second) thì mới cập nhật ans.
            ans = x;
        }
	}
	
	// In ra kết quả cuối cùng: số may mắn (ans.first) và tần suất của nó (ans.second).
	cout << ans.first << " " << ans.second;
	
	// Dòng này chỉ để mình kiểm tra thời gian chạy chương trình, nó sẽ in ra màn hình console chứ không ghi vào file output.
	cerr << TIME;
	return 0;
}	
```

### **Phân tích tóm tắt code mẫu**

Code của mình giải quyết bài toán theo hướng tiếp cận tối ưu **O(N)** bằng cách sử dụng `unordered_map`.

1.  **Đếm tần suất:** Điểm mấu chốt đầu tiên là dùng `unordered_map<int, int> mp` để đếm số lần xuất hiện của mỗi số. Việc duyệt qua N số và thực hiện `mp[x]++` chỉ mất thời gian O(N) vì mỗi thao tác trên hash map có độ phức tạp trung bình là O(1).

2.  **Tìm kết quả:** Sau khi có đầy đủ tần suất, mình duyệt qua map một lần nữa. Logic tìm kiếm được chia làm hai trường hợp:
    *   **Tìm tần suất lớn hơn:** Nếu gặp một số có tần suất cao hơn tần suất lớn nhất hiện tại, mình cập nhật ngay lập tức `ans`.
    *   **Xử lý khi tần suất bằng nhau:** Khi tần suất bằng nhau, code có một logic so sánh riêng là `x.first < x.second`. Tức là nó sẽ ưu tiên chọn số `x` nếu giá trị của số đó nhỏ hơn tần suất của chính nó. Đây là quy tắc xử lý trường hợp hòa (tie-breaking) được cài đặt trong code.

Cách tiếp cận này vừa nhanh về mặt thời gian, vừa rõ ràng về mặt logic, là phương pháp tiêu chuẩn cho dạng bài đếm tần suất.
