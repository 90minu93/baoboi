<div align="center">

# Retro Player

**Trình phát nhạc cá nhân chạy hoàn toàn trên trình duyệt, phong cách retro gamer + minimalism.**

Một file HTML duy nhất. Không framework, không build, không backend.

![version](https://img.shields.io/badge/version-1.2.0-5b8cff?style=flat-square)
![single file](https://img.shields.io/badge/single-file-ffd166?style=flat-square)
![no build](https://img.shields.io/badge/build-none-243056?style=flat-square)
![backend](https://img.shields.io/badge/backend-none-243056?style=flat-square)
![theme](https://img.shields.io/badge/theme-dark%20navy-0b1020?style=flat-square)

[Demo](https://90minu93.github.io/baoboi/) · [Bắt đầu nhanh](#bắt-đầu-nhanh) · [Nguồn nhạc](#nguồn-nhạc-hỗ-trợ) · [Tính năng](#tính-năng) · [Nhúng vào web](#nhúng-vào-trang-khác) · [Tùy biến](#tùy-biến) · [Hạn chế](#hạn-chế-đã-biết) · [Kế hoạch](#kế-hoạch)

</div>

> [!NOTE]
> Tên hiển thị trong giao diện hiện vẫn là "Music Player" và sẽ đổi thành "Retro Player" ở bản cập nhật sau. Tên thư mục `Teo-player` và API `window.MusicPlayer` giữ nguyên.

---

## Bắt đầu nhanh

1. Mở trang demo: https://90minu93.github.io/baoboi/ (hoặc host bất kỳ khác).
2. Dán link nhạc vào ô nhập.
3. Bấm **Play** để nghe thử, hoặc **Add** để thêm vào playlist.
4. Dùng nút bài trước, phát/tạm dừng, bài sau, thanh **Volume** và **Sleep timer**.

> [!WARNING]
> Không mở bằng cách bấm đúp vào file (`file://`). Một số nguồn, nhất là YouTube, không phát được khi trang chạy từ `file://`. Hãy chạy qua `https://` (GitHub Pages) hoặc một server cục bộ.

---

## Nguồn nhạc hỗ trợ

| Nguồn | Cách phát |
|---|---|
| YouTube | iframe ẩn, điều khiển bằng IFrame API (postMessage) |
| SoundCloud | Widget API chính thức, nạp khi phát bài SoundCloud đầu tiên |
| Link trực tiếp `mp3` `m4a` `aac` `ogg` `opus` `wav` `webm` `flac` | thẻ `<audio>` |
| Google Drive | thử `<audio>` qua link tải trực tiếp, lỗi thì chuyển sang iframe preview |

> [!IMPORTANT]
> File Google Drive phải chia sẻ ở chế độ "Bất kỳ ai có đường liên kết".

---

## Tính năng

| | |
|---|---|
| **Playlist** | Thêm, xóa, sửa tên / ca sĩ, chống trùng link, bài trước / sau, tự chuyển bài |
| **Metadata tự động** | oEmbed (YouTube, SoundCloud), thẻ ID3 và ước tính bitrate với mp3 trực tiếp |
| **Âm lượng** | Thanh chia ô vuông như thanh máu game |
| **Hẹn giờ tắt** | 15 / 30 / 45 / 60 / 90 phút (chưa lưu qua lần tải lại trang) |
| **Xuất / nhập JSON** | Chuyển playlist giữa các máy, nhập sẽ gộp và bỏ bài trùng |
| **Media Session** | Điều khiển từ màn hình khóa, phím media |
| **Giao diện** | Theme tối cố định, bóng đổ cứng, nút có hiệu ứng lún, bài đang phát màu vàng |
| **Scanline** | Hiệu ứng CRT có sẵn, tắt mặc định |
| **I18N** | Toàn bộ chữ giao diện gom trong một khối, dễ thêm ngôn ngữ |

---

## Lưu trữ dữ liệu

Playlist lưu trong `localStorage` của trình duyệt, khóa `musicplayer-v1`. Playlist gắn với từng trình duyệt và từng địa chỉ trang.

Chuyển sang máy khác: **Export JSON** ở máy cũ, **Import JSON** ở máy mới.

<details>
<summary><b>Định dạng JSON (nhấn để mở)</b></summary>

```json
{
  "app": "music-player",
  "v": 1,
  "list": [
    {
      "url": "https://example.com/song.mp3",
      "name": "Tên bài",
      "artist": "Ca sĩ",
      "thumb": "",
      "quality": "",
      "m": ""
    }
  ]
}
```

Khi nhập, các bài được gộp vào playlist hiện có và bỏ bài trùng link.

</details>

---

## Nhúng vào trang khác

```html
<div id="app"></div>
<script>
  MusicPlayer.mount(document.getElementById('app'), {
    mode: 'full',   // 'full' = trang đầy đủ, 'mini' = nút ♪ có popup
    lang: 'en',
    theme: 'dark'
  });
</script>
```

Mỗi lúc chỉ có một trình phát hoạt động.

<details>
<summary><b>API công khai <code>window.MusicPlayer</code></b></summary>

| Hàm | Ghi chú |
|---|---|
| `mount`, `unmount` | Gắn / gỡ trình phát |
| `play`, `pause`, `toggle`, `next`, `prev` | Điều khiển phát |
| `add` | Thêm bài vào playlist |
| `setLang`, `setTheme` | Đổi ngôn ngữ / theme |
| `setVolume`, `setSleep` | Âm lượng, hẹn giờ tắt |
| `exportJSON`, `importJSON` | Xuất / nhập playlist |
| `getList`, `state`, `version` | Đọc trạng thái |

</details>

---

## Tùy biến

<details>
<summary><b>Bảng màu</b></summary>

Màu nằm trong các biến CSS `--mp-*`, định nghĩa dưới `[data-theme="dark"]`.

| Vai trò | Giá trị |
|---|---|
| Nền trang | `#0b1020` |
| Khối nổi | `#131a33` |
| Viền | `#243056` |
| Màu nhấn | `#5b8cff` |
| Bài đang phát | `#ffd166` |
| Chữ chính | `#dbe4ff` |
| Chữ phụ | `#7080a8` |

Ở chế độ `mini`, trình phát ăn theo biến màu của trang chủ khi trang đó có định nghĩa.

</details>

<details>
<summary><b>Bật scanline (hiệu ứng CRT)</b></summary>

Thêm thuộc tính `data-scanlines="on"` vào phần tử gốc của trình phát. Ví dụ trong Console (F12):

```js
document.getElementById('app').setAttribute('data-scanlines', 'on');
```

</details>

<details>
<summary><b>Thêm ngôn ngữ</b></summary>

Ngôn ngữ gốc là tiếng Anh (`I18N.en`). Để thêm tiếng Việt, tạo khối `I18N.vi` ngay sau khối `I18N` trong `index.html`, chép các khóa từ `I18N.en` và dịch giá trị. Khóa nào thiếu sẽ tự lấy từ tiếng Anh. Sau đó dùng `lang: 'vi'` khi `mount`.

</details>

<details>
<summary><b>Thêm theme</b></summary>

Thêm một khối CSS `.mp[data-theme="tên"]` chứa các biến `--mp-*`, rồi thêm `tên` vào mảng `THEMES` trong code.

</details>

---

## Hạn chế đã biết

1. Playlist gắn với từng trình duyệt, chuyển máy phải xuất / nhập JSON thủ công.
2. Chất lượng âm thanh chỉ ước tính được với mp3 trực tiếp.
3. iPhone khóa màn hình: chỉ link mp3 trực tiếp mới phát tiếp.
4. Google Drive có thể không phát trực tiếp được, khi đó dùng iframe preview.
5. Metadata YouTube / SoundCloud phụ thuộc oEmbed của bên thứ ba, thiếu mạng hoặc bị chặn thì không lấy được.
6. File nhạc trên máy (ví dụ `D:/bai.mp3`) không thêm được vào playlist vì trang web không đọc được đường dẫn ổ đĩa.
7. Chưa có shuffle, repeat, hàng đợi, nhiều playlist.

---

## Kế hoạch

- [ ] Nhiều playlist (cấu trúc lưu mới `musicplayer-v2`, vẫn nhập được JSON cũ)
- [ ] Đổi tên hiển thị thành Retro Player
- [ ] Thanh tiến trình chia ô cho mp3 trực tiếp
- [ ] Chặn Add file trên máy bằng thông báo rõ ràng (vẫn cho Play tạm)
- [ ] Lưu hẹn giờ tắt qua lần tải lại trang
- [ ] Thông báo lỗi nguồn nhạc rõ ràng hơn
- [ ] Tùy chọn: thanh tiến trình cho YouTube / SoundCloud, hàng đợi, làm lại chế độ `mini`

---

## Ghi chú cho người phát triển

> [!TIP]
> Toàn bộ logic nằm trong một IIFE, chỉ để lộ `window.MusicPlayer`. Code ưu tiên dễ đọc để người khác phát triển tiếp.

- Không đổi tên hay xóa biến `--mp-*`.
- Không đổi khóa `musicplayer-v1` khi chưa có bước chuyển dữ liệu, để playlist cũ không bị mất.
- Giữ khả năng nhập file JSON định dạng `v:1`.
