# New Dashboard Pages Documentation

## Overview
This document describes the newly created and enhanced pages for the Interlink Dashboard system.

## Pages Created/Updated

### 1. Data Management (`/admin/data-management`)
**Location:** `/src/app/(admin)/admin/data-management/page.tsx`

A comprehensive data management interface that provides:

#### Features:
- **Statistics Dashboard**
  - Real-time counters for Users, Stores, Brands, Products, and Orders
  - Visual representation with icons and color-coded cards
  
- **Export Data**
  - Export Users data as CSV
  - Export Products data as CSV
  - Export Orders data as CSV
  - Export complete database backup
  
- **Import Data**
  - Drag-and-drop file upload interface
  - Support for CSV, JSON, and SQL formats
  - File size limit: 50MB
  - Warning system for data overwrite
  
- **Backup Management**
  - List all backups (automatic and manual)
  - Create manual backups on-demand
  - Download existing backups
  - Delete old backups
  - Automatic backup scheduling
  - Backup type indicators (Auto/Manual)
  - File size display
  - Timestamp for each backup
  
- **Database Maintenance**
  - Optimize database tables
  - Clear application cache
  - Check database integrity

#### Usage:
```tsx
// Navigate to
/admin/data-management

// Export data
Click on any "Export X" button

// Create backup
Click "Create Backup" button

// Download backup
Click download icon next to backup entry

// Delete backup
Click trash icon next to backup entry
```

#### Mock Data:
The page currently uses mock data for demonstration. To connect to real APIs:

1. Create API services in `/lib/services/dataService.ts`:
```typescript
export const dataService = {
  getStats: () => api.get('/api/stats'),
  exportData: (type: string) => api.post('/api/export', { type }),
  importData: (file: File) => api.post('/api/import', file),
  createBackup: () => api.post('/api/backups/create'),
  getBackups: () => api.get('/api/backups'),
  downloadBackup: (id: string) => api.get(`/api/backups/${id}/download`),
  deleteBackup: (id: string) => api.delete(`/api/backups/${id}`),
};
```

2. Update the page to use real API calls

---

### 2. Settings (`/admin/settings`)
**Location:** `/src/app/(admin)/admin/settings/page.tsx`

A comprehensive system settings page with tabbed interface:

#### Features:

##### General Settings Tab
- Site Name configuration
- Site URL
- Admin Email
- Timezone selection (Bangkok, Singapore, UTC, New York)
- Language selection (English, Thai, Chinese)

##### Email Settings Tab
- Email Provider selection (SMTP, SendGrid, Mailgun, Amazon SES)
- SMTP Host configuration
- SMTP Port
- SMTP Username
- SMTP Password
- Configuration tips and warnings

##### Security Settings Tab
- Two-Factor Authentication toggle
- Session Timeout (in minutes)
- Minimum Password Length
- Password Change Requirements toggle

##### Notifications Settings Tab
- Email Notifications toggle
- Order Notifications toggle
- Low Stock Alerts toggle
- System Alerts toggle

##### Backup Settings Tab
- Automatic Backup toggle
- Backup Frequency (Hourly, Daily, Weekly, Monthly)
- Retention Period (in days)

##### API Settings Tab
- Enable API Access toggle
- API Rate Limit (requests per hour)
- API Timeout (in seconds)
- Warning for external integrations

#### Usage:
```tsx
// Navigate to
/admin/settings

// Switch between tabs
Click on any tab in the left sidebar

// Save changes
Click "Save Changes" button at the bottom

// Cancel changes
Click "Cancel" button to discard
```

#### Responsive Design:
- Mobile-friendly tab navigation
- Collapsible sidebar on mobile devices
- Toggle switches for boolean settings
- Form validation built-in

---

### 3. User Management (Already Existing - Enhanced)
**Location:** `/src/app/(admin)/admin/users/page.tsx`

Full CRUD functionality for user management:

#### Features:
- List all users with pagination
- Create new users
- Edit existing users
- Delete users
- Role-based filtering
- Status indicators (Active/Inactive)
- Store assignment for Store Admin and Staff
- Password management (required for new, optional for edit)

---

### 4. Dashboard (Already Existing)
**Location:** `/src/app/(admin)/page.tsx`

Main dashboard with overview statistics and charts.

---

### 5. Authentication (Already Existing)
**Location:** 
- `/src/app/(full-width-pages)/(auth)/signin/page.tsx`
- `/src/app/(full-width-pages)/(auth)/signup/page.tsx`

Login and registration pages with JWT authentication.

---

## Navigation Updates

### Sidebar Navigation
**Location:** `/src/layout/AppSidebar.tsx`

Added "Data Management" menu item to the "Others" section:

```typescript
const othersItems: NavItem[] = [
  {
    icon: <PieChartIcon />,
    name: "Reports",
    path: "/admin/reports",
  },
  {
    icon: <BoxCubeIcon />,
    name: "Data Management",  // NEW
    path: "/admin/data-management",
  },
  {
    icon: <ListIcon />,
    name: "Settings",
    path: "/admin/settings",
  },
];
```

---

## Design Principles

### Color Scheme
- **Blue**: Information, exports, downloads
- **Green**: Success, imports, active states
- **Purple**: Backups, special features
- **Orange/Yellow**: Warnings, cautions
- **Red**: Danger, delete actions
- **Gray**: Neutral, disabled states

### Icons Used
From Heroicons v2:
- `ArrowDownTrayIcon` - Downloads, exports
- `ArrowUpTrayIcon` - Uploads, imports
- `DocumentArrowDownIcon` - Document exports
- `TrashIcon` - Delete actions
- `CircleStackIcon` - Database operations
- `ChartBarIcon` - Statistics
- `ClockIcon` - Time-related info

### Typography
- Page titles: `text-3xl font-bold`
- Section headings: `text-xl font-semibold`
- Card headings: `font-medium`
- Body text: `text-sm`
- Small text: `text-xs`

### Spacing
- Sections: `space-y-6`
- Cards: `p-6`
- Buttons: `px-4 py-2`
- Input fields: `px-4 py-2`

---

## Future Enhancements

### Data Management
1. Real-time backup progress indicator
2. Scheduled backup calendar view
3. Backup restoration functionality
4. Data comparison tools
5. Export format options (Excel, PDF)
6. Import data validation and preview
7. Audit logs for data operations

### Settings
1. Theme customization
2. Logo upload
3. Custom email templates
4. Advanced security options (IP whitelist, rate limiting)
5. Integration settings (payment gateways, shipping providers)
6. Multi-language content management
7. Custom fields configuration

### User Management
1. Bulk user operations
2. User activity logs
3. Permission matrix view
4. User groups/teams
5. Advanced filtering and search
6. Export user data

---

## API Endpoints Needed

### Data Management
```
GET    /api/stats              - Get system statistics
POST   /api/export             - Export data
POST   /api/import             - Import data
GET    /api/backups            - List backups
POST   /api/backups/create     - Create backup
GET    /api/backups/:id/download - Download backup
DELETE /api/backups/:id        - Delete backup
POST   /api/maintenance/optimize - Optimize tables
POST   /api/maintenance/cache-clear - Clear cache
POST   /api/maintenance/integrity - Check integrity
```

### Settings
```
GET    /api/settings           - Get all settings
PUT    /api/settings           - Update settings
GET    /api/settings/:category - Get category settings
PUT    /api/settings/:category - Update category settings
```

---

## Installation & Setup

### 1. Install Dependencies
```bash
cd interlink-frontend-dashboard
npm install
```

### 2. Environment Variables
Create `.env.local`:
```env
NEXT_PUBLIC_API_URL=http://localhost:3001/api
NEXT_PUBLIC_APP_NAME=Interlink Store
```

### 3. Run Development Server
```bash
npm run dev
```

### 4. Build for Production
```bash
npm run build
npm start
```

---

## Troubleshooting

### Icons Not Showing
Make sure Heroicons is installed:
```bash
npm install @heroicons/react
```

### Styling Issues
Verify Tailwind CSS is configured properly in `tailwind.config.ts`

### API Connection Issues
Check that backend is running and NEXT_PUBLIC_API_URL is correct

---

## Testing Checklist

### Data Management Page
- [ ] Statistics cards display correct numbers
- [ ] Export buttons trigger download
- [ ] Import file upload works
- [ ] Create backup creates new entry
- [ ] Download backup initiates download
- [ ] Delete backup removes entry with confirmation
- [ ] Maintenance actions show feedback

### Settings Page
- [ ] All tabs are accessible
- [ ] Form inputs are editable
- [ ] Toggle switches work correctly
- [ ] Save button persists changes
- [ ] Cancel button discards changes
- [ ] Conditional fields show/hide properly
- [ ] Validation errors display

### Navigation
- [ ] Sidebar shows Data Management link
- [ ] All navigation links work
- [ ] Active page is highlighted
- [ ] Mobile menu toggles correctly

---

## Support

For issues or questions:
- Check the main project README.md
- Review API documentation in `/documents/api/`
- Contact the development team

---

## License

Copyright © 2024 Interlink. All rights reserved.
