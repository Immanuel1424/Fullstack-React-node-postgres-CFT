# Fullstack React + Node.js + PostgreSQL Deployment on AWS using CloudFormation

This project sets up a secure, two-tier AWS VPC infrastructure using CloudFormation. It deploys:

- **Public Subnet**: Nginx reverse proxy server
- **Private Subnet**: Fullstack application (React frontend, Node.js backend, PostgreSQL DB)

---

## 📦 Features

- Isolated private and public subnets in a custom VPC  
- EC2 instances for frontend/backend and reverse proxy  
- PostgreSQL database auto-created with a `users` table  
- `systemd` service setup for automatic app start  
- GitHub repo cloning and deployment via EC2 UserData  

---

## 🚀 Getting Started

### 1. **Prepare**

- ✅ Create or select an existing **EC2 Key Pair**  
- ✅ Push your fullstack project to GitHub  
  It should have:

  ```
  your-repo/
  ├── server/       ← Node.js backend (entry: index.js)
  └── client/       ← React frontend (with src/Api.js using API_URL)
  ```

---

### 2. **Update Template**

In the CloudFormation YAML, replace:
```bash
REPO_URL="<REPO_URL_PLACEHOLDER>"
```
with:
```bash
REPO_URL="https://github.com/your-username/your-repo.git"
```

---

### 3. **Deploy via AWS CLI**

```bash
aws cloudformation deploy   --template-file vpc-react-node-cft.yaml   --stack-name fullstack-app   --parameter-overrides KeyName=your-key-name   --capabilities CAPABILITY_NAMED_IAM
```

> ☁️ Ensure the AMI ID (`ami-0fc5d935ebf8bc3bc`) matches a valid Ubuntu 24.04 AMI in your region.

---

## 🔧 Network Architecture

```
VPC (12.0.0.0/24)
├── Public Subnet (12.0.1.0/24)
│   └── NginxInstance (with public IP)
│       - Ports: 80, 443 open to 0.0.0.0/0
│
└── Private Subnet (12.0.2.0/24)
    └── AppInstance (React + Node + PostgreSQL)
        - Ports 3000, 5000, 5432 open only to Public SG
```

---

## 🛠 Services Setup

- **Node.js backend**: Exposed on port `5000`  
- **React frontend**: Served using `serve` on port `3000`  
- **Database**: PostgreSQL with a table `users(id, name, email)`  

---

## ✅ Output

After deployment, check:

- `NginxPublicIP`: Visit this IP in your browser.  
- `PrivateAppInstance`: Useful for SSH via bastion or private inspection  

---

## ❗ Optional Enhancements

- [ ] Add NAT Gateway for permanent internet access in private subnet  
- [ ] Add Nginx reverse proxy config for routing `/api` to backend  
- [ ] Add HTTPS using Let's Encrypt (requires domain)  

---

## 🧼 Cleanup

To delete all AWS resources:

```bash
aws cloudformation delete-stack --stack-name fullstack-app
```

---

## 📄 License

MIT License — feel free to modify and adapt.
