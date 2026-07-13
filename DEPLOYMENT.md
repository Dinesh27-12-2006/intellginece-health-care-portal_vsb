# Deployment Guide

## Quick Start - Deploy to Production

### Option 1: Deploy to Vercel (Recommended)

1. **Install Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **Login to Vercel**
   ```bash
   vercel login
   ```

3. **Deploy**
   ```bash
   vercel --prod
   ```

4. **Set Environment Variables in Vercel Dashboard**
   - Go to your project settings in Vercel
   - Add environment variables:
     - `SUPABASE_URL`: Your production Supabase URL
     - `SUPABASE_ANON_KEY`: Your production anon key

### Option 2: Deploy to Netlify

1. **Install Netlify CLI**
   ```bash
   npm install -g netlify-cli
   ```

2. **Login to Netlify**
   ```bash
   netlify login
   ```

3. **Deploy**
   ```bash
   netlify deploy --prod
   ```

4. **Set Environment Variables**
   ```bash
   netlify env:set SUPABASE_URL "your-supabase-url"
   netlify env:set SUPABASE_ANON_KEY "your-anon-key"
   ```

### Option 3: Deploy via GitHub (Automatic)

#### For Vercel:
1. Go to [vercel.com](https://vercel.com)
2. Click "Import Project"
3. Connect your GitHub repository: `Dinesh27-12-2006/intellginece-health-care-portal`
4. Configure environment variables
5. Click "Deploy"

#### For Netlify:
1. Go to [netlify.com](https://netlify.com)
2. Click "Add new site" → "Import an existing project"
3. Connect your GitHub repository
4. Configure environment variables
5. Click "Deploy"

## Important: Update Supabase Client Configuration

After deployment, you need to update your Supabase client initialization to use environment variables properly.

### Current Issue
Your `js/supabaseClient.js` file has hardcoded values. For production, you should:

1. Keep the `.env.example` file in the repository
2. Never commit the actual `.env` file
3. Set environment variables on your hosting platform

### For Static Sites (Current Setup)
Since this is a static HTML/JS application without a build step, you'll need to:

**Option A: Update values directly in `supabaseClient.js` for production**
- Keep separate versions for development and production
- Use git branches (main for production, develop for development)

**Option B: Add a simple build step (Recommended)**
- Add a build script that replaces placeholders with environment variables
- This keeps your code secure and maintainable

## Supabase Setup for Production

1. **Create Production Supabase Project**
   - Go to [supabase.com](https://supabase.com)
   - Create a new project (separate from development)

2. **Run Database Schema**
   - Open SQL Editor in Supabase Dashboard
   - Copy contents from `supabase/schema.sql`
   - Execute the SQL

3. **Configure Authentication**
   - Go to Authentication → Settings
   - Add your production URL to "Site URL"
   - Add redirect URLs (e.g., `https://your-domain.com/*`)
   - Configure email templates

4. **Enable Realtime**
   - Already configured in schema.sql
   - Verify in Database → Replication

5. **Get API Keys**
   - Project Settings → API
   - Copy URL and anon/public key
   - Add to hosting platform environment variables

## Security Checklist Before Going Live

- ✅ `.env` file is in `.gitignore`
- ✅ No sensitive keys in source code
- ✅ HTTPS enabled (automatic on Vercel/Netlify)
- ✅ RLS policies enabled on all Supabase tables
- ✅ Authentication configured properly
- ✅ CORS settings configured in Supabase
- ✅ Email confirmations enabled
- ✅ Rate limiting considered

## Post-Deployment Steps

1. **Test the live site**
   - Create a test account
   - Book an appointment
   - Verify realtime updates
   - Test on mobile devices

2. **Monitor**
   - Check Supabase logs for errors
   - Monitor hosting platform analytics
   - Set up uptime monitoring (e.g., UptimeRobot)

3. **Configure Custom Domain (Optional)**
   - Purchase domain from provider (Namecheap, GoDaddy, etc.)
   - Add custom domain in Vercel/Netlify settings
   - Update DNS records
   - Wait for SSL certificate provisioning (automatic)

## Troubleshooting

### Issue: Supabase connection fails
- Check API keys are correct
- Verify URL in environment variables
- Check browser console for CORS errors
- Verify redirect URLs in Supabase Auth settings

### Issue: Realtime not working
- Verify Realtime is enabled on tables
- Check RLS policies allow reading
- Inspect browser console for connection errors

### Issue: Authentication not working
- Check Site URL in Supabase Auth settings
- Verify redirect URLs include your production domain
- Check email provider settings (for confirmation emails)

## Continuous Deployment

Once set up with Vercel or Netlify connected to GitHub:
1. Push changes to your main branch
2. Automatic deployment triggers
3. Site updates in 1-2 minutes
4. Rollback available if needed

## Support Resources

- Vercel Documentation: https://vercel.com/docs
- Netlify Documentation: https://docs.netlify.com
- Supabase Documentation: https://supabase.com/docs
- GitHub Repository: https://github.com/Dinesh27-12-2006/intellginece-health-care-portal
