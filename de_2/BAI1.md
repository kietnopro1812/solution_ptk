## **Lời giải bài toán "Số âm"**


#### **Ý tưởng 1: Sắp xếp rồi tìm kiếm**

*   **Cách làm:**
    1.  Đọc và lưu trữ toàn bộ N số nguyên vào một mảng hoặc vector.
    2.  Sử dụng một thuật toán sắp xếp (ví dụ `sort` trong C++) để sắp xếp mảng theo thứ tự tăng dần.
    3.  Sau khi sắp xếp, duyệt mảng từ cuối về đầu. Phần tử đầu tiên nhỏ hơn 0 mà ta gặp chính là số âm lớn nhất. In ra số đó và kết thúc.
    4.  Nếu duyệt hết mảng mà không tìm thấy số âm nào, có nghĩa là tất cả các số đều không âm. In ra 0.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N log N)**, chủ yếu do bước sắp xếp.
    *   **Đánh giá:** Với N ≤ 10⁵, thuật toán này tốn khoảng 10⁵ * log(10⁵) ≈ 1.7 * 10⁶ phép tính, hoàn toàn đáp ứng được giới hạn thời gian. Do đó, cách này **chắc chắn qua được full test** và không bị TLE (Time Limit Exceeded).

---

#### **Ý tưởng 2: Lọc các số âm ra mảng phụ**

*   **Cách làm:**
    1.  Tạo một mảng/vector phụ để chỉ lưu các số âm.
    2.  Duyệt qua N số của input. Nếu gặp một số `a < 0`, thêm nó vào mảng phụ.
    3.  Sau khi duyệt xong, kiểm tra xem mảng phụ có rỗng không.
        *   Nếu rỗng, in ra 0.
        *   Nếu không, tìm phần tử lớn nhất trong mảng phụ (bằng cách duyệt một lượt hoặc dùng hàm `*max_element`) và in ra kết quả.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N)**, vì ta chỉ duyệt qua dãy số một lần để lọc và duyệt qua mảng phụ (tối đa N phần tử) một lần để tìm max.
    *   **Đánh giá:** Nhanh hơn ý tưởng 1, **chắc chắn qua được full test**. Tuy nhiên, cách này tốn thêm bộ nhớ O(N) để lưu mảng phụ trong trường hợp xấu nhất.

---

#### **Ý tưởng 3: Duyệt một lượt và cập nhật kết quả (Tối ưu)**

*   **Cách làm:**
    1.  Không cần lưu trữ cả dãy số. Ta chỉ cần một biến `ans` để lưu kết quả.
    2.  Khởi tạo `ans` bằng một giá trị rất nhỏ (nhỏ hơn bất kỳ số âm nào có thể có trong input).
    3.  Duyệt qua N số. Với mỗi số `a` đọc vào, ta kiểm tra:
        *   Nếu `a` là số âm (`a < 0`) VÀ `a` lớn hơn `ans` hiện tại (`a > ans`), thì cập nhật `ans = a`.
    4.  Sau khi duyệt xong, kiểm tra giá trị cuối cùng của `ans`. Nếu nó vẫn bằng giá trị khởi tạo ban đầu, nghĩa là không có số âm nào, in ra 0. Ngược lại, in ra `ans`.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N)**, vì chỉ duyệt qua dãy số đúng một lần.
    *   **Không gian (Bộ nhớ):** **O(1)**, vì chỉ sử dụng một vài biến đơn, không phụ thuộc vào N.
    *   **Đánh giá:** Đây là thuật toán **tối ưu nhất** cả về thời gian và không gian.

---

### **Bảng so sánh các ý tưởng**

| Tiêu chí | Ý tưởng 1: Sắp xếp | Ý tưởng 2: Lọc ra mảng phụ | Ý tưởng 3: Duyệt một lượt (Tối ưu) |
| :--- | :--- | :--- | :--- |
| **Ưu điểm** | - Hướng tiếp cận tự nhiên.<br>- Hữu ích nếu có thêm yêu cầu khác trên dãy đã sắp xếp. | - Tách bạch logic rõ ràng (lọc riêng, tìm max riêng). | - **Tối ưu nhất** về thời gian.<br>- **Tiết kiệm bộ nhớ nhất**.<br>- Code ngắn gọn, dễ cài đặt. |
| **Nhược điểm** | - Chậm hơn các cách khác.<br>- Tốn bộ nhớ không cần thiết. | - Tốn bộ nhớ O(N) để lưu mảng phụ. | - Gần như không có. |
| **Độ phức tạp thời gian** | **O(N log N)** | **O(N)** | **O(N)** |
| **Độ phức tạp không gian**| **O(N)** | **O(N)** | **O(1)** |

---

### **Code mẫu full test để tham khảo**

```cpp
#include<bits/stdc++.h>
#define name "BAI1"
#define ll long long
#define e '\n'
#define fi first
#define se second
#define N 1000000
#define TIME endl << "Run time: " << (1000 * clock() / CLOCKS_PER_SEC) << "ms" << endl
using namespace std;

// Khai báo biến: n là số lượng, a là giá trị đọc vào, ans là kết quả
ll n, a, ans = -99999999999999999;

int main() {
	// Tối ưu tốc độ nhập xuất
	ios_base::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	
	// Đọc/ghi file theo yêu cầu của đề bài
	freopen(name".INP", "r", stdin);
	freopen(name".OUT", "w", stdout);
	
	// Đọc số lượng phần tử N
	cin >> n;
	
	// Vòng lặp N lần để đọc từng số và xử lý
	for(int i = 1; i <= n; i++) {
		cin >> a;
		// Logic chính: nếu a là số âm và lớn hơn đáp án hiện tại thì cập nhật
		if(a < 0 && a > ans) ans = a;
	}

	// Nếu ans vẫn là giá trị khởi tạo, nghĩa là không có số âm nào -> in ra 0
	// Ngược lại, in ra số âm lớn nhất tìm được
	if (ans == -99999999999999999) {
	    cout << 0;
	} else {
	    cout << ans;
	}
	
	// In thời gian chạy (chỉ hiển thị trên console, không ghi vào file OUT)
	cerr << TIME;
	return 0;
}	
```

### **Phân tích tóm tắt code mẫu**

```
ans = -99999999999999999;
```
Biến `ans` được khởi tạo với một giá trị "lính canh" (sentinel value). Đây là một số cực nhỏ, đảm bảo rằng bất kỳ số âm nào trong input (với |aᵢ| ≤ 10⁵) cũng sẽ lớn hơn nó. Điều này giúp cho số âm đầu tiên gặp được sẽ ngay lập tức trở thành `ans`.

```
for(int i = 1; i <= n; i++) { cin >> a; ... }
```
Vòng lặp này duyệt qua N lần. Trong mỗi lần lặp, chương trình đọc một số `a` và xử lý ngay lập tức mà không cần lưu vào mảng. Cách làm này giúp tiết kiệm bộ nhớ, đạt độ phức tạp không gian O(1).

```
if(a < 0 && a > ans) ans = a;
```
Đây là dòng lệnh cốt lõi của thuật toán. Nó lọc ra các số âm (`a < 0`) và so sánh để tìm ra số âm có giá trị lớn nhất (`a > ans`). Nếu một số `a` thỏa mãn cả hai điều kiện, nó sẽ trở thành ứng cử viên mới cho kết quả cuối cùng.
