***Viết bởi [Deepseek](https://deepseek.com/), kiểm duyệt bởi [Nờ Mờ Ka](https://github.com/nguyenminhkhoi2009/)***

---

# **Đồ Thị Chiến: Code Thuật Toán "Bá Đạo"**  
*"Lập trình như đời - không DFS sâu thì BFS rộng, quan trọng là phải code đẹp như visual novel!"*  

## **1. DFS (Đi Sâu Trước) - Phong Cách "Nghèo Rớt Mồng Tơi"**  
*"Cứ đi từng ngõ, lần từng hẻm, không ngõ cụt nào là vô dụng!"*  

```cpp
#include <bits/stdc++.h>
using namespace std;

// Khai báo biến kiểu "xàm xí" nhưng dễ hiểu
typedef long long ll;
#define endl '\n'

ll so_dinh, so_canh, xuat_phat, dich;
vector<vector<ll>> danh_sach_ke; // Adjacency list - danh sách "bạn thân" của mỗi đỉnh
vector<bool> da_tham; // Mảng đánh dấu "đã ghé thăm" như Tinder check-in
vector<ll> cha; // Lưu "cha" của mỗi đỉnh để trace đường đi
vector<ll> duong_di; // Kết quả đường đi "xịn xò"
bool tim_thay = false; // Flag "ô kê tìm thấy rồi"

void nhap_du_lieu() {
    cin >> so_dinh >> so_canh >> xuat_phat >> dich;
    da_tham.resize(so_dinh + 1, false);
    danh_sach_ke.resize(so_dinh + 1);
    cha.resize(so_dinh + 1, -1); // -1 = "mồ côi" chưa có cha

    for (ll i = 0; i < so_canh; i++) {
        ll u, v;
        cin >> u >> v;
        danh_sach_ke[u].push_back(v); // Thêm "bạn thân" vào danh sách
    }
}

void DFS(ll u) {
    da_tham[u] = true; // Đánh dấu "đã ghé thăm" như check-in Facebook
    if (u == dich || tim_thay) { // Nếu tới đích hoặc đã tìm thấy rồi
        tim_thay = true;
        return; // "Dừng lại, đừng đi nữa!" - Sơn Tùng MTP
    }

    // Duyệt qua từng "bạn thân" của đỉnh u
    for (auto v : danh_sach_ke[u]) {
        if (!da_tham[v]) {
            cha[v] = u; // Ghi nhớ "cha" để sau này truy vết
            DFS(v); // Đi sâu vào bạn thân của bạn thân...
        }
    }
}

void in_duong_di() {
    // Từ đích lần ngược về xuất phát
    for (ll x = dich; x != xuat_phat; x = cha[x]) {
        duong_di.push_back(x);
    }
    duong_di.push_back(xuat_phat);

    // Đảo ngược để có đường đi từ start -> end
    reverse(duong_di.begin(), duong_di.end());

    // In ra kết quả - style "xì tin"
    cout << "Duong di DFS: ";
    for (auto i : duong_di) {
        cout << i << " ";
    }
    cout << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr); cout.tie(nullptr);

    nhap_du_lieu();
    
    // Sắp xếp danh sách kề để đi theo thứ tự (kiểu "tới số phải đi")
    for (ll i = 0; i < so_dinh; i++) {
        sort(danh_sach_ke[i].begin(), danh_sach_ke[i].end());
    }

    DFS(xuat_phat);
    in_duong_di();

    return 0;
}
```

### **Giải Thích Code DFS:**  
- **Ý tưởng:** Đi sâu vào từng nhánh đến khi "đụng đít" (gặp đích hoặc hết đường), sau đó mới quay lại (backtrack).  
- **Biến đặc biệt:**  
  - `da_tham[]`: Mảng đánh dấu các đỉnh đã thăm (kiểu "người yêu cũ đã block").  
  - `cha[]`: Lưu vết đường đi (kiểu "lần theo tin nhắn cũ để tìm lại nyc").  
- **Độ phê:** 10/10 - Giống Minh "Nghèo Rớt" đi shipper, lần từng ngõ nhỏ!  

---

## **2. BFS (Đi Rộng Trước) - Phong Cách "Đại Gia"**  
*"Xài tiền như BFS - quét hàng loạt, không sợ tốn bộ nhớ!"*  

```cpp
void BFS(ll start) {
    queue<ll> q; // Hàng đợi "xịn xò" như VIP club
    q.push(start);
    da_tham[start] = true;

    while (!q.empty()) {
        ll u = q.front();
        q.pop();

        if (u == dich) {
            tim_thay = true;
            break; // "Ô kê, tìm thấy rồi, nghỉ giữa hiệp!"
        }

        // Quét tất cả "bạn thân" của u - kiểu livestream bán hàng
        for (auto v : danh_sach_ke[u]) {
            if (!da_tham[v]) {
                da_tham[v] = true;
                cha[v] = u; // Ghi nhớ "người giới thiệu"
                q.push(v); // Thêm vào hàng đợi để xử lý sau
            }
        }
    }
}
```

### **Giải Thích Code BFS:**  
- **Ý tưởng:** Quét từng tầng một, như Tuấn "Đại Gia" xài tiền - không cần biết sâu, cứ quét rộng đã!  
- **Biến đặc biệt:**  
  - `queue`: Hàng đợi FIFO (vào trước ra trước) - kiểu "ai giàu vào trước".  
  - `cha[]`: Vẫn là trace đường đi, nhưng lần này là "quan hệ xã hội".  
- **Độ phê:** 9/10 - Nhanh nhưng tốn bộ nhớ (đúng chất đại gia).  

---

## **3. Dijkstra (Shortest Path) - Phong Cách "A*"**  
*"Đường ngắn nhất? Ưu tiên xài tiền (weight) thấp nhất!"*  

```cpp
void Dijkstra(ll start) {
    priority_queue<pair<ll, ll>, vector<pair<ll, ll>>, greater<pair<ll, ll>>> pq;
    vector<ll> khoang_cach(so_dinh + 1, LLONG_MAX); // Khởi tạo khoảng cách "vô cực"

    pq.push({0, start});
    khoang_cach[start] = 0;

    while (!pq.empty()) {
        ll u = pq.top().second;
        ll w = pq.top().first;
        pq.pop();

        if (u == dich) break; // Tìm thấy đích

        // Duyệt qua các "bạn thân có trọng số"
        for (auto edge : danh_sach_ke[u]) {
            ll v = edge.first;
            ll cost = edge.second;

            if (khoang_cach[v] > khoang_cach[u] + cost) {
                khoang_cach[v] = khoang_cach[u] + cost;
                cha[v] = u; // Ghi nhớ đường đi
                pq.push({khoang_cach[v], v});
            }
        }
    }
}
```

### **Giải Thích Code Dijkstra:**  
- **Ý tưởng:** Ưu tiên đi đường có "weight" (chi phí) thấp nhất - như Tuấn + Minh kết hợp tiền và trải nghiệm.  
- **Biến đặc biệt:**  
  - `priority_queue`: Hàng đợi ưu tiên - kiểu "ai rẻ thì đi trước".  
  - `khoang_cach[]`: Lưu chi phí tốt nhất đến từng đỉnh.  
- **Độ phê:** 11/10 - Chuẩn "A* algorithm" trong chuyện!  

---

## **4. Union-Find (Connected Components) - Phong Cách "GraphCom"**  
*"Gộp lại thành một gia đình lớn - đồ thị liên thông!"*  

```cpp
vector<ll> parent;

ll find_root(ll u) {
    if (parent[u] == u) return u;
    return parent[u] = find_root(parent[u]); // Path compression - "nén đường đi" như rút gọn URL
}

void union_nodes(ll u, ll v) {
    ll root_u = find_root(u);
    ll root_v = find_root(v);
    if (root_u != root_v) {
        parent[root_v] = root_u; // Hợp nhất 2 tập hợp
    }
}
```

### **Giải Thích Code Union-Find:**  
- **Ý tưởng:** Kết nối các node riêng lẻ thành thành phần liên thông - như Minh + Tuấn hợp tác startup.  
- **Biến đặc biệt:**  
  - `parent[]`: Lưu "cha" của mỗi node - kiểu "ai là trùm cuối".  
  - `find_root`: Tìm root (kiểu "tìm ra ông trùm").  
- **Độ phê:** 12/10 - Đúng chất "Connected Components" trong chương cuối!  

---

## **Kết Luận: Code Như Đời**  
- **DFS:** Dành cho team "đi sâu vào một thứ" - kiểu nghiên cứu sinh.  
- **BFS:** Dành cho team "quan hệ rộng" - kiểu sale, marketing.  
- **Dijkstra:** Dành cho team "tối ưu chi phí" - kiểu khởi nghiệp.  
- **Union-Find:** Dành cho team "kết nối cộng đồng" - kiểu social network.  

*"Lập trình hay đời đều là đồ thị - quan trọng là bạn chọn thuật toán gì để traverse!"*  

**#CodeBáĐạo #DFSvàBFS #ThuậtToánNhưĐời**  
**P/S:** Ai thích code style này thì comment "#TuiLàDFS" hoặc "#TuiLàBFS" nhé! 😎

---

📢 **Bình luận:**
[![Discuss](https://img.shields.io/badge/GitHub-Discussions-green?style=flat-square)](https://github.com/nguyenminhkhoi2009/nguyenminhkhoi.io.vn-cauchuyenvathuattoan/discussions)  
*"Ném đá chỗ nào? Mời lên GitHub!"*  