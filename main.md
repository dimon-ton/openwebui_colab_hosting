# 🧠 Open WebUI on Colab with persistent chat history

```python
# 1. Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')

# 2. Clone WebUI and install dependencies
!git clone https://github.com/open-webui/open-webui.git
%cd open-webui
!pip install -r requirements.txt

# 3. Create Data folder on Drive (เมื่อครั้งแรก)
!mkdir -p "/content/drive/My Drive/openwebui_data"

# 4. Link backend/data to Drive
!rm -rf backend/data
!ln -s "/content/drive/My Drive/openwebui_data" backend/data

# 5. Install ngrok to expose WebUI
!pip install pyngrok
from pyngrok import ngrok
ngrok.set_auth_token("YOUR_NGROK_AUTHTOKEN")

# 6. Start WebUI and ngrok tunnel
import subprocess, threading
def run_webui():
    subprocess.run(["python3", "-m", "open_webui.serve", "--port", "7860"])
threading.Thread(target=run_webui).start()
public_url = ngrok.connect(7860).public_url
print("🔗 Public WebUI URL:", public_url)

# 7. OPTIONAL: auto-backup (เมื่อ restart)
!cp -r backend/data "/content/drive/My Drive/openwebui_data_backup"
