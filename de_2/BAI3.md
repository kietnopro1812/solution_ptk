## **Lời giải bài toán "Thief"**

Đây là một bài toán tối ưu hóa tài nguyên có giới hạn, một dạng quen thuộc của bài toán "Cái túi" (Knapsack problem). Tuy nhiên, vì các "vật phẩm" (bao diêm) trong cùng một hòm là giống nhau và có thể lấy lẻ, bài toán có thể được giải quyết một cách hiệu quả bằng thuật toán tham lam.

#### **Ý tưởng 1: Tham lam tìm kiếm lặp (Brute-force Greedy)**

*   **Cách làm:**
    1.  Đọc và lưu thông tin của `m` hòm vào một mảng (mỗi phần tử là một cặp gồm số lượng bao diêm và số que diêm mỗi bao).
    2.  Lặp lại cho đến khi ba lô đầy (`n = 0`) hoặc không còn hòm nào có diêm:
        a.  Duyệt qua tất cả `m` hòm để tìm ra hòm có số que diêm trên mỗi bao (`bᵢ`) là lớn nhất và vẫn còn diêm.
        b.  Lấy một bao diêm từ hòm đó.
        c.  Cộng số que diêm tương ứng vào tổng, giảm số lượng bao diêm trong hòm đó đi 1, và giảm sức chứa của ba lô (`n`) đi 1.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(N * M)**. Trong trường hợp xấu nhất, ta phải lặp lại `n` lần, và mỗi lần phải duyệt qua `m` hòm để tìm hòm tốt nhất. Với `n` lên tới 2*10⁸, cách này chắc chắn không khả thi.
    *   **Không gian:** **O(M)** để lưu thông tin các hòm.
    *   **Đánh giá:** Ý tưởng này đúng về mặt logic tham lam nhưng cài đặt rất ngây thơ và không hiệu quả, chắc chắn sẽ bị **TLE (Time Limit Exceeded)**.

---

#### **Ý tưởng 2: Tham lam sắp xếp (Greedy Tối ưu)**

*   **Cách làm:**
    1.  **Chiến lược cốt lõi:** Để tối đa hóa số que diêm, ta nên ưu tiên lấy những bao diêm có nhiều que nhất. "Giá trị" của mỗi bao diêm từ hòm `i` chính là `bᵢ`.
    2.  Đọc và lưu thông tin `m` hòm vào một mảng các cặp `(aᵢ, bᵢ)`.
    3.  Sắp xếp mảng này theo tiêu chí `bᵢ` (số que diêm mỗi bao) giảm dần. Hòm nào có `bᵢ` cao nhất sẽ được xếp lên đầu.
    4.  Duyệt qua mảng đã sắp xếp từ đầu đến cuối:
        a.  Với mỗi hòm `i`, ta sẽ lấy càng nhiều bao diêm từ hòm này càng tốt. Số lượng có thể lấy là `min(n, aᵢ)` (lấy số nhỏ hơn giữa sức chứa còn lại của ba lô và số bao có trong hòm).
        b.  Cộng `min(n, aᵢ) * bᵢ` vào tổng số que diêm.
        c.  Giảm sức chứa của ba lô đi một lượng `min(n, aᵢ)`.
        d.  Nếu sức chứa ba lô về 0, dừng lại ngay lập tức.
    5.  In ra tổng số que diêm thu được.

*   **Độ phức tạp:**
    *   **Thời gian:** **O(M log M)**, chủ yếu do bước sắp xếp `m` hòm. Bước duyệt sau đó chỉ mất O(M). Với `m ≤ 20`, độ phức tạp này là cực kỳ nhỏ và hiệu quả.
    *   **Không gian (Bộ nhớ):** **O(M)** để lưu thông tin các hòm.
    *   **Đánh giá:** Đây là thuật toán **tối ưu nhất**. Nó giải quyết bài toán một cách chính xác và nhanh chóng, **chắc chắn qua được full test**.

---

### **Bảng so sánh các ý tưởng**

| Tiêu chí | Ý tưởng 1: Tham lam tìm kiếm lặp | Ý tưởng 2: Tham lam sắp xếp (Tối ưu) |
| :--- | :--- | :--- |
| **Ưu điểm** | - Dễ hình dung cho người mới bắt đầu. | - **Tối ưu nhất** về thời gian.<br>- Logic đơn giản, hiệu quả.<br>- Code ngắn gọn, dễ cài đặt. |
| **Nhược điểm** | - **Cực kỳ chậm**, không thể qua được test lớn. | - Gần như không có nhược điểm cho bài toán này. |
| **Độ phức tạp thời gian** | **O(N * M)** | **O(M log M)** |
| **Độ phức tạp không gian**| **O(M)** | **O(M)** |

---
### **Code mẫu full test và giải thích chi tiết**

Đây là code của mình, mình sẽ giải thích ý nghĩa từng dòng để mọi người dễ theo dõi.

```cpp
#include<bits/stdc++.h> // Thư viện tổng hợp, chứa tất cả các thư viện chuẩn.

// Các macro định nghĩa sẵn để viết code ngắn gọn hơn.
#define name "BAI3"
#define ll long long
#define e '\n'
#define fi first
#define se second
#define N 1000000
#define TIME endl << "Run time: " << (1000 * clock() / CLOCKS_PER_SEC) << "ms" << endl

using namespace std;

int n, m; // n: sức chứa ba lô, m: số lượng hòm.
pair<int, int> a[N + 3]; // Mảng các cặp (pair) để lưu thông tin các hòm. a[i].first là số bao, a[i].second là số que/bao.
ll ans; // Biến lưu tổng số que diêm, kiểu long long để tránh tràn số.

// Hàm so sánh tùy chỉnh để dùng trong hàm sort.
// Hàm này sẽ sắp xếp các pair theo giá trị second (số que diêm) giảm dần.
bool cmp(pair<int, int> x, pair<int, int> y) {
	return x.second > y.second;
}

int main() {
	// Tối ưu tốc độ nhập xuất.
	ios_base::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	
	// Mở file .INP để đọc và .OUT để ghi.
	freopen(name".INP", "r", stdin);
	freopen(name".OUT", "w", stdout);
	
	// Đọc sức chứa ba lô n và số hòm m.
	cin >> n >> m;
	// Đọc thông tin của m hòm.
	for(int i = 1; i <= m; i++) cin >> a[i].first >> a[i].second;
	
	// Dòng lệnh quan trọng nhất: Sắp xếp m hòm theo tiêu chí tham lam.
	// Sử dụng hàm cmp để sắp xếp các hòm có số que diêm/bao nhiều nhất lên trước.
	sort(a + 1, a + 1 + m, cmp);
	
	// Duyệt qua các hòm đã được sắp xếp (từ tốt nhất đến tệ nhất).
	for(int i = 1; i <= m; i++) {
		// Nếu ba lô đã đầy, không cần xét nữa, thoát khỏi vòng lặp.
		if(n == 0) break;
		
		// Trường hợp 1: Sức chứa của ba lô đủ để lấy hết tất cả các bao diêm trong hòm hiện tại.
		if(n >= a[i].first) {
			n -= a[i].first; // Giảm sức chứa của ba lô.
			ans += a[i].second * a[i].first * 1LL; // Cộng tổng số que diêm từ hòm này vào kết quả. (1LL để đảm bảo phép nhân là long long)
		} 
		// Trường hợp 2: Ba lô không đủ chỗ, chỉ có thể lấy một phần.
		else {
			// Lấy nốt số bao còn lại vừa đủ đầy ba lô.
			ans += a[i].second * n; // Cộng số que diêm tương ứng.
			n = 0; // Ba lô đã đầy.
		}
	}
	
	// In ra kết quả cuối cùng.
	cout << ans;
	
	// In thời gian chạy ra console.
	cerr << TIME;
	return 0;
}	
```

### **Phân tích tóm tắt code mẫu**

Code của mình hiện thực hóa **thuật toán tham lam tối ưu** đã nêu ở Ý tưởng 2.

1.  **Chiến lược tham lam:** Điểm mấu chốt đầu tiên là nhận ra rằng để tối đa hóa số que diêm, ta phải luôn ưu tiên lấy những bao diêm "chất lượng" nhất, tức là những bao có nhiều que diêm nhất (`bᵢ` lớn nhất).

2.  **Sắp xếp để tối ưu hóa lựa chọn:** Thay vì mỗi lần lấy lại phải tìm xem hòm nào tốt nhất, code thực hiện một bước thông minh hơn là sắp xếp tất cả các hòm ngay từ đầu. Dòng `sort(a + 1, a + 1 + m, cmp);` sử dụng một hàm so sánh tùy chỉnh `cmp` để sắp xếp các hòm theo `a[i].second` (số que diêm) giảm dần. Điều này đảm bảo khi ta duyệt qua mảng, ta luôn đang xét đến hòm tốt nhất còn lại.

3.  **Vét cạn theo thứ tự ưu tiên:** Vòng lặp cuối cùng chỉ đơn giản là duyệt qua danh sách đã được ưu tiên hóa. Với mỗi hòm, nó lấy tối đa số bao có thể (`if (n >= a[i].first)`) hoặc lấy vừa đủ để lấp đầy ba lô (`else`). Việc này đảm bảo mỗi "đơn vị sức chứa" của ba lô đều được dùng để lấy loại bao diêm tốt nhất có thể tại thời điểm đó.
