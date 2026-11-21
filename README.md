<div align="center">

# Retro Player

**Trình phát nhạc cá nhân chạy hoàn toàn trên trình duyệt, phong cách retro gamer + minimalism.**

Một file HTML duy nhất. Không framework, không build, không backend.

![version](https://img.shields.io/badge/version-1.3.1-5b8cff?style=flat-square)
![single file](https://img.shields.io/badge/single-file-ffd166?style=flat-square)
![no build](https://img.shields.io/badge/build-none-243056?style=flat-square)
![backend](https://img.shields.io/badge/backend-none-243056?style=flat-square)
![theme](https://img.shields.io/badge/theme-dark%20navy-0b1020?style=flat-square)

[Demo](https://90minu93.github.io/baoboi/) · [Bắt đầu nhanh](#bắt-đầu-nhanh) · [Nguồn nhạc](#nguồn-nhạc-hỗ-trợ) · [Tính năng](#tính-năng) · [Nhúng vào web](#nhúng-vào-trang-khác) · [Tùy biến](#tùy-biến) · [Hạn chế](#hạn-chế-đã-biết) · [Kế hoạch](#kế-hoạch)

</div>

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
| **Nhiều playlist** | Đã có ở lớp dữ liệu và API (tạo, đổi tên, xóa, chuyển, chuyển bài giữa các list). **Chưa có giao diện quản lý**, xem mục Kế hoạch |
| **Metadata tự động** | oEmbed (YouTube, SoundCloud), thẻ ID3 và ước tính bitrate với mp3 trực tiếp |
| **Add link file trên máy** | Bị chặn kèm thông báo rõ ràng. Nút **Play** vẫn phát tạm, không lưu vào playlist |
| **Âm lượng** | Thanh chia ô vuông như thanh máu game. Trên thiết bị không cho web chỉnh âm lượng (iOS) sẽ ẩn kèm ghi chú, xem mục Hạn chế |
| **Hẹn giờ tắt** | 15 / 30 / 45 / 60 / 90 phút (chưa lưu qua lần tải lại trang) |
| **Xuất / nhập JSON** | Xuất tất cả playlist (`v:2`), nhập được cả `v:1` lẫn `v:2`, bỏ bài trùng |
| **Media Session** | Điều khiển từ màn hình khóa, phím media |
| **Giao diện** | Theme tối cố định, bóng đổ cứng, nút lún khi nhấn, icon SVG ăn theo màu theme, tam giác play nhấp nháy ở bài đang phát |
| **Scanline** | Hiệu ứng CRT có sẵn, tắt mặc định |
| **I18N** | Toàn bộ chữ giao diện gom trong một khối, dễ thêm ngôn ngữ |

---

## Lưu trữ dữ liệu

Playlist lưu trong `localStorage` của trình duyệt, khóa **`musicplayer-v2`**. Playlist gắn với từng trình duyệt và từng địa chỉ trang.

- Lần đầu mở bản mới, dữ liệu cũ trong `musicplayer-v1` được **tự chuyển** sang `v2`: toàn bộ bài cũ vào một playlist tên "Default".
- `musicplayer-v1` được **giữ nguyên làm bản sao lưu**, chỉ đọc, không bị ghi hay xóa.
- Nếu `v2` bị hỏng, trang tự dựng lại từ `v1` (hoặc tạo "Default" rỗng), không báo lỗi lên giao diện. Luôn có ít nhất một playlist.

Chuyển sang máy khác: **Export JSON** ở máy cũ, **Import JSON** ở máy mới.

<details>
<summary><b>Cấu trúc lưu <code>musicplayer-v2</code></b></summary>

```json
{
  "v": 2,
  "lists": [
    { "id": "...", "name": "Default", "list": [ { "url": "...", "name": "...", "artist": "...", "thumb": "...", "quality": "...", "m": "..." } ] }
  ],
  "cur": "id playlist đang chọn",
  "idx": 0,
  "vol": 1,
  "plOpen": true,
  "url": "",
  "lang": "en",
  "theme": "dark"
}
```

Các trường `idx`, `vol`, `plOpen`, `url`, `lang`, `theme` giữ nguyên ý nghĩa từ `v1`. `idx` là vị trí bài trong playlist đang chọn. Giá trị `lang` và `theme` lạ được chuẩn hóa về `en` và `dark` khi chạy.

</details>

<details>
<summary><b>Định dạng JSON xuất / nhập</b></summary>

Bản xuất ra (`v:2`, chứa tất cả playlist):

```json
{
  "app": "music-player",
  "v": 2,
  "lists": [
    {
      "name": "Default",
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
  ]
}
```

Quy tắc nhập:

- **`v:1`:** gộp vào playlist đang chọn, bỏ bài trùng link.
- **`v:2`:** mỗi playlist trong file, nếu đã có playlist cùng tên thì gộp vào đó (bỏ bài trùng), nếu chưa có thì tạo mới.
- File sai định dạng thì báo lỗi, dữ liệu đang có không bị đổi.

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

> [!NOTE]
> Chế độ `mini` vẫn dùng được nhưng **không được phát triển thêm**. Mọi cải tiến giao diện chỉ áp dụng cho chế độ `full`.

<details>
<summary><b>API công khai <code>window.MusicPlayer</code></b></summary>

| Hàm | Ghi chú |
|---|---|
| `mount`, `unmount` | Gắn / gỡ trình phát |
| `play`, `pause`, `toggle`, `next`, `prev` | Điều khiển phát |
| `add` | Thêm bài vào playlist đang chọn |
| `getList` | Trả bài của playlist đang chọn |
| `getPlaylists` | Trả `{lists:[{id,name,count}], current}` |
| `createPlaylist(name)` | Trả về id. Tên rỗng dùng tên mặc định, tên trùng thêm hậu tố số. Không tự chuyển sang |
| `renamePlaylist(id, name)` | Đổi tên playlist |
| `deletePlaylist(id)` | Không xóa được playlist cuối cùng. Xóa playlist đang chọn thì chuyển sang playlist đầu tiên còn lại |
| `switchPlaylist(id)` | Dừng nhạc đang phát rồi chuyển |
| `moveTrack(url, fromId, toId)` | Chuyển bài giữa hai playlist, bỏ qua nếu đích đã có link đó |
| `setLang`, `setTheme` | Giá trị không hợp lệ rơi về `en` / `dark` |
| `setVolume`, `setSleep` | Âm lượng, hẹn giờ tắt |
| `exportJSON`, `importJSON` | Xuất / nhập playlist |
| `state`, `version` | Đọc trạng thái, phiên bản |

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
| Nền ô ảnh bìa, hover | `#1b2447` |
| Bóng đổ cứng | `#050810` |

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

Lưu ý: các khóa `play` và `stop` không còn chứa ký tự `▶` và `■` (icon đã chuyển sang SVG), khi dịch đừng thêm lại các ký tự đó.

</details>

<details>
<summary><b>Thêm theme</b></summary>

Thêm một khối CSS `.mp[data-theme="tên"]` chứa các biến `--mp-*`, rồi thêm `tên` vào mảng `THEMES` trong code.

</details>

---

## Hạn chế đã biết

1. Playlist gắn với từng trình duyệt, chuyển máy phải xuất / nhập JSON thủ công.
2. Chất lượng âm thanh chỉ ước tính được với mp3 trực tiếp.
3. File nhạc trên máy (ví dụ `D:/bai.mp3`) không thêm được vào playlist vì trang web không đọc được đường dẫn ổ đĩa, chỉ **Play** tạm được.
4. Google Drive có thể không phát trực tiếp được, khi đó dùng iframe preview của Google (giao diện riêng, không tua được, không đổi tốc độ được).
5. Mp3 trực tiếp đang dùng thanh điều khiển gốc của trình duyệt. YouTube và SoundCloud chưa có thanh tiến trình, chưa tua được.
6. Metadata YouTube / SoundCloud phụ thuộc oEmbed của bên thứ ba, thiếu mạng hoặc bị chặn thì không lấy được.
7. **iPhone (lỗi đã biết, chưa sửa):**
   - Hết bài không tự chuyển sang bài kế tiếp mà dừng hẳn.
   - Bài YouTube mở ra không tự phát, phải bấm pause rồi play.
   - Chỉnh **Volume** không có tác dụng với mọi nguồn (iOS không cho web chỉnh âm lượng bằng code, hãy dùng nút âm lượng của máy).
   - Khóa màn hình: chỉ link mp3 trực tiếp mới phát tiếp.
8. Chưa có shuffle, repeat, hàng đợi, đổi vị trí bài, chỉnh tốc độ phát, giao diện quản lý nhiều playlist.

---

## Kế hoạch

Theo thứ tự dự kiến:

- [ ] Làm sáng dòng bài đang phát trong playlist
- [ ] Dòng "Player by 90minutes" và số phiên bản ở header
- [ ] Đổi nốt các ký tự Unicode còn lại sang SVG
- [ ] Thanh tiến trình ô vuông cho mp3, khung Drive gọn
- [ ] Thanh tiến trình và tua cho YouTube / SoundCloud
- [ ] Chỉnh tốc độ phát
- [ ] Giao diện quản lý playlist (tạo, đổi tên, xóa, chuyển bài giữa các list)
- [ ] Đổi vị trí bài hát trong playlist
- [ ] Thumbnail đĩa than xoay, logo, favicon
- [ ] Lưu hẹn giờ tắt qua lần tải lại trang, thông báo lỗi nguồn nhạc rõ ràng hơn
- [ ] Sửa lỗi iPhone (tự qua bài, tự phát bài kế, Volume)
- [ ] Tùy chọn: hàng đợi "phát tiếp theo"

---

## Ghi chú cho người phát triển

> [!TIP]
> Toàn bộ logic nằm trong một IIFE, chỉ để lộ `window.MusicPlayer`. Code ưu tiên dễ đọc để người khác phát triển tiếp.

- Không đổi tên hay xóa biến `--mp-*`.
- Không đổi khóa `musicplayer-v2`. Không xóa hay ghi đè `musicplayer-v1` (bản sao lưu).
- Giữ khả năng nhập file JSON định dạng `v:1` và `v:2`.
- Mọi chuỗi chữ giao diện nằm trong khối `I18N`, không viết chữ cứng ngoài khối đó.
- Chế độ `mini` không được phát triển thêm.
