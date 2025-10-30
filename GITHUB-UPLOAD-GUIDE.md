# 📁 GITHUB UPLOAD GUIDE

## ❌ DO NOT UPLOAD THESE FILES:

### Build Files (Auto-generated)
- `dist/` folder - Created by `npm run build`
- `build/` folder - Alternative build output
- `node_modules/` - Dependencies (huge folder)

### Environment Files (Sensitive)
- `.env.local` - Contains your Supabase keys
- `.env` - Any environment variables
- `.env.production` - Production secrets

### IDE/System Files
- `.vscode/` - VS Code settings
- `.DS_Store` - Mac system files
- `Thumbs.db` - Windows thumbnails

## ✅ UPLOAD THESE FILES:

### Source Code
- `src/` folder - All your React components
- `public/` folder - Static assets (images, favicon, etc.)
- `index.html` - Main HTML file

### Configuration Files
- `package.json` - Dependencies and scripts
- `package-lock.json` - Exact dependency versions
- `vite.config.ts` - Vite configuration
- `tailwind.config.ts` - Tailwind CSS config
- `tsconfig.json` - TypeScript configuration
- `vercel.json` - Vercel deployment config

### Documentation
- `README.md` - Project documentation
- `*.md` files - All markdown documentation

### Important Files
- `.gitignore` - Tells Git what NOT to upload
- `supabase-schema.sql` - Database schema
- `.env.example` - Template for environment variables

## 🚀 GITHUB UPLOAD STEPS:

### 1. Initialize Git Repository
```bash
cd shot-link-scribe-main
git init
git add .
git commit -m "Initial commit: Mitalgic website"
```

### 2. Create GitHub Repository
1. Go to [github.com](https://github.com)
2. Click "New repository"
3. Name: `mitalgic-website`
4. Description: `Mitalgic Global - Industrial Engineering Solutions Website`
5. Set to **Public** (for Vercel free tier)
6. Don't initialize with README (you already have one)

### 3. Connect and Push
```bash
git remote add origin https://github.com/YOUR_USERNAME/mitalgic-website.git
git branch -M main
git push -u origin main
```

## 📦 WHAT GETS UPLOADED:

### Project Structure on GitHub:
```
mitalgic-website/
├── src/                    # ✅ Source code
├── public/                 # ✅ Static assets
├── package.json           # ✅ Dependencies
├── vite.config.ts         # ✅ Build config
├── vercel.json           # ✅ Deployment config
├── README.md             # ✅ Documentation
├── .gitignore            # ✅ Ignore rules
└── *.md files            # ✅ All guides

# ❌ NOT UPLOADED:
├── dist/                 # Build output
├── node_modules/         # Dependencies
├── .env.local           # Secrets
└── .vscode/             # IDE settings
```

## 🔧 VERCEL DEPLOYMENT PROCESS:

### What Vercel Does:
1. **Clones** your GitHub repository
2. **Installs** dependencies: `npm install`
3. **Builds** the project: `npm run build`
4. **Deploys** the `dist/` folder to CDN
5. **Serves** your website globally

### Why You Don't Upload `dist/`:
- Vercel builds it automatically
- Keeps repository clean and small
- Ensures fresh builds every deployment
- Reduces upload time and storage

## 🔐 SECURITY NOTES:

### Environment Variables:
- **NEVER** upload `.env.local` to GitHub
- Add environment variables directly in Vercel Dashboard
- Use `.env.example` as a template for others

### Sensitive Data:
- Supabase keys are in `.env.local` (not uploaded)
- API keys stay secure in Vercel environment
- Database credentials never exposed in code

## ✅ FINAL CHECKLIST:

Before uploading to GitHub:
- [x] `.gitignore` file created
- [x] `.env.local` excluded from upload
- [x] `dist/` folder excluded
- [x] `node_modules/` excluded
- [x] All source code included
- [x] Documentation files included
- [x] Configuration files included

## 🚀 AFTER GITHUB UPLOAD:

1. **Connect to Vercel**:
   - Import GitHub repository
   - Add environment variables
   - Deploy automatically

2. **Repository Size**:
   - Should be ~50-100MB (without node_modules)
   - Fast to clone and deploy
   - Easy to manage and update

3. **Continuous Deployment**:
   - Push to GitHub = Auto-deploy to Vercel
   - No manual build/upload needed
   - Version control for all changes

**REMEMBER**: Upload source code, NOT build files. Vercel handles the building and deployment automatically!