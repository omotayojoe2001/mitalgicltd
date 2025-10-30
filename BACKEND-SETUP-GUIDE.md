# Backend Setup Guide - Mitalgic Website

## 🎯 Project Overview
- **Supabase Project ID**: jhumwdvnpcmnvqtbxnva
- **Project URL**: https://jhumwdvnpcmnvqtbxnva.supabase.co
- **Database**: PostgreSQL with Row Level Security
- **Storage**: File uploads for images and PDFs

## 📋 Manual Setup Checklist

### 1. Database Setup (COMPLETED ✅)
SQL schema has been run. Tables created:
- `featured_projects` - Homepage featured projects
- `services` - All service pages content
- `team_members` - Board members with photos
- `blog_posts` - Blog articles with publishing
- `newsletter_subscribers` - Email subscriptions
- `contact_info` - Social media and contact details

### 2. Storage Buckets Setup (REQUIRED 🔴)
**Go to Supabase Dashboard → Storage → Create Buckets:**

```
Bucket Name: images
- Public: YES
- File size limit: 5MB
- Allowed MIME types: image/jpeg, image/png, image/webp

Bucket Name: pdfs  
- Public: YES
- File size limit: 10MB
- Allowed MIME types: application/pdf

Bucket Name: profiles
- Public: YES  
- File size limit: 2MB
- Allowed MIME types: image/jpeg, image/png
```

### 3. Authentication Setup (REQUIRED 🔴)
**Go to Authentication → Settings:**
- Enable Email authentication
- Disable Sign-ups (admin only)
- Create admin user manually in Users tab

### 4. API Settings (REQUIRED 🔴)
**Go to Settings → API:**
- Verify anon key matches .env.local
- Verify service role key matches .env.local
- Note down JWT secret for admin panel

## 🔧 Backend Development Notes

### Database Schema Details

#### Featured Projects Table
```sql
- id (UUID, Primary Key)
- title (VARCHAR 255) - Project name
- description (TEXT) - Short description for homepage
- image_url (VARCHAR 500) - Project image URL
- project_url (VARCHAR 500) - Link to full project page
- is_featured (BOOLEAN) - Only 1 can be true at a time
- created_at, updated_at (TIMESTAMPS)
```

#### Services Table  
```sql
- id (UUID, Primary Key)
- title (VARCHAR 255) - Service name
- slug (VARCHAR 255, UNIQUE) - URL slug
- overview_text (TEXT) - Service overview
- what_we_offer_text (TEXT) - What we offer section
- process_text (TEXT) - Process description
- image_url (VARCHAR 500) - Service image
- icon_name (VARCHAR 100) - Icon identifier
- ndt_pdf_url (VARCHAR 500) - PDF for NDT service only
- is_active (BOOLEAN) - Show/hide service
```

#### Blog Posts Table
```sql
- id (UUID, Primary Key)
- title (VARCHAR 255) - Blog title
- slug (VARCHAR 255, UNIQUE) - URL slug
- category (VARCHAR 100) - Blog category
- publication_date (DATE) - Publish date
- writer_name (VARCHAR 255) - Author name
- writer_role (VARCHAR 255) - Author position
- read_time (VARCHAR 50) - "5 min read"
- short_summary (TEXT) - Preview text
- full_content (TEXT) - Full article content
- cover_image_url (VARCHAR 500) - Blog cover image
- is_published (BOOLEAN) - Draft/published
- is_featured (BOOLEAN) - Show on homepage
```

### Critical Business Rules

#### Featured Projects
- **ONLY 1 PROJECT** can have `is_featured = true`
- When marking new project as featured, auto-unfeature others
- Required fields: title, description, image_url

#### Services
- **UNIQUE SLUGS** required for URLs
- Auto-generate slug from title: `title.toLowerCase().replace(/[^a-z0-9]+/g, '-')`
- NDT service gets special PDF upload field

#### Blog Posts
- **UNIQUE SLUGS** required for URLs  
- Categories: "Engineering", "Maintenance", "NDT", "Careers", "Industrial Innovation"
- Only published blogs show on frontend
- Featured blogs appear on homepage

#### Contact Info
- **SINGLE ROW** table (id = '1')
- Never INSERT, only UPDATE
- All social media URLs optional

### File Upload Strategy

#### Image Upload Process
```javascript
1. Validate file (type, size)
2. Generate unique filename: `${timestamp}-${originalname}`
3. Upload to appropriate bucket
4. Get public URL
5. Save URL to database
6. Return success/error
```

#### Storage Paths
```
images/projects/project-image-123456.jpg
images/blog-covers/blog-cover-123456.jpg
images/services/service-image-123456.jpg
pdfs/ndt-services/ndt-guide-123456.pdf
profiles/team-members/member-photo-123456.jpg
```

## 🔒 Security Implementation

### Row Level Security Policies (ACTIVE)
```sql
-- Public read access for published content
-- Admin-only write access via service role key
-- Newsletter subscriptions allow public INSERT
```

### Admin Authentication
- Use Supabase Auth with service role key
- Protect all admin routes with middleware
- Session management via JWT tokens

### File Security
- Validate MIME types server-side
- Limit file sizes (images: 5MB, PDFs: 10MB)
- Generate unique filenames to prevent conflicts
- Use signed URLs for sensitive files

## 📊 Admin Panel Requirements

### Dashboard Widgets
```javascript
// Quick stats to display
- Total published blogs: SELECT COUNT(*) FROM blog_posts WHERE is_published = true
- Newsletter subscribers: SELECT COUNT(*) FROM newsletter_subscribers WHERE is_active = true  
- Active services: SELECT COUNT(*) FROM services WHERE is_active = true
- Team members: SELECT COUNT(*) FROM team_members WHERE is_active = true
```

### Essential Admin Features
1. **Content Management**
   - CRUD for all tables
   - Rich text editor for blog content
   - Image upload with preview
   - Drag & drop file uploads

2. **Publishing Controls**
   - Draft/publish toggle for blogs
   - Featured content selection
   - Active/inactive toggles

3. **Data Export**
   - Newsletter subscribers to CSV
   - Blog posts backup
   - Analytics data export

## 🚨 Critical Validations

### Before Save Validations
```javascript
// Slugs must be unique and URL-friendly
const validateSlug = (slug, table, excludeId) => {
  // Check uniqueness in database
  // Ensure format: lowercase, hyphens only
}

// Only one featured project allowed
const validateFeaturedProject = (projectId) => {
  // If marking as featured, unfeature others
}

// External URLs must be valid
const validateUrls = (urls) => {
  // Check HTTPS, valid format
  // Test accessibility
}
```

### File Upload Validations
```javascript
const validateFile = (file, type) => {
  const limits = {
    image: { size: 5 * 1024 * 1024, types: ['image/jpeg', 'image/png', 'image/webp'] },
    pdf: { size: 10 * 1024 * 1024, types: ['application/pdf'] }
  };
  
  // Validate size and MIME type
  // Scan for malware (optional)
}
```

## 🔄 Data Migration & Seeding

### Initial Data Setup
```sql
-- Insert default contact info
INSERT INTO contact_info VALUES (
  '1',
  '+234 708 882 7509',
  'info@mitalgic.org',
  'https://www.linkedin.com/company/81282258/admin/dashboard/',
  'https://www.instagram.com/mitalgic/',
  'https://web.facebook.com/profile.php?id=61561103456114',
  'https://x.com/mitalgic',
  'https://www.youtube.com/@mitalgic',
  '+90 535 742 41 57'
);

-- Insert team members
INSERT INTO team_members (full_name, position, biography, profile_image_url, linkedin_url) VALUES
('Mr Gabriel Ajayi', 'Board Member', 'Accomplished mechanical engineer and PMP® with extensive industry experience...', '/Gabriel-Ajayi.jpg', 'https://www.linkedin.com/in/gabriel-ajayi-27255022b/'),
('Mr Aydin GÖKDENOGLU', 'Board Member', 'Experienced engineer a high-tech Metallurgy Engineering Educator...', '/aydin.jpg', 'https://www.linkedin.com/in/ayd%C4%B1n-d%C3%BC%C4%9Fencio%C4%9Flu-84298029/');
```

## 📈 Performance Optimization

### Database Indexes (CREATED)
- `idx_featured_projects_featured` - Fast featured project lookup
- `idx_services_slug` - Fast service page loading
- `idx_blog_posts_published` - Fast published blog queries
- `idx_blog_posts_slug` - Fast blog detail pages

### Caching Strategy
- Cache published content for 5 minutes
- Cache contact info for 1 hour
- Invalidate cache on admin updates

## 🔧 API Endpoints Structure

### Public API (Frontend)
```
GET /api/projects/featured - Featured projects
GET /api/services - Active services
GET /api/services/[slug] - Service details
GET /api/blog - Published blogs
GET /api/blog/[slug] - Blog details
GET /api/team - Active team members
GET /api/contact - Contact information
POST /api/newsletter - Subscribe to newsletter
```

### Admin API (Backend)
```
POST /api/admin/auth - Admin login
GET /api/admin/dashboard - Dashboard stats
CRUD /api/admin/projects - Project management
CRUD /api/admin/services - Service management
CRUD /api/admin/blog - Blog management
CRUD /api/admin/team - Team management
PUT /api/admin/contact - Update contact info
GET /api/admin/newsletter - Newsletter management
POST /api/admin/upload - File uploads
```

## 🚀 Deployment Notes

### Environment Variables
```bash
# Production
NEXT_PUBLIC_SUPABASE_URL=https://jhumwdvnpcmnvqtbxnva.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=[anon_key]
SUPABASE_SERVICE_ROLE_KEY=[service_role_key]
FORMSUBMIT_EMAIL=info@mitalgic.org
```

### Build Process
1. Install dependencies: `npm install @supabase/supabase-js`
2. Build project: `npm run build`
3. Test admin panel functionality
4. Deploy to production

This setup provides a complete CMS backend for the Mitalgic website with all necessary features for content management.