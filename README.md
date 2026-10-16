# BanhTrungThu FE

Frontend cửa hàng bánh Trung Thu (React 19 + Vite + Tailwind CSS 4), phục vụ qua một Express server có kèm API tư vấn AI (Gemini) tại `POST /api/gemini/recommend`.

## Cấu trúc chính

| Thành phần | Mô tả |
| --- | --- |
| `src/` | Mã nguồn React |
| `server.ts` | Express server: dev dùng Vite middleware, production phục vụ `dist/` |
| `vite.config.ts` | Cấu hình Vite |
| `Dockerfile`, `.dockerignore` | Đóng gói image production |

## Biến môi trường

Copy `.env.example` thành `.env` rồi điền giá trị:

| Biến | Bắt buộc | Mặc định | Mô tả |
| --- | --- | --- | --- |
| `GEMINI_API_KEY` | Không | — | API key Gemini. Không có thì API tư vấn trả về câu trả lời mẫu |
| `GEMINI_MODEL` | Không | `gemini-2.5-flash` | Model Gemini dùng để tư vấn |
| `PORT` | Không | `3000` | Cổng server lắng nghe |

## Chạy local

**Yêu cầu:** Node.js 24+ (production chạy `server.ts` trực tiếp nhờ type stripping có sẵn của Node).

```bash
npm install
cp .env.example .env   # rồi điền GEMINI_API_KEY nếu có
npm run dev            # http://localhost:3000
```

### Build production

```bash
npm run build
npm start
```

## Chạy bằng Docker

Build image:

```bash
docker build -t banhtrungthu-fe .
```

Chạy container:

```bash
docker run -d --name banhtrungthu-fe -p 3000:3000 --env-file .env banhtrungthu-fe
```

Hoặc truyền biến trực tiếp:

```bash
docker run -d -p 3000:3000 -e GEMINI_API_KEY=your_key banhtrungthu-fe
```

Mở http://localhost:3000.

Đổi cổng (ví dụ 8080):

```bash
docker run -d -p 8080:8080 -e PORT=8080 --env-file .env banhtrungthu-fe
```

> Image dùng multi-stage build: stage `build` chạy `npm run build`, stage `runtime` chỉ cài dependencies production và copy `dist/` + `server.ts`. File `.env` không được copy vào image (đã loại trong `.dockerignore`), hãy truyền qua `--env-file` hoặc `-e`.

## Scripts

| Lệnh | Mô tả |
| --- | --- |
| `npm run dev` | Chạy dev server (Vite middleware + API) |
| `npm run build` | Build frontend ra `dist/` |
| `npm start` | Chạy server production |
| `npm run preview` | Xem trước bản build bằng Vite |
| `npm run lint` | Kiểm tra kiểu TypeScript |
