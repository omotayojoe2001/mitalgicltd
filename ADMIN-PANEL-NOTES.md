# Admin Panel Development Notes

## 🚀 Setup Instructions

### 1. Supabase Project Setup
1. Create new Supabase project at https://supabase.com
2. Run the SQL schema from `supabase-schema.sql` in Supabase SQL Editor
3. Enable Storage bucket for file uploads (images, PDFs)
4. Copy project URL and anon key to `.env.local`

### 2. Environment Variables
```bash
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
```

### 3. Install Dependencies
```bash
npm install @supabase/supabase-js
```

## 📋 Admin Panel Requirements

### Authentication
- Use Supabase Auth for admin login
- Protect all admin routes with authentication middleware
- Single admin user initially (can expand later)

### File Upload Strategy
- Use Supabase Storage for images and PDFs
- Create buckets: `images`, `pdfs`, `profiles`
- Generate public URLs for frontend consumption

## 🎯 Critical Admin Features

### Featured Projects Management
- **MUST HAVE**: Only 1 project can be featured at a time
- Auto-unfeature others when marking new project as featured
- Image upload with preview
- Project URL validation

### Services Management
- **IMPORTANT**: Slug auto-generation from title
- NDT service needs special PDF upload field
- Icon selection from predefined list or upload
- Preview service page before publishing

### Blog Management
- **CRITICAL**: Slug must be unique and URL-friendly
- Rich text editor for full content
- Category dropdown with predefined options
- Auto-save drafts functionality
- Publication date picker

### Team Members
- **REQUIRED**: Profile image upload and crop
- LinkedIn URL validation
- Active/inactive toggle for display control

### Newsletter Management
- Export to CSV functionality
- Bulk actions (delete, export selected)
- Email validation on import

### Contact Info
- **SINGLE ROW**: Only update, never create new
- Social media URL validation
- Phone number formatting
- WhatsApp number with country code

## ⚠️ Important Validations

### Slugs
- Auto-generate from title: `title.toLowerCase().replace(/[^a-z0-9]+/g, '-')`
- Check uniqueness before saving
- No special characters except hyphens

### Images
- Max file size: 5MB
- Allowed formats: JPG, PNG, WebP
- Auto-resize to standard dimensions
- Generate thumbnails for listings

### URLs
- Validate all external URLs (LinkedIn, social media)
- Ensure HTTPS for security
- Test links before saving

## 🔧 Technical Implementation

### Database Queries
- Use provided query functions in `src/utils/supabase-queries.ts`
- Always handle loading and error states
- Implement optimistic updates for better UX

### Real-time Updates
```javascript
// Subscribe to changes for live updates
supabase
  .channel('admin-updates')
  .on('postgres_changes', { event: '*', schema: 'public' }, payload => {
    // Update UI accordingly
  })
  .subscribe()
```

### File Upload Pattern
```javascript
const uploadFile = async (file, bucket, path) => {
  const { data, error } = await supabase.storage
    .from(bucket)
    .upload(path, file);
  
  if (error) throw error;
  
  const { data: { publicUrl } } = supabase.storage
    .from(bucket)
    .getPublicUrl(path);
    
  return publicUrl;
};
```

## 📊 Admin Dashboard Widgets

### Quick Stats
- Total blog posts (published/draft)
- Newsletter subscribers count
- Featured projects count
- Active team members

### Recent Activity
- Latest blog posts
- New newsletter subscribers
- Recent project updates

## 🎨 UI/UX Guidelines

### Forms
- Use consistent validation messages
- Show loading states during saves
- Confirm destructive actions (delete)
- Auto-save drafts every 30 seconds

### Tables
- Sortable columns
- Search/filter functionality
- Pagination for large datasets
- Bulk actions where applicable

### Image Management
- Drag & drop upload
- Image preview before upload
- Crop/resize functionality
- Alt text for accessibility

## 🔒 Security Considerations

### Row Level Security (RLS)
- Already configured in schema
- Public read access for published content
- Admin-only write access

### File Security
- Validate file types server-side
- Scan uploads for malware
- Limit file sizes
- Use signed URLs for sensitive files

## 📱 Responsive Design
- Mobile-friendly admin interface
- Touch-friendly buttons and inputs
- Responsive tables with horizontal scroll
- Mobile image upload support

## 🚨 Error Handling
- Network error recovery
- File upload failure handling
- Form validation errors
- Database constraint violations

## 📈 Performance Tips
- Lazy load images in admin lists
- Paginate large datasets
- Cache frequently accessed data
- Optimize image sizes before upload

## 🔄 Backup Strategy
- Regular database backups via Supabase
- Export functionality for all data
- Version control for content changes
- Recovery procedures documentation