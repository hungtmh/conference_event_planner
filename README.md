# 23127195 - Quản lý và Xóa Tin Nhắn

## 📋 Thông tin

- **MSSV**: 23127195
- **Feature**: Quản lý và Xóa Tin Nhắn (Message Management & Deletion)
- **Self-Evaluated Points**: 10/10

## 🎯 Mô tả tính năng

Tính năng quản lý tin nhắn cho phép người dùng:
1. **Xem lịch sử chat** - Hiển thị toàn bộ tin nhắn với giao diện bubble
2. **Gửi tin nhắn** - Gửi và lưu tin nhắn vào database
3. **Xóa chỉ mình tôi** (Soft Delete) - Ẩn tin nhắn chỉ với người dùng hiện tại
4. **Thu hồi tin nhắn** (Hard Delete) - Xóa vĩnh viễn tin nhắn cho cả 2 phía

## 🏗️ Kiến trúc 3 lớp

```
┌─────────────────────────────────────────────────────────┐
│                    GUI LAYER (Swing)                    │
│  ┌─────────────────┐    ┌─────────────────┐            │
│  │   MainApp.java  │    │  ChatPanel.java │            │
│  └────────┬────────┘    └────────┬────────┘            │
└───────────┼──────────────────────┼──────────────────────┘
            │                      │
            ▼                      ▼
┌─────────────────────────────────────────────────────────┐
│                  SERVICE LAYER (Business)               │
│              ┌─────────────────────────┐                │
│              │   MessageService.java   │                │
│              └───────────┬─────────────┘                │
└──────────────────────────┼──────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    DAO LAYER (JDBC)                     │
│  ┌──────────────────────┐  ┌─────────────────────────┐  │
│  │ DatabaseConnection   │  │     MessageDAO.java     │  │
│  └──────────────────────┘  └─────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   MODEL LAYER (Entity)                  │
│              ┌─────────────────────────┐                │
│              │      Message.java       │                │
│              └─────────────────────────┘                │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    MySQL     │
                    │   Database   │
                    └──────────────┘
```

## 📁 Cấu trúc thư mục

```
23127195/
├── src/
│   ├── model/
│   │   └── Message.java           # Entity class
│   ├── dao/
│   │   ├── DatabaseConnection.java # MySQL connection manager
│   │   └── MessageDAO.java        # Data Access Object
│   ├── service/
│   │   └── MessageService.java    # Business logic
│   └── gui/
│       ├── ChatPanel.java         # Chat UI component
│       └── MainApp.java           # Main entry point
├── lib/
│   └── mysql-connector-j-8.0.33.jar
├── resources/
│   └── database_setup.sql         # SQL script
├── screenshots/
│   ├── 01_main_screen.png
│   ├── 02_chat_history.png
│   ├── 03_send_message.png
│   ├── 04_delete_menu.png
│   └── 05_recall_message.png
├── config.properties              # Database configuration
├── pom.xml                        # Maven dependencies
├── README.md                      # This file
└── MessageFeature.jar             # Executable JAR
```

## 📝 Danh sách Classes và Methods

### 1. Model Layer

#### `model/Message.java`
| Method | Mô tả |
|--------|-------|
| `Message()` | Constructor mặc định |
| `Message(int, int, int, String, Timestamp)` | Constructor đầy đủ |
| `getMessageId()` | Lấy ID tin nhắn |
| `getSenderUsername()` | Lấy username người gửi |
| `getContent()` | Lấy nội dung tin nhắn |
| `getCreatedAt()` | Lấy thời gian gửi |

### 2. DAO Layer (JDBC)

#### `dao/DatabaseConnection.java`
| Method | Mô tả |
|--------|-------|
| `getConnection()` | Tạo kết nối MySQL |
| `closeConnection(Connection)` | Đóng kết nối an toàn |
| `testConnection()` | Test kết nối database |

#### `dao/MessageDAO.java`
| Method | Mô tả |
|--------|-------|
| `getChatHistory(String, String)` | Lấy lịch sử chat giữa 2 người |
| `sendMessage(String, String, String)` | Gửi tin nhắn mới |
| `deleteMessageForMe(int, String)` | Soft delete tin nhắn |
| `recallMessage(int, String)` | Hard delete tin nhắn |
| `getMessageById(int)` | Lấy tin nhắn theo ID |

### 3. Service Layer

#### `service/MessageService.java`
| Method | Mô tả |
|--------|-------|
| `getChatHistory(String, String)` | Lấy lịch sử với validation |
| `sendMessage(String, String, String)` | Gửi tin nhắn với validation |
| `deleteMessageForMe(int, String)` | Xóa tin nhắn cho mình |
| `recallMessage(int, String)` | Thu hồi tin nhắn |
| `isSender(int, String)` | Kiểm tra quyền thu hồi |

### 4. GUI Layer (Swing)

#### `gui/ChatPanel.java`
| Method | Mô tả |
|--------|-------|
| `initializeUI()` | Khởi tạo giao diện |
| `openChat(String, String)` | Mở cuộc chat |
| `loadChatHistory()` | Load lịch sử từ DB |
| `addMessageBubble(...)` | Thêm bubble tin nhắn |
| `showMessageMenu(...)` | Hiện menu xóa/thu hồi |
| `sendMessage()` | Gửi tin nhắn |

#### `gui/MainApp.java`
| Method | Mô tả |
|--------|-------|
| `initializeFrame()` | Khởi tạo JFrame |
| `initializeUI()` | Khởi tạo giao diện |
| `connectChat()` | Kết nối cuộc chat |
| `main(String[])` | Entry point |

## 🔧 Hướng dẫn cài đặt

### Yêu cầu
- Java JDK 11+
- MySQL 5.7+ hoặc 8.0+
- MySQL Connector/J 8.0.33

### Bước 1: Setup Database
```bash
mysql -u root -p < resources/database_setup.sql
```

### Bước 2: Cấu hình kết nối
Sửa file `config.properties`:
```properties
db.url=jdbc:mysql://localhost:3306/chat_app
db.username=root
db.password=your_password
```

### Bước 3: Chạy ứng dụng
```bash
java -jar MessageFeature.jar
```

Hoặc compile từ source:
```bash
javac -cp "lib/*" -d out src/model/*.java src/dao/*.java src/service/*.java src/gui/*.java
java -cp "out;lib/*" gui.MainApp
```

## ✅ Điểm đánh giá

| Tiêu chí | Điểm | Chi tiết |
|----------|------|----------|
| **Swing** | 3/3 | JFrame, JPanel, JScrollPane, JTextField, JButton, JPopupMenu, MouseListener, SwingWorker |
| **JDBC** | 4/4 | PreparedStatement, ResultSet, Connection, CRUD operations, SQL Injection prevention |
| **3-Layer** | 3/3 | Model → DAO → Service → GUI rõ ràng, separation of concerns |
| **Tổng** | **10/10** | |

## 📸 Screenshots

1. **Giao diện chính** - `screenshots/01_main_screen.png`
2. **Lịch sử chat** - `screenshots/02_chat_history.png`
3. **Gửi tin nhắn** - `screenshots/03_send_message.png`
4. **Menu xóa** - `screenshots/04_delete_menu.png`
5. **Thu hồi tin nhắn** - `screenshots/05_recall_message.png`

## 📚 References

- [MySQL Connector/J Documentation](https://dev.mysql.com/doc/connector-j/8.0/en/)
- [Java Swing Tutorial](https://docs.oracle.com/javase/tutorial/uiswing/)
- [JDBC Tutorial](https://docs.oracle.com/javase/tutorial/jdbc/)
