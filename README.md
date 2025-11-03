# Stripe Simplified - Course Management Platform

A full-stack course management platform built with Next.js 16, Convex, Stripe, and Clerk authentication.

## 🚀 Features

- 🔐 **Authentication**: Secure user authentication with Clerk
- 💳 **Payment Processing**: Stripe integration for course purchases and subscriptions
- 📚 **Course Management**: Browse and purchase individual courses
- 🎓 **Pro Plans**: Monthly and yearly subscription options
- 🔄 **Real-time Updates**: Automatic data synchronization with Convex
- 📧 **Email Notifications**: Welcome emails with Resend
- 🛡️ **Rate Limiting**: Built-in rate limiting with Upstash Redis
- 🎨 **Modern UI**: Beautiful interface with Tailwind CSS and shadcn/ui

## 🛠️ Tech Stack

- **Framework**: [Next.js 16](https://nextjs.org/) (App Router)
- **Database & Backend**: [Convex](https://convex.dev/)
- **Authentication**: [Clerk](https://clerk.com/)
- **Payments**: [Stripe](https://stripe.com/)
- **Email**: [Resend](https://resend.com/)
- **Rate Limiting**: [Upstash Redis](https://upstash.com/)
- **Styling**: [Tailwind CSS](https://tailwindcss.com/) + [shadcn/ui](https://ui.shadcn.com/)
- **Language**: TypeScript

## 📋 Prerequisites

- Node.js 18+ installed
- npm/yarn/pnpm/bun package manager
- Accounts on:
  - [Clerk](https://clerk.com/)
  - [Convex](https://convex.dev/)
  - [Stripe](https://stripe.com/)
  - [Resend](https://resend.com/)
  - [Upstash](https://upstash.com/)

## 🔧 Installation

1. **Clone the repository**
```bash
git clone https://github.com/TanMinhNgo/stripe-masterclass
cd stripe-simplified
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**

Create a `.env.local` file in the root directory:

```bash
# Convex
CONVEX_DEPLOYMENT=your-deployment-name
NEXT_PUBLIC_CONVEX_URL=https://your-deployment.convex.cloud

# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_xxx
CLERK_SECRET_KEY=sk_test_xxx
CLERK_JWT_ISSUER_DOMAIN=https://your-domain.clerk.accounts.dev
CLERK_WEBHOOK_SECRET=whsec_xxx

# Stripe
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLIC_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
STRIPE_MONTHLY_PRICE_ID=price_xxx
STRIPE_YEARLY_PRICE_ID=price_xxx

# Upstash Redis
UPSTASH_REDIS_REST_URL=https://xxx.upstash.io
UPSTASH_REDIS_REST_TOKEN=xxx

# App
NEXT_PUBLIC_APP_URL=http://localhost:3000

# Resend
RESEND_API_KEY=re_xxx
```

4. **Set up Convex environment variables**
```bash
npx convex env set NEXT_PUBLIC_APP_URL http://localhost:3000
npx convex env set STRIPE_SECRET_KEY sk_test_xxx
npx convex env set STRIPE_MONTHLY_PRICE_ID price_xxx
npx convex env set STRIPE_YEARLY_PRICE_ID price_xxx
npx convex env set CLERK_WEBHOOK_SECRET whsec_xxx
npx convex env set UPSTASH_REDIS_REST_URL https://xxx.upstash.io
npx convex env set UPSTASH_REDIS_REST_TOKEN xxx
npx convex env set RESEND_API_KEY re_xxx
```

## 🏃 Running the Application

1. **Start Convex development server**
```bash
npx convex dev
```

2. **Start Next.js development server** (in a new terminal)
```bash
npm run dev
```

3. **Open the app**
   
   Navigate to [http://localhost:3000](http://localhost:3000)

## 🔗 Webhook Configuration

### Clerk Webhook

1. Go to [Clerk Dashboard](https://dashboard.clerk.com/) → Webhooks
2. Add endpoint: `https://your-convex-deployment.convex.site/clerk-webhook`
3. Subscribe to events: `user.created`
4. Copy the signing secret to `CLERK_WEBHOOK_SECRET`

### Stripe Webhook

1. Go to [Stripe Dashboard](https://dashboard.stripe.com/webhooks)
2. Add endpoint: `http://localhost:3000/api/webhooks/stripe` (for local testing)
3. Subscribe to events:
   - `checkout.session.completed`
   - `invoice.payment_succeeded`
4. Copy the signing secret to `STRIPE_WEBHOOK_SECRET`

**For production**: Use your production URL instead of localhost

## 📁 Project Structure

```
stripe-simplified/
├── app/                    # Next.js app directory
│   ├── api/               # API routes
│   │   └── webhooks/      # Webhook handlers
│   ├── courses/           # Course pages
│   ├── pro/               # Pro subscription page
│   └── page.tsx           # Home page
├── convex/                # Convex backend
│   ├── courses.ts         # Course queries/mutations
│   ├── purchases.ts       # Purchase logic
│   ├── subscriptions.ts   # Subscription logic
│   ├── users.ts           # User management
│   ├── stripe.ts          # Stripe integration
│   └── http.ts            # HTTP routes
├── components/            # React components
├── lib/                   # Utility libraries
│   ├── stripe.ts          # Stripe client
│   ├── resend.ts          # Resend client
│   └── ratelimit.ts       # Rate limiting
└── emails/                # Email templates
```

## 🧪 Testing Payments

Use Stripe test cards:
- **Success**: `4242 4242 4242 4242`
- **Decline**: `4000 0000 0000 0002`
- **Any future date for expiry, any 3 digits for CVC**

## 📦 Key Dependencies

```json
{
  "next": "^15.0.0",
  "convex": "latest",
  "@clerk/nextjs": "latest",
  "stripe": "latest",
  "@upstash/ratelimit": "latest",
  "resend": "latest",
  "react-email": "latest"
}
```

## 🚀 Deployment

### Deploy to Vercel

1. Push code to GitHub
2. Import project in [Vercel](https://vercel.com)
3. Add all environment variables
4. Deploy!

### Update Convex deployment

```bash
npx convex deploy
```

### Update webhooks

- Update Clerk webhook URL to production Convex URL
- Update Stripe webhook URL to production domain

## 📝 Available Scripts

```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run ESLint
npx convex dev       # Start Convex dev server
npx convex deploy    # Deploy Convex functions
```

## 🐛 Common Issues

### "Missing required headers" error
- Ensure Clerk webhook secret is correctly set
- Check that webhook URL is accessible

### "Connect Timeout Error"
- Ensure `npx convex dev` is running
- Check `NEXT_PUBLIC_CONVEX_URL` in `.env.local`

### "No such price" error
- Verify you're using Price IDs (start with `price_`), not Product IDs
- Check Stripe Dashboard for correct Price IDs

### "Missing courseId or stripeCustomerId"
- Ensure webhook handler differentiates between course purchases and subscriptions
- Check Stripe metadata is properly set

## 📚 Learn More

- [Next.js Documentation](https://nextjs.org/docs)
- [Convex Documentation](https://docs.convex.dev)
- [Clerk Documentation](https://clerk.com/docs)
- [Stripe Documentation](https://stripe.com/docs)

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 🙏 Acknowledgments

Built with amazing tools:
- Next.js team for the incredible framework
- Convex for seamless backend
- Clerk for authentication
- Stripe for payment processing
- Vercel for hosting

---

Made with ❤️ by [Tan Minh Ngo](https://github.com/TanMinhNgo)
