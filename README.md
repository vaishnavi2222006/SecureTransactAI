 (cd "$(git rev-parse --show-toplevel)" && git apply --3way <<'EOF' 
diff --git a/README.md b/README.md
new file mode 100644
index 0000000000000000000000000000000000000000..3d197dc4c3ed8b1971d19dc5b132476b5811f0ee
--- /dev/null
+++ b/README.md
@@ -0,0 +1,88 @@
+# SecureTransact AI – Real-Time Fraud Detection Platform
+
+SecureTransact AI is a microservice-based fraud detection platform for financial transaction monitoring.
+It combines:
+
+- **React frontend** for dashboards and transaction operations
+- **Node.js/Express API** for authentication and transaction management
+- **FastAPI ML service** for fraud probability scoring
+- **MongoDB** for persistence
+
+## Architecture
+
+1. User logs in and receives JWT.
+2. User submits transaction from React dashboard.
+3. Node API stores transaction and calls FastAPI `/predict`.
+4. FastAPI returns fraud probability + risk level.
+5. Node updates transaction and returns result.
+
+## Project Structure
+
+```
+SecureTransactAI/
+├── client/
+├── server/
+├── ml-service/
+└── README.md
+```
+
+## Quick Start
+
+### 1) ML Service
+
+```bash
+cd ml-service
+python -m venv .venv
+source .venv/bin/activate
+pip install -r requirements.txt
+python train_model.py
+uvicorn app:app --reload --port 8000
+```
+
+### 2) Node API
+
+```bash
+cd server
+cp .env.example .env
+npm install
+npm run dev
+```
+
+### 3) React Client
+
+```bash
+cd client
+npm install
+npm run dev
+```
+
+## Environment Variables
+
+### server/.env
+
+- `PORT=5000`
+- `MONGO_URI=mongodb://localhost:27017/securetransact`
+- `JWT_SECRET=change_me`
+- `ML_SERVICE_URL=http://127.0.0.1:8000`
+- `CLIENT_ORIGIN=http://localhost:5173`
+
+## API Overview
+
+### Auth
+- `POST /api/auth/register`
+- `POST /api/auth/login`
+- `GET /api/auth/me`
+
+### Transactions
+- `POST /api/transactions`
+- `GET /api/transactions`
+- `GET /api/transactions/summary`
+
+### ML Service
+- `POST /predict`
+- `GET /health`
+
+## Notes
+
+- The ML model uses synthetic data by default so the project is runnable out-of-the-box.
+- Replace training data with Kaggle credit-card fraud dataset for production-quality performance.
 
EOF
)
