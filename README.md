# Secure Identity Hub & Loan Approval System

This project contains a comprehensive banking system comprising a **Secure Identity Hub** frontend and a **Loan Approval Prediction** backend.

## Project Structure

- `frontend/`: React/Vite application for the user interface.
- `backend/`: FastAPI application for loan prediction and user management.
- `sql/`: Database scripts.

## Prerequisites

- **Python 3.8+**
- **Node.js 16+** & **npm**
- **PostgreSQL** (running locally)

## Setup Instructions

### 1. Environment Variables Setup (IMPORTANT - Security)

**⚠️ SECURITY FIRST: Configure Environment Variables Before Running**

1. **Copy the environment template:**
   ```bash
   cp .env.example .env
   ```
   
   **Note:** The `.env` file in this repository contains only placeholder values and is safe. Once you add your real credentials, Git will ignore future changes to this file (it's in `.gitignore`).

2. **Generate a secure JWT secret key:**
   ```bash
   openssl rand -hex 32
   ```
   Copy the output and paste it as your `JWT_SECRET_KEY` in `.env`

3. **Set your database credentials:**
   - Update `DATABASE_URL` in `.env` with your PostgreSQL password
   - Replace `YOUR_PASSWORD` with your actual PostgreSQL password

4. **Get OpenRouter API key (for AI chatbot):**
   - Visit https://openrouter.ai/ and create an account
   - Get your API key and add it to `OPENROUTER_API_KEY` in `.env`

5. **Verify your .env file:**
   ```bash
   # Your .env should look like this (with real values):
   DATABASE_URL=postgresql+asyncpg://postgres:YourActualPassword@localhost/loan_app_db
   JWT_SECRET_KEY=a1b2c3d4e5f6...  # 64 character hex string from openssl
   OPENROUTER_API_KEY=sk-or-v1-...  # Your actual API key
   ```

**⚠️ Security Warnings:**
- Never commit `.env` file to Git (it's in .gitignore)
- Never share your secrets with anyone
- Use different credentials for development and production
- Rotate credentials regularly (every 90 days recommended)

### 2. Database Setup
1.  Ensure PostgreSQL is running.
2.  Create a database named `loan_app_db`:
    ```bash
    psql -U postgres -c "CREATE DATABASE loan_app_db;"
    ```
3.  Ensure your `.env` file is properly configured with your database credentials (see Environment Variables Setup above).
    > **Note**: The application will fail to start if DATABASE_URL is not set correctly.

### 3. Backend Setup
Navigate to the root directory and install Python dependencies:

```bash
pip install -r backend/requirements.txt
```

Initialize the database tables:

```bash
python backend/create_tables.py
```

Start the backend server:

```bash
uvicorn backend.main:app --reload
```

The API will be available at `http://localhost:8000`.

> **Security Note**: The application will fail to start with clear error messages if any required environment variables (DATABASE_URL, JWT_SECRET_KEY, OPENROUTER_API_KEY) are not properly configured.

### 4. Frontend Setup
Open a new terminal, navigate to the frontend directory:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The frontend will be available at `http://localhost:5173`.

## Features
- **User Authentication**: Secure signup and login with JWT tokens
- **AI-Powered Loan Prediction**: Machine learning-based loan eligibility assessment with SHAP explanations
- **Interactive Dashboard**: Real-time loan status, credit score monitoring, and financial overview
- **Loan Application**: Step-by-step loan application form with AI advisor
- **PDF Report Generation**: Comprehensive RBI-compliant loan analysis reports
- **QR Code Sharing**: Share and download reports on mobile devices
- **Payment Gateway**: Mock payment integration (Card, UPI, Net Banking, Wallets)
- **Profile Management**: Complete user profile and security settings

## Screenshots

### 1. Dashboard - Financial Overview
![Dashboard](screenshots/dashboard.png)
- Real-time credit score monitoring (752/900)
- Active loan tracking with outstanding balance
- AI-powered loan eligibility predictions (₹8,00,000 pre-approved)
- Alerts & notifications center
- AI Credit Advisor chatbot

### 2. My Loans
![My Loans](screenshots/my-loans.png)
- View all active loans with details
- Track loan ID, type, amount, and outstanding balance
- Real-time status updates

### 3. Apply for Loan
![Apply for Loan](screenshots/apply-loan.png)
- AI-Powered Loan Eligibility Advisor
- Personal & Employment information
- Financial details with automatic calculations
- Loan details configuration
- Household information
- Instant eligibility assessment

### 4. Loan Analysis Results
![Loan Analysis](screenshots/loan-analysis.png)
- Comprehensive approval decision (95% approval score)
- Credit score range prediction (710-760)
- Interest rate analysis (12.75%)
- Monthly EMI calculation (₹2,263)
- Total interest breakdown (₹1,35,752)
- Decision factors with AI explanations
- Feature impact analysis
- ML approval score visualization
- Loan cost breakdown chart
- Risk assessment radar chart
- Next steps guidance (KYC, documentation)
- PDF report download & QR code sharing

### 5. Security & Profile Settings
![Profile Settings](screenshots/profile-settings.png)
- Complete account information
- Customer ID: LA26253834
- Email verification status
- Mobile number & address details
- KYC status tracking
- Password management
- Account role information

### 6. Mobile QR Code Download
![QR Code](screenshots/qr-code.png)
- Scan to download report on mobile
- Secure & encrypted link
- 24-hour expiration for security
- Works across all devices

## Deployment
**⚠️ SECURITY WARNING FOR PRODUCTION:**

This repository includes a `.env.example` template file. For production deployment:

1. **Never use the example credentials** - generate new, strong secrets
2. **Never commit `.env` file to Git** - it's in .gitignore for security
3. **Use environment-specific configurations:**
   - Different database credentials for dev/staging/production
   - Rotate JWT secret keys regularly
   - Use secrets management services (AWS Secrets Manager, Azure Key Vault, etc.)
4. **Set proper file permissions:**
   ```bash
   chmod 600 .env  # Only owner can read/write
   ```
5. **Enable security features:**
   - Use HTTPS in production
   - Enable CORS restrictions
   - Implement rate limiting
   - Enable database connection pooling
   - Use environment variable injection from your deployment platform

## Security Best Practices

### Environment Variables
- ✅ All secrets are now stored in environment variables
- ✅ No hardcoded credentials in source code
- ✅ `.env` file is gitignored
- ✅ `.env.example` provides setup template
- ✅ Strong validation for JWT secret key (minimum 32 characters)
- ✅ Clear error messages when required variables are missing

### Credentials Management
- Generate strong, random secrets using `openssl rand -hex 32`
- Never reuse passwords across environments
- Rotate credentials every 90 days minimum
- Use different API keys for development and production
- Store production secrets in secure vaults (not in .env files on servers)

### What Changed (Security Fix)
This repository previously contained hardcoded secrets. The following security issues were fixed:
1. ✅ Removed hardcoded OpenRouter API key from `backend/chatbot_model.py`
2. ✅ Removed hardcoded database password from `backend/database.py` and `.env`
3. ✅ Removed weak default JWT secret from `backend/auth.py`
4. ✅ Added validation to ensure all required environment variables are set
5. ✅ Added `.env.example` with placeholder values
6. ✅ Updated `.gitignore` to ensure `.env` is never committed
7. ✅ Added comprehensive security documentation

**If you're updating from an earlier version:** You must now configure all environment variables in your `.env` file before the application will start.
