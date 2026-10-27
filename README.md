# Liquid Type — Variable Font Animation v2

Animation thay đổi trục thật của variable font. Không dùng scale/sóng để giả lập biến thiên glyph.

## GitHub Pages
Giải nén, upload toàn bộ nội dung ZIP vào gốc repository:
- index.html
- fontkit.js
- mp4-muxer.js
- fonts/ (giữ nguyên)
- licenses/ (giữ nguyên)

Settings → Pages → Deploy from a branch → main → /(root) → Save. Mở link HTTPS GitHub Pages bằng Chrome/Edge desktop để có khả năng xuất H.264 tốt nhất. Không cần npm/build/server. Vì có ES modules và fetch font, không mở trực tiếp bằng file://; dùng GitHub Pages hoặc localhost.

## Cơ chế
Fontkit đọc variationAxes và nội suy outline/advance width từ dữ liệu variable font, sau đó vẽ glyph vào Canvas. Không có hiệu ứng kéo giãn hình học hay displacement. Blur/threshold là tùy chọn phụ, mặc định tắt.

## Điều khiển
- Mỗi trục thật trong font có checkbox animate, From, To và Phase 0–1.
- Checkbox tắt: trục trở về default của font. Font không có trục sẽ được báo là static.
- Together: các glyph cùng nhịp; Stagger: lệch nhịp từng ký tự; Linear: tăng giảm đều theo sóng tam giác.
- Loop preset 5–30 giây; nhập riêng 0.1–600 giây. Nhịp/loop là số nguyên để khớp hai đầu.
- Size, tracking, line-height, căn chữ, vị trí, màu chữ/mảng nền/viền/canvas, palette, chuyển màu, trong suốt.
- Upload TTF/OTF/WOFF/WOFF2. Một số font phức tạp hoặc định dạng biến thiên không tương thích có thể không đọc được. Fontkit hỗ trợ tốt các TrueType variable font có gvar.
- Save/load preset JSON v2 giữ cả các giá trị trục. File font upload không nằm trong preset; cần upload lại trước khi load preset trên máy khác. Preset v1 không tương thích.
- Font kèm theo từ Google Fonts: Archivo (wght/wdth), Inter (opsz/wght), Unbounded (wght), Space Grotesk (wght). Các trục được lấy trực tiếp từ file, không hardcode giới hạn.

## Export
- PNG: frame hiện tại, hỗ trợ nền trong suốt.
- MP4: H.264, nền màu, không âm thanh, một loop. Cần browser/máy có WebCodecs và H.264. Thử giảm resolution nếu encoder từ chối.
- MOV alpha: codec PNG lossless RGBA, nền trong suốt; không phải ProRes. Import trong phần mềm dựng hỗ trợ QuickTime PNG. Có giới hạn 650 MB frame để tránh hết RAM; giảm fps/resolution/loop khi vượt giới hạn.
- Mỗi chiều tối đa 4096 px, kích thước xuất làm tròn số chẵn cho H.264. Nếu bật Blur, xử lý pixel có thể chậm ở độ phân giải lớn.

## Đã kiểm tra
- Browser hiển thị đúng trục Archivo wght 100–900 và wdth 62–125.
- Glyph thật và advance width thay đổi khi thay các trục; kiểm tra các instance độc lập bằng fontkit.
- Pause/timeline, file MOV 15 frame ở 30fps, 0.5 giây, codec PNG RGBA.
- Môi trường QA thiếu WebCodecs nên chưa kiểm chứng xuất MP4 thực tế; chưa kiểm tra import MOV trực tiếp trong Adobe.

Font licenses trong licenses/. Fontkit và mp4-muxer được đóng gói local. Không cần kết nối Google Fonts/CDN lúc sử dụng. Upload font không gửi file lên server.
