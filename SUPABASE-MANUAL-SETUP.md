# 🚀 SUPABASE MANUAL SETUP CHECKLIST

## ✅ COMPLETED
- [x] SQL Schema executed
- [x] Environment variables configured  
- [x] Supabase package installed

## 🔴 REQUIRED MANUAL STEPS

### 1. CREATE STORAGE BUCKETS
**Go to: https://jhumwdvnpcmnvqtbxnva.supabase.co/project/jhumwdvnpcmnvqtbxnva/storage/buckets**

Create these 3 buckets:

#### Bucket 1: `images`
- **Public**: ✅ YES
- **File size limit**: 5MB
- **Allowed MIME types**: `image/jpeg, image/png, image/webp`

#### Bucket 2: `pdfs`
- **Public**: ✅ YES  
- **File size limit**: 10MB
- **Allowed MIME types**: `application/pdf`

#### Bucket 3: `profiles`
- **Public**: ✅ YES
- **File size limit**: 2MB
- **Allowed MIME types**: `image/jpeg, image/png`

### 2. SETUP AUTHENTICATION
**Go to: https://jhumwdvnpcmnvqtbxnva.supabase.co/project/jhumwdvnpcmnvqtbxnva/auth/users**

#### Disable Public Signups:
- Go to **Authentication → Settings**
- Turn OFF "Enable email confirmations"
- Turn OFF "Enable phone confirmations"  
- Set "Site URL" to your domain

#### Create Admin User:
- Go to **Authentication → Users**
- Click "Add user"
- Email: `admin@mitalgic.org`
- Password: `[create secure password]`
- Email confirmed: ✅ YES

### 3. VERIFY API KEYS
**Go to: https://jhumwdvnpcmnvqtbxnva.supabase.co/project/jhumwdvnpcmnvqtbxnva/settings/api**

Confirm these match your `.env.local`:
- **Project URL**: `https://jhumwdvnpcmnvqtbxnva.supabase.co` ✅
- **Anon Key**: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...` ✅
- **Service Role Key**: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...` ✅

### 4. SEED INITIAL DATA
**Go to: https://jhumwdvnpcmnvqtbxnva.supabase.co/project/jhumwdvnpcmnvqtbxnva/editor**

Run this SQL to add initial team members:

```sql
-- Insert team members with existing photos
INSERT INTO team_members (full_name, position, biography, profile_image_url, linkedin_url) VALUES
('Mr Gabriel Ajayi', 'Board Member', 'Accomplished mechanical engineer and PMP® with extensive industry experience across multiple sectors. A certified Project Management Professional with proven track record of leading complex industrial projects.', '/Gabriel-Ajayi.jpg', 'https://www.linkedin.com/in/gabriel-ajayi-27255022b/'),
('Mr Aydin GÖKDENOGLU', 'Board Member', 'Experienced engineer a high-tech Metallurgy Engineering Educator. International expertise in manufacturing and metal processes. Brings valuable academic and practical knowledge to complex projects, a trainer, advisor, writer, patent creator and more.', '/aydin.jpg', 'https://www.linkedin.com/in/ayd%C4%B1n-d%C3%BC%C4%9Fencio%C4%9Flu-84298029/');

-- Insert Power Plant project
INSERT INTO featured_projects (title, description, image_url, project_url, is_featured) VALUES
('Power Plant Modernization', '65% of energy improvement achieved. Delivered 2 weeks ahead of schedule. Zero safety incidents during implementation.', '/power-plant.jpg', '/projects/power-plant-modernization', true);

-- Insert initial services
INSERT INTO services (title, slug, overview_text, what_we_offer_text, process_text, is_active) VALUES
('Engineering Consulting', 'engineering-consulting', 'Expert services including technical guidance, detailed designs, and optimized designs for industrial projects.', 'We provide comprehensive engineering consulting services.', 'Our proven process ensures successful project delivery.', true),
('Assembly & Commissioning', 'assembly-commissioning', 'Turnkey asset installation, stress tests, and exhaustive system validation of equipment.', 'Complete assembly and commissioning services.', 'Systematic approach to equipment installation.', true),
('Maintenance Services', 'maintenance-services', 'Preventive and corrective maintenance with comprehensive spare parts support.', 'Full maintenance service offerings.', 'Proactive maintenance strategies.', true),
('Approved Suppliers List', 'approved-suppliers', 'Comprehensive vendor vetting and strategic procurement services.', 'Vetted supplier network and procurement.', 'Strategic supplier management.', true),
('Business Expansion', 'business-expansion', 'Market entry strategies and international business development.', 'International expansion services.', 'Strategic market entry planning.', true);
```

## 🧪 TEST SETUP

### Test Database Connection
Run this in your project terminal:

```bash
npm run dev
```

Then visit: `http://localhost:3000`

### Test API Endpoints
Create a test file to verify connection:

```javascript
// test-supabase.js
import { supabase } from './src/lib/supabase.js';

async function testConnection() {
  const { data, error } = await supabase.from('contact_info').select('*');
  console.log('Connection test:', { data, error });
}

testConnection();
```

## 📋 BACKEND DEVELOPER HANDOFF

### Project Structure Ready ✅
```
src/
├── types/database.ts          # TypeScript interfaces
├── lib/supabase.ts           # Supabase client
├── hooks/useSupabase.ts      # React hooks
└── utils/supabase-queries.ts # Database queries
```

### Admin Panel Requirements 📋
1. **Authentication**: Use Supabase Auth with admin user
2. **File Uploads**: Use storage buckets created above
3. **CRUD Operations**: Use provided query functions
4. **Validation**: Implement business rules from BACKEND-SETUP-GUIDE.md

### Critical Business Rules ⚠️
- Only 1 featured project at a time
- Unique slugs for services and blogs
- Single contact info row (update only)
- File size and type validation

### Next Steps for Backend Dev 🚀
1. Create admin login page
2. Build CRUD interfaces for each table
3. Implement file upload functionality
4. Add form validations
5. Create dashboard with stats

### Support Files 📁
- `BACKEND-SETUP-GUIDE.md` - Complete technical documentation
- `ADMIN-PANEL-NOTES.md` - UI/UX requirements
- `supabase-schema.sql` - Database schema reference

## 🆘 TROUBLESHOOTING

### Common Issues:
1. **Storage bucket not public**: Check bucket settings
2. **RLS blocking queries**: Verify policies are active
3. **File upload fails**: Check MIME types and size limits
4. **Auth not working**: Verify admin user created

### Support:
- Supabase Dashboard: https://jhumwdvnpcmnvqtbxnva.supabase.co
- Documentation: https://supabase.com/docs