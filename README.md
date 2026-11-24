<div align="center">

# ♪ Retro Player

**Trình phát nhạc cá nhân chạy ngay trên trình duyệt, phong cách retro gamer.**

![Version](https://img.shields.io/static/v1?label=version&message=1.3.9&color=5b8cff&style=flat-square)
![Single file](https://img.shields.io/static/v1?label=single&message=file&color=ffd166&style=flat-square)
![No build](https://img.shields.io/static/v1?label=no%20build&message=no%20backend&color=243056&style=flat-square)

[Demo](https://90minu93.github.io/baoboi/) · [Tính năng](#tính-năng) · [Cách dùng](#cách-dùng) · [Dữ liệu](#dữ-liệu-và-sao-lưu) · [Hạn chế](#hạn-chế-đã-biết) · [Kế hoạch](#kế-hoạch) · [Dùng như thư viện](#dùng-như-thư-viện)

</div>

> [!NOTE]
> Toàn bộ trình phát nằm trong **một file `index.html`** (HTML, CSS, JS thuần). Không framework, không build, không backend, không phụ thuộc phông ngoài. Mở file là chạy.

## Tính năng

- **Nhiều nguồn nhạc:** YouTube, SoundCloud, link trực tiếp tới file âm thanh (mp3, m4a, aac, ogg, oga, opus, wav, webm, flac) và Google Drive.
- **Tự lấy thông tin bài:** tên bài, ca sĩ, ảnh bìa khi thêm bài (YouTube, SoundCloud, thẻ ID3 của mp3). Với link mp3 trực tiếp có ước tính chất lượng (kbps).
- **Nhiều playlist:** tạo, đổi tên, xóa (có hỏi xác nhận), chuyển bài sang playlist khác, đổi vị trí bài bằng nút lên / xuống.
- **Thanh tiến trình và tua:** cho mp3, YouTube, SoundCloud (khi biết thời lượng). Phím mũi tên trái / phải tua 5 giây.
- **Tốc độ phát:** 0.75x đến 2x, giữ nguyên cao độ.
- **Âm lượng và hẹn giờ tắt:** 15, 30, 45, 60 hoặc 90 phút.
- **Media Session:** điều khiển từ màn hình khóa và phím media, có thanh tua trên màn hình khóa.
- **Xuất / nhập JSON** để chuyển playlist giữa các máy.
- **Giao diện retro:** nền navy tối, chữ monospace, viền 2px, bóng đổ cứng, thanh âm lượng chia ô, bài đang phát sáng lên.

## Cách dùng

1. Mở [bản demo](https://90minu93.github.io/baoboi/) hoặc mở `index.html` trong trình duyệt.
2. Dán link bài hát vào ô nhập.
3. Bấm **Play** để phát thử, hoặc **+ Add** để thêm vào playlist.
4. Bấm vào tên bài trong danh sách để phát. Dùng nút **Prev** / **Next** để chuyển bài.

<details>
<summary><b>Loại link được hỗ trợ</b></summary>

| Nguồn | Ví dụ | Ghi chú |
|---|---|---|
| YouTube | link `watch`, `youtu.be`, `shorts`, `live` | Chạy qua trình phát ẩn |
| SoundCloud | link bài hát trên soundcloud.com | Không đổi được tốc độ |
| File âm thanh | link `http(s)` kết thúc bằng `.mp3`, `.m4a`... | Hỗ trợ đầy đủ nhất |
| Google Drive | link chia sẻ file | Phải chia sẻ "Anyone with the link" |

Đường dẫn file trên máy (ví dụ `D:/nhac/bai.mp3`) không thêm được vào playlist.

</details>

## Dữ liệu và sao lưu

Playlist được lưu trong `localStorage` của **chính trình duyệt đang dùng**, không gửi đi đâu cả.

> [!TIP]
> Để dùng trên máy khác hoặc sao lưu: bấm **Export JSON** để tải file, rồi bấm **Import JSON** trên máy kia. Nên xuất định kỳ để phòng khi xóa dữ liệu trình duyệt.

- File xuất có dạng `{"app":"music-player","v":2,"lists":[...]}`.
- Khi nhập, playlist trùng tên được gộp, bài trùng link được bỏ qua. File cũ (`v:1`) và mảng link trần vẫn nhập được.

## Hạn chế đã biết

> [!WARNING]
> Các lỗi dưới đây xảy ra trên **iPhone** (mọi trình duyệt trên iOS dùng chung WebKit). PC (Opera GX) và Android hoạt động bình thường.

1. Hết bài không tự chuyển sang bài kế, mà dừng hẳn.
2. Bài YouTube mở ra hoặc chuyển sang không tự phát, phải bấm pause rồi play.
3. Chỉnh âm lượng trong trình phát không có tác dụng, hãy dùng nút âm lượng của máy.
4. Khóa màn hình: chỉ link file âm thanh trực tiếp mới phát tiếp.

Các hạn chế khác:

- **Google Drive** đôi khi phải dùng khung xem trước của Google: không tua, không đổi tốc độ được và khung hiển thị còn bị cắt nút play.
- **SoundCloud** không hỗ trợ đổi tốc độ.
- Lấy tên bài, ca sĩ, ảnh bìa của file mp3 có thể không được nếu máy chủ chứa file chặn truy cập từ trang khác.
- Thanh tiến trình của YouTube và SoundCloud chỉ hiện khi trình phát báo được thời lượng (livestream thì không có).

## Kế hoạch

- [x] Giao diện retro, bài đang phát sáng lên
- [x] Nhiều playlist, chuyển bài giữa các playlist, đổi vị trí bài
- [x] Thanh tiến trình và tua cho mp3, YouTube, SoundCloud
- [x] Tốc độ phát
- [x] Media Session (màn hình khóa, phím media)
- [ ] Ảnh bìa dạng đĩa than xoay khi đang phát
- [ ] Logo và favicon
- [ ] Nhớ hẹn giờ tắt sau khi tải lại trang
- [ ] Thông báo lỗi nguồn nhạc rõ ràng hơn
- [ ] Sửa khung Drive bị cắt nút play
- [ ] Gỡ code lưu trữ cũ không còn dùng
- [ ] Hàng đợi "phát tiếp theo" (tùy chọn)
- [ ] Sửa các lỗi trên iPhone

> [!IMPORTANT]
> Chế độ `mini` (nút ♪ mở bảng nhỏ) vẫn dùng được nhưng không được phát triển thêm.

## Dùng như thư viện

Trình phát đưa ra một đối tượng toàn cục duy nhất là `window.MusicPlayer`.

```html
<div id="app"></div>
<script>
  MusicPlayer.mount(document.getElementById('app'), { mode: 'full' });
</script>
```

<details>
<summary><b>Các hàm công khai</b></summary>

| Nhóm | Hàm |
|---|---|
| Vòng đời | `mount`, `unmount` |
| Phát | `play`, `pause`, `toggle`, `next`, `prev`, `add` |
| Cài đặt | `setVolume`, `setRate`, `setSleep`, `setLang`, `setTheme` |
| Dữ liệu | `getList`, `exportJSON`, `importJSON`, `state`, `version` |
| Playlist | `getPlaylists`, `createPlaylist`, `renamePlaylist`, `deletePlaylist`, `switchPlaylist`, `moveTrack` |

</details>

<details>
<summary><b>Thêm ngôn ngữ hoặc giao diện</b></summary>

- **Ngôn ngữ:** mọi chữ giao diện nằm trong khối `I18N`. Thêm khối mới (ví dụ `I18N.vi`), khóa thiếu sẽ lấy từ tiếng Anh.
- **Giao diện:** thêm một khối CSS `.mp[data-theme="tên"]` và đưa tên vào mảng `THEMES`.

</details>
