
import zipfile
import os

# Tạo thư mục chứa mã nguồn WebApp AIVT
project_dir = "/mnt/data/AIVT_WebApp"
os.makedirs(project_dir, exist_ok=True)

# Nội dung của index.html cho AIVT WebApp
html_code = """
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>🧠 AIVT-Coin – Blockchain đơn giản</title>
  <style>
    body {
      font-family: system-ui;
      background: #fff;
      color: #000;
      padding: 20px;
    }
    #blockchain-output {
      white-space: pre-wrap;
      background: #f4f4f4;
      border: 1px solid #ccc;
      padding: 10px;
      max-height: 400px;
      overflow-y: auto;
    }
    button {
      padding: 10px;
      margin-top: 10px;
      font-size: 16px;
      width: 100%;
    }
  </style>
</head>
<body>

  <h1>💰 AIVT-Coin Blockchain</h1>
  <button onclick="addNewBlock()">➕ Thêm block mới</button>
  <button onclick="showBlockchain()">📜 Hiển thị chuỗi khối</button>
  <div id="blockchain-output"></div>

  <script>
    class Block {
      constructor(index, timestamp, data, previousHash = '') {
        this.index = index;
        this.timestamp = timestamp;
        this.data = data;
        this.previousHash = previousHash;
        this.hash = this.calculateHash();
      }

      calculateHash() {
        return btoa(this.index + this.timestamp + JSON.stringify(this.data) + this.previousHash)
               .substring(0, 64);
      }
    }

    class Blockchain {
      constructor() {
        this.chain = [this.createGenesisBlock()];
      }

      createGenesisBlock() {
        return new Block(0, Date.now(), "🚀 Khởi tạo AIVT-Coin", "0");
      }

      getLatestBlock() {
        return this.chain[this.chain.length - 1];
      }

      addBlock(newBlock) {
        newBlock.previousHash = this.getLatestBlock().hash;
        newBlock.hash = newBlock.calculateHash();
        this.chain.push(newBlock);
      }

      isChainValid() {
        for (let i = 1; i < this.chain.length; i++) {
          const current = this.chain[i];
          const previous = this.chain[i - 1];

          if (current.hash !== current.calculateHash()) return false;
          if (current.previousHash !== previous.hash) return false;
        }
        return true;
      }
    }

    const AIVT_Coin = new Blockchain();

    function addNewBlock() {
      const newData = prompt("Nhập dữ liệu cho block mới:", "Giao dịch AIVT-Coin");
      if (newData) {
        const newBlock = new Block(AIVT_Coin.chain.length, Date.now(), newData);
        AIVT_Coin.addBlock(newBlock);
        alert("✅ Đã thêm block thành công!");
      }
    }

    function showBlockchain() {
      const output = document.getElementById("blockchain-output");
      output.textContent = JSON.stringify(AIVT_Coin.chain, null, 2);
    }

    window.addEventListener("load", () => {
      console.log("Blockchain hợp lệ?", AIVT_Coin.isChainValid());
    });
  </script>

</body>
</html>
"""

# Ghi file index.html vào thư mục dự án
index_path = os.path.join(project_dir, "index.html")
with open(index_path, "w", encoding="utf-8") as f:
    f.write(html_code)

# Đóng gói toàn bộ mã vào file ZIP
zip_path = "/mnt/data/AIVT_WebApp_Full.zip"
with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
    for root, dirs, files in os.walk(project_dir):
        for file in files:
            full_path = os.path.join(root, file)
            relative_path = os.path.relpath(full_path, project_dir)
            zipf.write(full_path, arcname=relative_path)

zip_path