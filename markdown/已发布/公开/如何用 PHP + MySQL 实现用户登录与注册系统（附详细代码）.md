## 如何用 PHP + MySQL 实现用户登录与注册系统（附详细代码）

### 前言
在现代的 Web 应用中，用户登录与注册功能几乎是必备模块。今天我们来实现一个简易但完整的 PHP 登录注册系统。这个教程会介绍注册、登录、会话管理以及页面跳转等核心功能，代码易懂，适合初学者和想快速搭建验证功能的朋友。

### 技术栈
- **后端**：PHP
- **数据库**：MySQL
- **样式**：基本 HTML + CSS

### 项目结构
为了简单起见，我们的项目结构如下：
```
php-login/
├── db.php               # 数据库连接文件
├── index.php            # 首页，提供登录和注册链接
├── register.php         # 注册页面
├── login.php            # 登录页面
├── welcome.php          # 欢迎页面（登录后可见）
├── logout.php           # 登出页面
└── styles.css           # 样式文件
```

### 准备工作：数据库配置
首先，我们需要创建一个名为 `php_login` 的数据库，然后创建 `users` 表，用于存储用户名和密码。

```sql
CREATE DATABASE php_login;
USE php_login;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);
```

### 1. 数据库连接文件（`db.php`）
我们创建一个数据库连接文件，用于在项目中复用连接。

```php
<?php
$host = 'localhost';
$db = 'php_login';
$user = 'root';  // 数据库用户名
$pass = '';      // 数据库密码

try {
    $pdo = new PDO("mysql:host=$host;dbname=$db;charset=utf8", $user, $pass);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Database connection failed: " . $e->getMessage());
}
?>
```

### 2. 首页（`index.php`）
首页提供“登录”和“注册”链接。

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>欢迎来到我们的系统</h1>
        <p>请选择登录或注册</p>
        <div class="buttons">
            <a href="login.php" class="btn">登录</a>
            <a href="register.php" class="btn">注册</a>
        </div>
    </div>
</body>
</html>
```

### 3. 注册页面（`register.php`）
注册页面允许用户输入用户名和密码，并将其存储在数据库中。

```php
<?php
require 'db.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST['username'];
    $password = $_POST['password'];
    $hashedPassword = password_hash($password, PASSWORD_DEFAULT);

    $stmt = $pdo->prepare("INSERT INTO users (username, password) VALUES (?, ?)");
    try {
        $stmt->execute([$username, $hashedPassword]);
        echo "<script>alert('注册成功！正在跳转到登录页面'); window.location.href='login.php';</script>";
    } catch (Exception $e) {
        $message = "用户名已被占用，请选择其他用户名。";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h2>注册</h2>
        <?php if (isset($message)) { echo "<p class='error'>$message</p>"; } ?>
        <form method="POST" action="register.php">
            <label>用户名</label>
            <input type="text" name="username" required>
            <label>密码</label>
            <input type="password" name="password" required>
            <button type="submit" class="btn">注册</button>
        </form>
        <p>已有账号？<a href="login.php">登录</a></p>
    </div>
</body>
</html>
```

### 4. 登录页面（`login.php`）
登录页面验证用户凭据，并启动会话。

```php
<?php
require 'db.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST['username'];
    $password = $_POST['password'];

    $stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");
    $stmt->execute([$username]);
    $user = $stmt->fetch();

    if ($user && password_verify($password, $user['password'])) {
        session_start();
        $_SESSION['username'] = $username;
        header("Location: welcome.php");
        exit();
    } else {
        $message = "用户名或密码错误。";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h2>登录</h2>
        <?php if (isset($message)) { echo "<p class='error'>$message</p>"; } ?>
        <form method="POST" action="login.php">
            <label>用户名</label>
            <input type="text" name="username" required>
            <label>密码</label>
            <input type="password" name="password" required>
            <button type="submit" class="btn">登录</button>
        </form>
        <p>没有账号？<a href="register.php">注册</a></p>
    </div>
</body>
</html>
```

### 5. 欢迎页面（`welcome.php`）
这是一个仅限登录用户访问的页面，展示欢迎信息。

```php
<?php
session_start();
if (!isset($_SESSION['username'])) {
    header("Location: login.php");
    exit();
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>欢迎</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>欢迎，<?php echo htmlspecialchars($_SESSION['username']); ?>!</h1>
        <p>您已成功登录。</p>
        <a href="logout.php" class="btn">登出</a>
    </div>
</body>
</html>
```

### 6. 登出页面（`logout.php`）
用户可以通过此页面退出登录。

```php
<?php
session_start();
session_unset();
session_destroy();
header("Location: index.php");
exit();
```

### 7. CSS 样式（`styles.css`）
简单的 CSS 样式，让页面看起来更美观。

```css
body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #ff7e5f, #feb47b);
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    color: #333;
}

.container {
    width: 350px;
    padding: 20px;
    background-color: #fff;
    border-radius: 12px;
    box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.2);
    text-align: center;
}

h1, h2 {
    margin-bottom: 15px;
    color: #333;
}

input[type="text"], input[type="password"] {
    width: 100%;
    padding: 10px;
    margin-top: 5px;
    border: 1px solid #ddd;
    border-radius: 8px;
}

.btn {
    display: inline-block;
    margin-top: 20px;
    padding: 10px;
    width: 100%;
    background-color: #ff7e5f;
    color: #fff;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
}
```

### 运行项目
如果你使用 XAMPP，可以将项目放入 `htdocs` 文件夹，通过 `http://localhost/php-login/index.php` 访问。

### 注意

 1. 需要手动调整db.php中有关数据库的相关配置。
 2. 项目打包资源笔者已上传，需要的[点此获取](https://mp.csdn.net/mp_download/manage/download/UpDetailed)。
### 总结
我们成功实现了一个简单的 PHP 用户登录和注册系统。通过这个项目，你可以深入理解 PHP 和 MySQL 的交互，同时掌握[密码哈希](https://blog.csdn.net/weixin_73334344/article/details/143452019)和会话管理等基础知识。希望这个教程对你有所帮助！

---

## Cover 图

![cover_1](如何用 PHP + MySQL 实现用户登录与注册系统（附详细代码）.assets/620088c764ab7539.jpeg)
