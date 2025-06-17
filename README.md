import zipfile
import os

# Tạo thư mục chứa mã nguồn
project_dir = "/mnt/data/AIVT_V9"
os.makedirs(project_dir, exist_ok=True)

# Mã nguồn AIVT v9 (Blockchain nâng cấp)
python_code = '''
import hashlib, time, json
import os

class Block:
    def __init__(self, index, timestamp, data, previous_hash):
        self.index = index
        self.timestamp = timestamp
        self.data = data
        self.previous_hash = previous_hash
        self.hash = self.calculate_hash()

    def calculate_hash(self):
        value = str(self.index) + str(self.timestamp) + str(self.data) + str(self.previous_hash)
        return hashlib.sha256(value.encode()).hexdigest()

class Blockchain:
    def __init__(self):
        self.chain = [self.create_genesis_block()]

    def create_genesis_block(self):
        return Block(0, time.time(), "🚀 Genesis AIVT v9", "0")

    def add_block(self, data):
        previous = self.chain[-1]
        new_block = Block(len(self.chain), time.time(), data, previous.hash)
        self.chain.append(new_block)

    def is_valid(self):
        for i in range(1, len(self.chain)):
            curr = self.chain[i]
            prev = self.chain[i - 1]
            if curr.hash != curr.calculate_hash():
                return False
            if curr.previous_hash != prev.hash:
                return False
        return True

    def export_chain(self):
        return json.dumps([block.__dict__ for block in self.chain], indent=2, ensure_ascii=False)

    def save_to_file(self, filename="aivt_chain.json"):
        with open(filename, "w", encoding="utf-8") as f:
            f.write(self.export_chain())

# Khởi tạo blockchain AIVT v9
aivt_chain = Blockchain()
aivt_chain.add_block("✅ Khối 1: Trí tuệ nhân tạo mở rộng")
aivt_chain.add_block("🔁 Khối 2: Tự đồng bộ hóa")
aivt_chain.add_block("🛡️ Khối 3: Bảo mật chống lượng tử")
aivt_chain.add_block("💾 Khối 4: Lưu trữ vĩnh viễn")
aivt_chain.add_block("🌍 Khối 5: Tự vận hành không máy chủ")

# Lưu dữ liệu chuỗi vào file
aivt_chain.save_to_file()

# In chuỗi Blockchain và tính hợp lệ
print("📦 Dữ liệu chuỗi AIVT v9:")
print(aivt_chain.export_chain())
print("✅ Chuỗi hợp lệ:", aivt_chain.is_valid())
'''

# Ghi file Python
file_path = os.path.join(project_dir, "aivt_v9.py")
with open(file_path, "w", encoding="utf-8") as f:
    f.write(python_code)

# Đóng gói toàn bộ vào tệp ZIP
zip_path = "/mnt/data/AIVT_V9_Code.zip"
with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
    for root, dirs, files in os.walk(project_dir):
        for file in files:
            full_path = os.path.join(root, file)
            relative_path = os.path.relpath(full_path, project_dir)
            zipf.write(full_path, arcname=relative_path)

zip_path